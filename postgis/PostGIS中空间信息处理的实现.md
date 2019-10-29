

## spatial_ref_sys表
基于PostGIS模板创建的数据库的public模式下，有一个spatial_ref_sys表，它存放的是OGC规范的空间参考。
srid存放的就是空间参考的Well-Known ID，对这个空间参考的定义主要包括两个字段，srtext存放的是以字符串描述的空间参考，proj4text存放的则是以字符串描述的PROJ.4 投影定义（PostGIS使用PROJ.4实现投影）。


