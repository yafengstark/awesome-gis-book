数据库对多维数据的存取有两种索引方案，R-Tree和GiST（Generalized Search Tree），在PostgreSQL中的GiST比R-Tree的健壮性更好，因此PostGIS对空间数据的索引一般采用GiST实现。



以下的语句给sde模式中的cities表添加了一个空间索引shape_index_cities



```sql
 CREATE INDEX shape_index_cities
 ON sde.cities
 USING gist
(shape);

```



1. 另外要注意的是，空间索引只有在进行基于边界范围的查询时才起作用，比如“&&”操作。

