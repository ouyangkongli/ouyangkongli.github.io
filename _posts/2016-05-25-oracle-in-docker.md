---
layout: post
title: 在Docker上部署Oracle
description: 总结近期本人面试遇到的典型题目
tag: [Docker, oracle]
comments: true
categories: Docker
---

近期由于工作的需求，需要补充一下Oracle DB的知识；

oracle-xe-11g是Oracle推出的一款免费产品，可惜只有rpm包；
由于习惯于使用界面更加友好的Ubuntu，但是Google了很久，在Ubuntu上安装oracle-xe-11g确实是件痛苦的事情。另外，为了趁此机会学习一下时髦的Docker，决定使用docker-oracle-xe-11g。

## 安装Docker
* 环境：Ubuntu Xenial 16.04 (LTS)

### 更新apt sources
> sudo apt-get update

> sudo apt-get install apt-transport-https ca-certificates 

> sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D




<!-- more -->
