WFS 标准
定义了一些操作，这些操作允许用户在分布式的环境下通过 HTTP 对空间数据迚
行查询、编辑等操作。



顾名思义，WFS是通过网络操作Feature的服务。



它支持客户端对服务器的Feature执行INSERT, UPDATE, DELETE, LOCK, QUERY，DISCOVERY操作。乍一看感觉WFS像是一个数据库。那么是不是还有类似于SQL的东东呢，确实是这样，关于这个主题我们会在随后讨论。



先来说说Feature与FeatureType。我们说过Feature是对现实抽象的基本元素。它把现实物体抽象为属性数据和地理数据，然后再加上一个Id来标识。许多同类的Feature往往会要求统一处理，对他们分组就显得有必要了。于是Layer出现了，一个Layer就是一组具有相同属性结构的Feature（这是我的理解）。很多系统中同一个Layer里面的Feature还必须具有相同的几何类型，例如都是Point或者都是Polygon。Layer还有一个的Style，这样Feature就会以一致的方式渲染。



OGC有一个" [简单对象访问协议 ](http://www.opengeospatial.org/standards/sfa)"，里面有很完整的关于GIS [对象建模 ](http://www.opengeospatial.org/standards/sfo)和 [数据库建模 ](http://www.opengeospatial.org/standards/sfs)的设计指导，是相当不错的学习资料



## 很重要的概念Filter

我们可以把Filter看做SQL语句中"where"的部分





## Transaction



Transaction，熟悉数据库的朋友对这个词都不陌生，事务。WFS修改数据的方法就是"提交事务"





和数据库一样，Transaction提供Insert，Update和Delete操作。





## 参考

[开放GIS标准OGC之路（3）之 WFS初探](https://blog.csdn.net/hnzhangshilong/article/details/6898622)