首先需要说明一下，这里许多函数是以ST_[X]yyy形式命名的，事实上很多函数也可以通过xyyy的形式访问，在PostGIS的函数库中我们可以看到这两种函数定义完全一样。

**空间函数**中的大部分可以被归纳为以下五类：

- 转换    ——    在geometry（PostGIS中存储空间信息的格式）和外部数据格式之间进行转换的函数
- 管理    ——    管理关于空间表和PostGIS组织的信息的函数
- 检索    ——    检索几何图形的属性和空间信息测量的函数
- 比较    ——    比较两种几何图形的空间关系的函数
- 生成    ——    基于其他几何图形生成新图形的函数
- 

# PostGIS中的常用函数



OGC标准函数
管理函数：函数	说明

- AddGeometryColumn(, , , , , )
> 添加几何字段

- DropGeometryColumn(, , )	
> 删除几何字段

- Probe_Geometry_Columns()	
> 检查数据库几何字段并在geometry_columns中归档

- ST_SetSRID(geometry, integer)	
> 给几何对象设置空间参考（在通过一个范围做空间查询时常用）



### 几何对象关系函数：

函数	说明
ST_Distance(geometry, geometry)	获取两个几何对象间的距离
ST_DWithin(geometry, geometry, float)	如果两个几何对象间距离在给定值范围内，则返回TRUE
ST_Equals(geometry, geometry)	判断两个几何对象是否相等（比如LINESTRING(0 0, 2 2)和LINESTRING(0 0, 1 1, 2 2)是相同的几何对象）
ST_Disjoint(geometry, geometry)	判断两个几何对象是否分离
ST_Intersects(geometry, geometry)	判断两个几何对象是否相交
ST_Touches(geometry, geometry)	判断两个几何对象的边缘是否接触
ST_Crosses(geometry, geometry)	判断两个几何对象是否互相穿过
ST_Within(geometry A, geometry B)	判断A是否被B包含
ST_Overlaps(geometry, geometry)	判断两个几何对象是否是重叠
ST_Contains(geometry A, geometry B)	判断A是否包含B
ST_Covers(geometry A, geometry B)	判断A是否覆盖 B
ST_CoveredBy(geometry A, geometry B)	判断A是否被B所覆盖
ST_Relate(geometry, geometry, intersectionPatternMatrix)	通过DE-9IM 矩阵判断两个几何对象的关系是否成立
ST_Relate(geometry, geometry)	获得两个几何对象的关系（DE-9IM矩阵）
————————————————

### 几何对象处理函数：

函数	说明
ST_Centroid(geometry)	获取几何对象的中心
ST_Area(geometry)	面积量测
ST_Length(geometry)	长度量测
ST_PointOnSurface(geometry)	返回曲面上的一个点
ST_Boundary(geometry)	获取边界
ST_Buffer(geometry, double, [integer])	获取缓冲后的几何对象
ST_ConvexHull(geometry)	获取多几何对象的外接对象
ST_Intersection(geometry, geometry)	获取两个几何对象相交的部分
ST_Shift_Longitude(geometry)	将经度小于0的值加360使所有经度值在0-360间
ST_SymDifference(geometry A, geometry B)	获取两个几何对象不相交的部分（A、B可互换）
ST_Difference(geometry A, geometry B)	从A去除和B相交的部分后返回
ST_Union(geometry, geometry)	返回两个几何对象的合并结果
ST_Union(geometry set)	返回一系列几何对象的合并结果
ST_MemUnion(geometry set)	用较少的内存和较长的时间完成合并操作，结果和ST_Union
————————————————
版权声明：本文为CSDN博主「学大大」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xk_zhang/article/details/52014737



### 几何对象存取函数：

函数	说明
ST_AsText(geometry)	获取几何对象的WKT描述
ST_AsBinary(geometry)	获取几何对象的WKB描述
ST_SRID(geometry)	获取几何对象的空间参考ID
ST_Dimension(geometry)获取几何对象的维数	
ST_Envelope(geometry)	获取几何对象的边界范围
ST_IsEmpty(geometry)	判断几何对象是否为空
ST_IsSimple(geometry)	判断几何对象是否不包含特殊点（比如自相交）
ST_IsClosed(geometry)	判断几何对象是否闭合
ST_IsRing(geometry)	判断曲线是否闭合并且不包含特殊点
ST_NumGeometries(geometry)	获取多几何对象中的对象个数
ST_GeometryN(geometry,int)	获取多几何对象中第N个对象
ST_NumPoints(geometry)	获取几何对象中的点个数
ST_PointN(geometry,integer)	获取几何对象的第N个点
ST_ExteriorRing(geometry)	获取多边形的外边缘
ST_NumInteriorRings(geometry)	获取多边形内边界个数
ST_NumInteriorRing(geometry)	(同上)
ST_InteriorRingN(geometry,integer)	获取多边形的第N个内边界
ST_EndPoint(geometry)	获取线的终点
ST_StartPoint(geometry)	获取线的起始点
ST_GeometryType(geometry)	获取几何对象的类型
ST_GeometryType(geometry)	类似上，但是不检查M值，即POINTM对象会被判断为point
ST_X(geometry)	获取点的X坐标
ST_Y(geometry)	获取点的Y坐标
ST_Z(geometry)	获取点的Z坐标
ST_M(geometry)	获取点的M值
————————————————
版权声明：本文为CSDN博主「学大大」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xk_zhang/article/details/52014737



