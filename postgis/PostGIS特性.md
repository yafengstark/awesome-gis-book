1.  PostGIS支持所有的空间数据类型，这些类型包括:点（POINT）、线（LINESTRING）、多边形（POLYGON）、多点（MULTIPOINT）、多线（MULTILINESTRING）、多多边形（MULTIPOLYGON）和集合对象集（GEOMETRYCOLLECTION）等。PostGIS支持所有的对象表达方法，比如WKT和WKB。
2. PostGIS支持所有的数据存取和构造法，GeomFromText()、AsBinary()，以及GeometryN()等。
3.  PostGIS提供简单的空间分析函数（如Area和Length）同时也提供其他一些具有复杂分析功能的函数，比如Distance。
4.  PostGIS提供了对于元数据的支持，如GEOMETRY_COLUMNS和SPATIAL_REF_SYS，同时，PostGIS也提供了相应的支持函数，如AddGeometryColumn和DropGeometryColumn。`
5. PostGIS提供了一系列的二元谓词（如Contains、Within、Overlaps和Touches）用于检测空间对象之间的空间关系，同时返回布尔值来表征对象之间符合这个关系。
6.  PostGIS提供了空间操作符（如Union和Difference）用于空间数据操作。比如，Union操作符融合多边形之间的边界。两个交迭的多边形通过Union运算就会形成一个新的多边形，这个新的多边形的边界为两个多边形中最大边界。

1.  数据库坐标变换`
 数据库中的几何类型可以通过Transform函数从一种投影系变换到另一种投影系中。在OpenGIS中的几何类型都将SRID作为自身结构的一部分，但不知什么原因，在OpenGIS的SFSQL规范中，并没有引入Transform。`
 
 ### 球体长度运算
8. 存储在普通地理坐标系中的集合类型如果不进行坐标变换是无法进行程度运算的，OpenGIS所提供的坐标变换使得积累类型的程度计算变成可能。

   ### 三维的几何类型
9. SFSQL规范只是针对二维集合类型。OpenGIS提供了对三维集合类型的支持，具体是利用输入的集合类型维数来决定输出的表现方式。例如，即便所有几何对象内部都以三维形式存储，纯粹的二维交叉点通常还是以二维的形式返回。此外，还提供几何对象在不同维度间转换的功能。`

   ### 空间聚集函数
10. `    在数据库中，聚集函数是一个执行某一属性列所有数据操作的函数。比如Sum和Average，Sum是求某一关系属性列的数据总和，Average则是求取某一关系属性列的数据平均值。与此对应，空间聚集函数也是执行相同的操作，不过操作的对象是空间数据。例如聚集函数Extent返回一系列要素中的最大的包裹矩形框，如“SELECT EXTENT(GEOM) FROM ROADS”这条SQL语句的执行结果是返回ROADS这个数据表中所有的包裹矩形框。`

   ###  栅格数据类型

     PostGIS通过一种新的数据类型片，提供对于大的栅格数据对象的存储。片由以下几个部分组成:包裹矩形框、SRID、类型和一个字节序列。通过将片的大小控制在数据库页值（32×32）以下，使得快速的随即访问变成可能。一般大的图片也是通过将其切成32×32像素的片然后再存储在数据库中的。