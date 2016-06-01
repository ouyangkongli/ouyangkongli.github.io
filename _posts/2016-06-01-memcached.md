---
layout: post
title: Memcached安装与使用
description: Memcached安装与使用
tag: [memcached]
comments: true
categories: memcached
---

Memcached是一个自由开源的，高性能，分布式内存对象缓存系统.

<!-- more -->

## 安装Memcached
* 环境：Ubuntu Xenial 16.04 (LTS)

For Debian or Ubuntu:

> sudo apt-get install memcached

For Redhat/Fedora:

> yum install memcached

从源码安装:

> sudo apt-get install libevent-dev

或

> yun install libevent-devel

    wget https://memcached.org/latest
    [you might need to rename the file]
    tar -zxf memcached-1.x.x.tar.gz
    cd memcached-1.x.x
    ./configure --prefix=/usr/local/memcached
    make && make test && sudo make install

### 参考资料
+ [https://github.com/memcached/memcached/wiki/Install](https://github.com/memcached/memcached/wiki/Install)