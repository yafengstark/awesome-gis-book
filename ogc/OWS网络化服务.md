OWS(OGC Web Service Common Implementation Specification)当 前版本是 2.0.0。

OWS 描述了 Web 服务通用的一些接口规范，包括请求和响应 的内容、请求的参数和编码等。



目前，OWS 包括 WFS、WMS、WCS，

- WFS-要素 Web 服务
- WMS-地图 Web 服务
- WCS- 栅格 Web 服务



每个 OWS 服务都包括 GetCapabilities 操作，这个操作返回这个服务的元
数据信息。



> 每个geoserver实例就会有一个GetCapabilities