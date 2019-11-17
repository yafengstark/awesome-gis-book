OGC定义了两种描述几何对象的格式，分别是WKB（Well-Known Binary）和WKT（Well-Known Text）。
在SQL语句中，用以下的方式可以使用WKT格式定义几何对象：



几何要素	WKT格式
点	POINT(0 0)
线	LINESTRING(0 0,1 1,1 2)
面	POLYGON((0 0,4 0,4 4,0 4,0 0),(1 1, 2 1, 2 2, 1 2,1 1))
多点	MULTIPOINT(0 0,1 2)
多线	MULTILINESTRING((0 0,1 1,1 2),(2 3,3 2,5 4))
多面	MULTIPOLYGON(((0 0,4 0,4 4,0 4,0 0),(1 1,2 1,2 2,1 2,1 1)), ((-1 -1,-1 -2,-2 -2,-2 -1,-1 -1)))
几何集合	GEOMETRYCOLLECTION(POINT(2 3),LINESTRING((2 3,3 4)))
————————————————
https://blog.csdn.net/xk_zhang/article/details/52014737