### 几何对象构造函数：

参考语义：
Text：WKT
WKB：WKB
Geom:Geometry
M:Multi
Bd:BuildArea
Coll:Collection ST_GeomFromText(text,[])

## PostGIS扩展函数
### 管理函数：

删除一个空间表（包括geometry_columns中的记录） DropGeometryTable([], )
更新空间表的空间参考 UpdateGeometrySRID([], , , )
更新空间表的统计信息 update_geometry_stats([, ])

参考语义：
Geos：GEOS库
Jts：JTS库
Proj：PROJ4库 postgis_version()

### 几何操作符：

函数	说明
A范围=B范围	A = B
A范围覆盖B范围或A范围在B范围左侧	A &<> B
A范围在B范围左侧	A <<>> B
A范围覆盖B范围或A范围在B范围下方	A &<|B
A范围覆盖B范围或A范围在B范围上方	A |&> B
A范围在B范围下方	A <<| B
A范围在B范围上方	A |>> B
A=B	A ~= B
A范围被B范围包含	A @ B
A范围包含B范围	A ~ B
A范围覆盖B范围	A && B

### 几何量测函数：

函数	说明
ST_Area(geometry)	量测面积
ST_distance_sphere(point, point)	根据经纬度点计算在地球曲面上的距离，单位米，地球半径取值6370986米
ST_distance_spheroid(point, point, spheroid)	类似上，使用指定的地球椭球参数
ST_length2d(geometry)	量测2D对象长度
ST_length3d(geometry)	量测3D对象长度
ST_length_spheroid(geometry,spheroid)	根据经纬度对象计算在地球曲面上的长度
ST_distance(geometry, geometry)	量测两个对象间距离
ST_max_distance(linestring,linestring)	量测两条线之间的最大距离
ST_perimeter2d(geometry)	量测2D对象的周长
ST_perimeter3d(geometry)	量测3D对象的周长
ST_azimuth(geometry, geometry)	量测两点构成的方位角，单位弧度

### 几何对象输出：

函数	说明
ST_AsBinary(geometry,{‘NDR’|’XDR’})	Binary
ST_AsEWKT(geometry)	EWKT
ST_AsEWKB(geometry, {‘NDR’|’XDR’})	EWKB
ST_AsHEXEWKB(geometry, {‘NDR’|’XDR’})	Canonical
ST_AsSVG(geometry, [rel], [precision])	SVG
ST_AsGML([version], geometry, [precision])	GML
ST_AsKML([version], geometry, [precision])	KML
ST_AsGeoJson([version], geometry, [precision], [options])	GeoJson

### 几何对象创建：

ST_GeomFromEWKB(bytea)；ST_MakePoint(, , [], [])；ST_MakePointM(, , )；ST_MakeBox2D(, )；ST_MakeBox3D(, )；ST_MakeLine(geometry set)；ST_MakeLine(geometry, geometry)
ST_LineFromMultiPoint(multipoint)；ST_MakePolygon(linestring, [linestring[]])；ST_BuildArea(geometry)；ST_Polygonize(geometry set)；ST_Collect(geometry set)；ST_Collect(geometry, geometry)
ST_Dump(geometry)；ST_DumpRings(geometry)

### 几何对象编辑：

线性参考：
根据location（0-1）获得该位置的点 ST_line_interpolate_point(linestring, location)

获取一段线 ST_line_substring(linestring, start, end)

根据点获取location（0-1） ST_line_locate_point(LineString, Point)

根据量测值获得几何对象 ST_locate_along_measure(geometry, float8)

根据量测值区间获得几何对象集合 ST_locate_between_measures(geometry, float8, float8)

杂项功能函数：
长事务支持：
————————————————
版权声明：本文为CSDN博主「学大大」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xk_zhang/article/details/52014737