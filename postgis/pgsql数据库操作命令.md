



### 

- 连接数据库, 默认的用户和数据库是postgres

```
psql -U user -d dbname
```

切换数据库,相当于mysql的use dbname

```
\c dbname
```


列举数据库，相当于mysql的show databases

```
\l
```



列举表，相当于mysql的show tables

```
\dt
```




查看表结构，相当于desc tblname,show columns from tbname

```
\d tblname
```





\di 查看索引 

创建数据库： 
create database [数据库名]; 
删除数据库： 
drop database [数据库名];  
*重命名一个表： 
alter table [表名A] rename to [表名B]; 
*删除一个表： 
drop table [表名]; 

*在已有的表里添加字段： 
alter table [表名] add column [字段名] [类型]; 
*删除表中的字段： 
alter table [表名] drop column [字段名]; 
*重命名一个字段：  
alter table [表名] rename column [字段名A] to [字段名B]; 
*给一个字段设置缺省值：  
alter table [表名] alter column [字段名] set default [新的默认值];
*去除缺省值：  
alter table [表名] alter column [字段名] drop default; 
在表中插入数据： 
insert into 表名 ([字段名m],[字段名n],......) values ([列m的值],[列n的值],......); 
修改表中的某行某列的数据： 
update [表名] set [目标字段名]=[目标值] where [该行特征]; 
删除表中某行数据： 
delete from [表名] where [该行特征]; 
delete from [表名];--删空整个表 
创建表： 
create table ([字段名1] [类型1] <references 关联表名(关联的字段名)>;,[字段名2] [类型2],......<,primary key (字段名m,字段名n,...)>;); 

\copyright     显示 PostgreSQL 的使用和发行条款
\encoding [字元编码名称]
                 显示或设定用户端字元编码
\h [名称]      SQL 命令语法上的说明，用 * 显示全部命令
\prompt [文本] 名称
                 提示用户设定内部变数
\password [USERNAME]
                 securely change the password for a user
\q             退出 psql



可以使用pg_dump和pg_dumpall来完成。比如备份sales数据库： 
pg_dump drupal>/opt/Postgresql/backup/1.bak