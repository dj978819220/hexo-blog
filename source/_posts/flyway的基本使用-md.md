---
title: flyway的基本使用
date: 2018-04-02 22:42:11
tags:
	- flyway
	- 数据库管理
categories:
	- 数据库
---

## 今天我们来看看flyway如何去使用

> flyway 是一款数据库软件支持主流的数据的版本管理，对于多个数据库可以进行同步更新，今天我们来看看具体如何使用


## 准备

[下载flyway命令行工具](https://pan.baidu.com/s/1COkuNKMxxykRzWCTaLhkPg)

## 命令

* flyway migrate

> 此命令会自动检查数据库脚本是否有变化，如果有变化，则执行脚本，更新数据库版本，如果数据库初始状态是空库，则会自动创建schema_version 表，用于存储数据库操作的版本记录，
只要数据库脚本有变化，都需要执行此命令。

* flyway clean

> 清除schema_version中记录所有表结构，视图，存储过程，函数以及所有的数据等都会被清除。

* flyway info

> 打印schema_version中记录信息

* flyway validate

> Validate是指验证已经Apply的Migrations是否有变更，Flyway是默认是开启验证的。

* flyway baseline

> Baseline 是指数据库非空状态下使用flyway首先执行的命令，用于创建schema_vision表。

* flyway repair

> Repair会修复Metadata表的错误，通常有两种用途：


`注意`: 加上-X显示详情 如: `flyway -X info`


## 数据库支持类型

Oracle：10g及更新所有版本（包括 Amazon RDS）
SQL Server：2008及更新版本（包括 Amazon RDS）
SQL Azure：最新版本
MySQL：5.1及更新版本（包括 Amazon RDS & Google Cloud SQL）
MariaDB：10.0及更新版本（包括 Amazon RDS）
Phoenix：4.2.2及更新版本
PostgreSQL：9.0及更新版本（包括 Heroku & Amazon RDS）
Vertica：6.5及更新版本
AWS Redshift：最新版本
DB2：9.7及更新版本
DB2 z/OS：9.1及更新版本
Derby：10.8.2.2及更新版本
H2：1.2.137及更新版本
Hsql：1.8及更新版本
SQLite：3.7.2及更新版本
SAP HANA：最新版本
solidDB：6.5及更新版本
Sybase ASE：12.5及更新版本


## 属性配置

flyway.driver=org.hsqldb.jdbcDriver
flyway.url=jdbc:hsqldb:file:/db/flyway_sample
flyway.user=SA
flyway.password=mySecretPwd
flyway.schemas=schema1,schema2,schema3
flyway.table=schema_history
flyway.locations=classpath:com.mycomp.migration,database/migrations,filesystem:/sql-migrations
flyway.sqlMigrationPrefix=Migration-
flyway.sqlMigrationSeparator=__
flyway.sqlMigrationSuffix=-OK.sql
flyway.encoding=ISO-8859-1
flyway.placeholderReplacement=true
flyway.placeholders.aplaceholder=value
flyway.placeholders.otherplaceholder=value123
flyway.placeholderPrefix=#[
flyway.placeholderSuffix=]
flyway.resolvers=com.mycomp.project.CustomResolver,com.mycomp.project.AnotherResolver
flyway.callbacks=com.mycomp.project.CustomCallback,com.mycomp.project.AnotherCallback
flyway.target=5.1
flyway.outOfOrder=false
flyway.validateOnMigrate=true
flyway.cleanOnValidationError=false
flyway.baselineOnMigrate=false


## 实例介绍，这里我以mysql为例

* 第一步：建立两个测试数据库 demo1, demo2

```sql
create database demo1;
create database demo2;
```


![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/88138324.jpg)


* 第二步配置 \flyway-5.0.7\conf\flyway.conf

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/25711198.jpg)

* 第三步：生成基本版本

`flyway -X baseline`

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/81337188.jpg)

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/33626480.jpg)

* 第四步: 编写版本控制sql,在图中位子写下这三个控制脚本

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/27884726.jpg)

- V1.1__addTable.sql

```sql
create table testA(
	id int
)
```

- V2.0__addTable.sql

```sql
create table testB(
	id int
)
```

- V3.0__dropTable.sql

```sql
drop table testA;
```

`注意: 是两个下划线__`

* 执行 flyway migrate

> 如下图是不是只剩下B表了

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/27884726.jpg)

## 其他数据库做同步工作好了看好

* 替换数据库链接，密码注意保持和想同步的数据库一致，执行`flyway migrate`

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/43800477.jpg)

* 看Demo2是不是立马和Demo1一样了

![](http://otxoa8jsq.bkt.clouddn.com/18-4-2/47506138.jpg)







