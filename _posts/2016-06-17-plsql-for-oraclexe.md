---
layout: post
title: pl/sql developer连接远程oracle
description:  pl/sql developer连接远程oracle
tag: [pl/sql, oracle]
comments: true
categories: oracle
---

前段时间在docker部署了oracle-xe-11g，[ [在Docker上部署Oracle](https://ouyangkongli.github.io/2016/05/25/oracle-in-docker)].
现在介绍一下，使用PL/SQL Developer 连接docker上的oracle。

<!-- more -->

### 安装Instant Client

#### 下载安装

[Instant Client](http://www.oracle.com/technetwork/cn/database/features/instant-client/index-092699-zhs.html) 这里安装的是Instant Client， 而不是庞大的Oracle Client，下载后解压到目录`%OracleClient%`。


#### 配置 

在`%OracleClient%`内创建 \network\admin\，再在admin下创建tnsnames.ora和sqlnet.ora文件(从oracle上拷贝也可)。

docker上的tnsnames.ora 和sqlnet.ora在目录`/u01/app/oracle/product/11.2.0/xe/network/admin`.

**[tnsnames.ora](/resources/tnsnames.ora "tnsnames.ora")**

**[sqlnet.ora](/resources/sqlnet.ora "sqlnet.ora")**




### 安装PL/SQL Development

下载安装后，运行-->取消；进入PL/SQL Development 配置，设置Connection. 这里`%OracleClient%` 为D:\oracle\instantclient_11_2  .



![CCNode](/images/oracle/plsql.PNG)  


重新启动PL/SQL Development  

![plsql-login.jpg](/images/oracle/plsql-login.jpg)

