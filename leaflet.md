

## 友链
[leaflet常用插件地址整理](https://zhuanlan.zhihu.com/p/35713983)

## 功能
迁移图

热力图（支持缩放的）

[marker动态聚合](https://leaflet.github.io/Leaflet.markercluster/example/marker-clustering-realworld.388.html)

[一些demo 有源码](https://juejin.im/post/5a658614f265da3e3f4cce0e)




### 坐标系

leaflet竟然不支持tms指定crs,只能通过切换整个地图的坐标系来支持wgs84切片。

也即标准tms不能与非标准tms共融



### leaflet与angular

[Use Leaflet Plugin In Angular](https://codehandbook.org/use-leaflet-in-angular/) 亲测可用


---
## map
先lat, 后lon


### 底图

==注意
Whenever using anything based on OpenStreetMap, an attribution is obligatory as per the copyright notice. Most other tile providers (such as MapBox, Stamen or Thunderforest) require an attribution as well. Make sure to give credit where credit is due.== 也就是都要带上权限信息

* 加载mapbox底图（来自leaflet官网的默认底图）
[mapbox tile](https://api.tiles.mapbox.com/v4/mapbox.streets/15/16376/10894.png?access_token=pk.eyJ1IjoibWFwYm94IiwiYSI6ImNpejY4NXVycTA2emYycXBndHRqcmZ3N3gifQ.rJcFIG214AriISLbB6B5aw)

```js
L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
    maxZoom: 18,
    id: 'mapbox.streets',
    accessToken: 'your.mapbox.access.token'
}).addTo(mymap);
```

id = mapbox.satellite代表遥感图


* 加载osm底图（web mecator)
```js
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
}).addTo(map);
```

## 代码片段
* 加载不同坐标系
```js
const map = L.map('mapTwo',{
      crs: L.CRS.EPSG3857
    }).setView([121.505, 30], 3);
```

* 添加marker
```js
L.marker([51.5, -0.09]).addTo(map)
    .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
    .openPopup();
```

* 添加圆形
```js
var circle = L.circle([51.508, -0.11], {
    color: 'red',
    fillColor: '#f03',
    fillOpacity: 0.5,
    radius: 500
}).addTo(mymap);
```
* 添加多边形
```js
var polygon = L.polygon([
    [51.509, -0.08],
    [51.503, -0.06],
    [51.51, -0.047]
]).addTo(mymap);
```
* 添加弹窗

openPopUp(): 自动弹出
```js
marker.bindPopup("<b>Hello world!</b><br>I am a popup.").openPopup();
circle.bindPopup("I am a circle.");
polygon.bindPopup("I am a polygon.");
```

* openOn : 标记自动弹出
```js
var popup = L.popup()
    .setLatLng([51.5, -0.09])
    .setContent("I am a standalone popup.")
    .openOn(mymap);
```

### 处理事件

```js
var popup = L.popup();
    function onMapClick(e) {
      popup
        .setLatLng(e.latlng)
        .setContent("You clicked the map at " + e.latlng.toString())
        .openOn(map);
    }

    map.on('click', onMapClick);
```

### 自定义标注图标

```js
 let LeafIcon = L.Icon.extend({
      options: {
        shadowUrl: 'assets/image/icon/leaf-shadow.png',
        iconSize: [38, 95],
        shadowSize: [50, 64],
        iconAnchor: [22, 94],
        shadowAnchor: [4, 62],
        popupAnchor: [-3, -76]
      }
    });
    let greenIcon = new LeafIcon({iconUrl: 'assets/image/icon/leaf-green.png'}),
      redIcon = new LeafIcon({iconUrl: 'assets/image/icon/leaf-red.png'}),
      orangeIcon = new LeafIcon({iconUrl: 'assets/image/icon/leaf-orange.png'});

    L.marker([31.231706, 121.472644], {icon: greenIcon}).addTo(this.map).bindPopup("上海市");

```

### 加载geojson
```js
var geojsonFeature = {
    "type": "Feature",
    "properties": {
        "name": "Coors Field",
        "amenity": "Baseball Stadium",
        "popupContent": "This is where the Rockies play!"
    },
    "geometry": {
        "type": "Point",
        "coordinates": [-104.99404, 39.75621] // lon, lat
    }
};

L.geoJSON(geojsonFeature).addTo(map); // 
```


* geojson图层
```js
var myLines = [{
    "type": "LineString",
    "coordinates": [[-100, 40], [-105, 45], [-110, 55]]
}, {
    "type": "LineString",
    "coordinates": [[-105, 40], [-110, 45], [-115, 55]]
}];



var myLayer = L.geoJSON().addTo(map);
myLayer.addData(geojsonFeature);
// 或者
var myStyle = { // 指定json样式
    "color": "#ff7800",
    "weight": 5,
    "opacity": 0.65
};

L.geoJSON(myLines, {
    style: myStyle
}).addTo(map);

```
## 制作交互式地图
* 加载geojson, 不同条件style不同
* 对geojson的feature绑定鼠标事件
* control 控制面板 实现图例


## 使用图层组和图层控制面板
图层分为两种：baselayer 或者overlayer; baselayer是互斥的，overlayer是覆盖的；

* 创建两个互斥的图层
```js
var grayscale = L.tileLayer(mapboxUrl, {id: 'MapID', attribution: mapboxAttribution}),
    streets   = L.tileLayer(mapboxUrl, {id: 'MapID', attribution: mapboxAttribution});

var map = L.map('map', {
    center: [39.73, -104.99],
    zoom: 10,
    layers: [grayscale, cities]
});
```

* 创建layergroup
```js
var littleton = L.marker([39.61, -105.02]).bindPopup('This is Littleton, CO.'),
    denver    = L.marker([39.74, -104.99]).bindPopup('This is Denver, CO.'),
    aurora    = L.marker([39.73, -104.8]).bindPopup('This is Aurora, CO.'),
    golden    = L.marker([39.77, -105.23]).bindPopup('This is Golden, CO.');
    
var cities = L.layerGroup([littleton, denver, aurora, golden]);
```

```js
var baseMaps = {
    "Grayscale": grayscale,
    "Streets": streets
};

var overlayMaps = {
    "Cities": cities
};

L.control.layers(baseMaps, overlayMaps).addTo(map);
```


```js
var baseMaps = {
    "<span style='color: gray'>Grayscale</span>": grayscale,
    "Streets": streets
};
```

## 添加比例尺
```js
 L.control.scale().addTo(map);
 ```
 
 * 控制zoom
 ```js
 map.setZoom(1);
 ```
 * setView
 * flyTo： 平滑过渡
 * zoomIn(delta) in delta个层级 
 * setZoomAround(fixedPoint, zoom), 依据固定点缩放
 

* 控制zoom小步缩放
```js
 var map = L.map('map', {
        zoomSnap: 0.25
    });
```
一次滚轮缩放0.25。默认为1

* Do a box zoom (drag with your mouse while pressing the shift key in your keyboard) ： 使用shift与鼠标画框进行缩放
* 

## 不只是地图，也支持其他平面系统
直角坐标系

游戏

## panes 图层网格
利用z-index 控制图层上下关系，顺序为；
* TileLayers and GridLayers
* Paths, like lines, polylines, circles, or GeoJSON layers.
* Marker shadows （图标影）
* Marker icons
* Popups （在最上面）
* 
但是这个顺序未必总是适合场景的。因此，leaflet提供可以定制化覆盖顺序。

## leaflet加载geoserver的矢量切片，

只做了在线实时切片，先切再加载未做。

效果
* 效果一般
* 速度一般
* 有经纬框，怀疑是geoserver切片的问题。
* 

结论：可能还是客户端模式更好一点。

## 图层操作
'移除'
```shell
map.removeLayer(Layer layer)
```
与control中移除不同


## leaflet样例
(入门Leaflet之小Demo)[https://github.com/liuvigongzuoshi/Leaflet_Demo]


## 自定义图标
[官网示例](https://leafletjs.com/examples/custom-icons/)

## 解决原生 marker的阴影图标marker-shadow.png无法加载

向angular.json 添加assets
```typescript
"assets": [
              "src/favicon.ico",
              "src/marker-shadow.png",
              "src/assets",
              {
                "glob": "**/*",
                "input": "./node_modules/@ant-design/icons-angular/src/inline-svg/",
                "output": "/assets/"
              }
            ]
```

## leaflet地图 无跨域问题
直接取用的img标签，无跨域问题。
但是Cesium有跨域问题。Cesium通过下载贴图方式（猜测是Canvas)