[docker 部署带postgis扩展的postgresql](https://blog.csdn.net/geol200709/article/details/89481194)



[docker安装PostgreSQL](https://blog.csdn.net/fwk19840301/article/details/80613800)

[使用 docker 提供 PostgreSQL＋PostGIS 服务]()




```shell
docker run --name postgis -e POSTGRES_PASSWORD=12345666 -p 5432:5432 -d kartoza/postgis
```



默认用户名postgres





