---
layout: post
title: 提升逼格-搭建自己的google镜像
description:  使用Nginx Module for Google搭建自己的google镜像
tag: [google, nginx, tool]
comments: true
categories: tool
---

### 介绍

如果还有人在百度上搜索技术资料，还在忍受着百度的各种无耻行为，还在为在国内不能使用google而烦恼，还在担心VPN可能会随时出卖你，还在抱怨国内的网络环境。
**Now is your answer！**

### 准备

首先要有一台能访问google.com的vps或云主机，如果你不差花钱的话，推荐一下阿里的 [云服务器ECS](https://ecs-buy.aliyun.com/#/prepay)。

<!-- more -->

### 神器初现

先来看一下这个开源神器，`ngx_http_google_filter_module`, 主要原理是用了nginx的反向代理。
cuber在github分享了他的代码，[https://github.com/cuber/ngx_http_google_filter_module](https://github.com/cuber/ngx_http_google_filter_module) .  
目前，有很多google镜像网站，都是由该扩展驱动。下面，按照官方教程简单介绍一下搭建的方法。  
更详细的文档，见 [https://github.com/cuber/ngx_http_google_filter_module/blob/master/README.zh-CN.md](https://github.com/cuber/ngx_http_google_filter_module/blob/master/README.zh-CN.md).

#### 安装 ####

```bash
#
# 安装 gcc & git
#
apt-get install build-essential git gcc g++ make

#
# 下载最新版源码
# nginx 官网: 
# http://nginx.org/en/download.html
#
wget "http://nginx.org/download/nginx-1.7.8.tar.gz"

#
# 下载最新版 pcre
# pcre 官网:
# http://www.pcre.org/
#
wget "ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.38.tar.gz"

#
# 下载最新版 openssl
# opessl 官网:
# https://www.openssl.org/
#
wget "https://www.openssl.org/source/openssl-1.0.1j.tar.gz"

#
# 下载最新版 zlib
# zlib 官网:
# http://www.zlib.net/
#
wget "http://zlib.net/zlib-1.2.8.tar.gz"

#
# 下载本扩展
#
git clone https://github.com/cuber/ngx_http_google_filter_module

#
# 下载 substitutions 扩展
#
git clone https://github.com/yaoweibin/ngx_http_substitutions_filter_module


#
# 解压缩
#
tar xzvf nginx-1.7.8.tar.gz
tar xzvf pcre-8.38.tar.gz
tar xzvf openssl-1.0.1j.tar.gz
tar xzvf zlib-1.2.8.tar.gz

#
# 进入 nginx 源码目录
#
cd nginx-1.7.8

#
# 设置编译选项
#
./configure \
  --prefix=/opt/nginx-1.7.8 \
  --with-pcre=../pcre-8.38 \
  --with-openssl=../openssl-1.0.1j \
  --with-zlib=../zlib-1.2.8 \
  --with-http_ssl_module \
  --add-module=../ngx_http_google_filter_module \
  --add-module=../ngx_http_substitutions_filter_module
  
#
# 编译, 安装
# 如果扩展有报错, 请发 issue 到
# https://github.com/cuber/ngx_http_google_filter_module/issues
#
make
sudo make install

#
# 启动, 安装过程到此结束
#
sudo /opt/nginx-1.7.8/sbin/nginx

#
# 配置修改后, 需要 reload nginx 来让配置生效, 
#
sudo /opt/nginx-1.7.8/sbin/nginx -s reload
```

#### 基本配置方法 ####

`http`配置方式

```nginx
server {
  server_name <你的域名>;
  listen 80;

  resolver 8.8.8.8;
  location / {
    google on;
  }
}
```


##### 默认语言偏好 #####
默认的语言偏好可用 `google_language` 来设置, 如果没有设置, 默认使用 `zh-CN` (中文)

```nginx
location / {
  google on;
  google_scholar on;
  # 设置成英文
  google_language "en"; 
}
```


### 参考


