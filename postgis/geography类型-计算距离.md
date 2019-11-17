https://zhuanlan.zhihu.com/p/63173609



使用标准的PostGIS笛卡尔平面坐标系空间函数ST_Distance(geometry, geometry)计算**洛杉矶**和**巴黎**之间的距离。请注意，SRID 4326声明了地理空间参考系统。

```plpgsql
SELECT ST_Distance(
  ST_GeometryFromText('POINT(-118.4079 33.9434)', 4326), -- Los Angeles (LAX)
  ST_GeometryFromText('POINT(2.5559 49.0083)', 4326)     -- Paris (CDG)
);
```



为了计算出真实的距离，我们不能把**地理坐标**近似的看成笛卡尔平面坐标，而应该把它们看成是球坐标。我们必须把点之间的距离作为球面上的真实路径来测量——大圆的一部分。

从1.5版开始，PostGIS通过地理（geography）数据类型提供此功能。



关于上面的测量应该使用geography而不是geometry类型，让我们再次尝试测量**洛杉矶**和**巴黎**之间的距离，我们将使用ST_GeographyFromText(text)函数，而不是ST_GeometryFromText(text)。

```text
SELECT ST_Distance(
  ST_GeographyFromText('POINT(-118.4079 33.9434)'), -- Los Angeles (LAX)
  ST_GeographyFromText('POINT(2.5559 49.0083)')     -- Paris (CDG)
);
```



### **一、使用Geography**

为了将geometry数据加载到geography表中，首先需要将geometry投影到EPSG:4326（经度-longitude/纬度-latitude），然后再将其转换为geography。**ST_Transform(geometry, srid)**函数能将坐标转换为地理坐标，**Geography(geometry)**函数能将geometry转换为geography。



对于geography类型，只有相关的少量空间函数：

- **ST_AsText(geography)** returns `text`
- **ST_GeographyFromText(text)** returns `geography`
- **ST_AsBinary(geography)** returns `bytea`
- **ST_GeogFromWKB(bytea)** returns `geography`
- **ST_AsSVG(geography)** returns `text`
- **ST_AsGML(geography)** returns `text`
- **ST_AsKML(geography)** returns `text`
- **ST_AsGeoJson(geography)** returns `text`
- **ST_Distance(geography, geography)** returns `double`
- **ST_DWithin(geography, geography, float8)** returns `boolean`
- **ST_Area(geography)** returns `double`
- **ST_Length(geography)** returns `double`
- **ST_Covers(geography, geography)** returns `boolean`
- **ST_CoveredBy(geography, geography)** returns `boolean`
- **ST_Intersects(geography, geography)** returns `boolean`
- **ST_Buffer(geography, float8)** returns `geography`[[1\]](https://link.zhihu.com/?target=https%3A//postgis.net/workshops/postgis-intro/geography.html%23casting-note)
- **ST_Intersection(geography, geography)** returns `geography`[[1\]](https://link.zhihu.com/?target=https%3A//postgis.net/workshops/postgis-intro/geography.html%23casting-note)



那么，结论是什么呢？

**如果你的数据在地理范围上是紧凑的（包含在州、县或市内），请使用基于笛卡尔坐标的geometry类型**，这样使你的数据有意义。有关可能的参考系统的选择，请参见[http://spatialreference.org](https://link.zhihu.com/?target=http%3A//spatialreference.org/)站点并输入您所在区域的名称。

**如果你需要测量在地理范围上是分散的数据集（覆盖世界大部分地区）的距离，请使用geography类型。**通过基于geography类型运行而节省的应用程序复杂性将抵消任何性能问题。同时转换为geometry可以抵消大多数功能限制。