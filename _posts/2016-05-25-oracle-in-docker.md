---
layout: post
title: 在Docker上部署Oracle
description: 在Docker上部署Oracle
tag: [Docker, oracle]
comments: true
categories: Docker
---

近期由于工作的需求，需要补充一下Oracle DB的知识；

oracle-xe-11g是Oracle推出的一款免费产品，可惜只有rpm包；
由于习惯于使用界面更加友好的Ubuntu，但是Google了很久，在Ubuntu上安装oracle-xe-11g确实是件痛苦的事情。另外，为了趁此机会学习一下时髦的Docker，决定使用docker-oracle-xe-11g。

## 安装Docker
* 环境：Ubuntu Xenial 16.04 (LTS)

可参考Docker官方文档：
[Ubuntu上安装Docker](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

## 安装docker-oracle-xe-11g

> docker pull wnameless/oracle-xe-11g
>
> docker run -d -p 49160:22 -p 49161:1521 -e ORACLE_ALLOW_REMOTE=true wnameless/oracle-xe-11g

已安装的oracle配置如下：

    hostname: localhost
    port: 49161
    sid: xe
    username: system
    password: oracle
    
1. SYS & SYSTEM 密码： oracle

2. SSH登陆： `ssh root@localhost -p 49160`  密码： `admin`

详细文档见：[https://hub.docker.com/r/wnameless/oracle-xe-11g/](https://hub.docker.com/r/wnameless/oracle-xe-11g/)


<!-- more -->
