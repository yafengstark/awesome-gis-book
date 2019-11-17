```plpgsql
SELECT postgis_full_version();
```

查看PostGIS的版本信息。








### 在PostGIS中创建一个空间表

在PostGIS中创建一个包含几何字段的空间表分为2步：

- 第一步创建一个一般表；
- 第二步给这个表添加几何字段。


以下先在test模式下创建一个名为cities的一般表：
```sql
create table test.cities (id int4, name varchar(20))
```

再给cities添加一个名为shape的几何字段（二维点）：
```sql
select AddGeometryColumn('test', 'cities', 'shape', 4326, 'POINT', 2)
```



### **对几何信息的检查**

```plsql
select IsValid('LINESTRING(0 0, 1 1)'), IsValid('LINESTRING(0 0,0 0)')
```

select ST_IsValid('LINESTRING(0 0, 1 1)');



默认PostGIS并不会使用IsValid函数检查用户插入的新数据，因为这会消耗较多的CPU资源（特别是复杂的几何对象）。当你需要使用这个功能的时候，你可以使用以下语句为表新建一个约束：

```plsql
ALTER TABLE citiesADD CONSTRAINT geometry_validCHECK (IsValid(shape))
```

这时当我们往这个表试图插入一个错误的空间对象的时候，会得到一个错误：

```plsql
INSERT INTO test.cities ( shape, name )VALUES ( GeomFromText('LINESTRING(0 0,0 0)', 4326), '北京');

ERROR: new row for relation “cities” violates check constraint “geometry_valid”SQL 状态: 23514
```






