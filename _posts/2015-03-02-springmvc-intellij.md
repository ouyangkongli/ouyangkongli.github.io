---
layout: post
keywords: springmvc
description:
title: "Intellij 下创建的springmvc"
categories: [java]
tags: [springmvc, gson]
group: archive
icon: file-alt
---
{% include oukongli/setup %}


参照Intellij官方帮助文档，[Getting Started with Spring MVC, Hibernate and JSON](https://confluence.jetbrains.com/display/IntelliJIDEA/Getting+Started+with+Spring+MVC,+Hibernate+and+JSON)，在教程的基础上做了以下修改:     


1.  将原文需要的css做为静态资源引用；**需要注意的是，静态资源的引用。 可参考：[http://blog.csdn.net/jbgtwang/article/details/7359592](http://blog.csdn.net/jbgtwang/article/details/7359592)**  
2.  原文使用的json包为org下，本文改为google的gson。  

源代码已上传至github上，[https://github.com/oukongli/javanote/tree/master/springmvc](https://github.com/oukongli/javanote/tree/master/springmvc);

<!-- more -->

关于gson的使用，可参见下列博客：  

1.  Json转换利器Gson之实例一-简单对象转化和带泛型的List转化 [http://blog.csdn.net/lk_blog/article/details/7685169](http://blog.csdn.net/lk_blog/article/details/7685169)  
2.  Json转换利器Gson之实例二-Gson注解和GsonBuilder [http://blog.csdn.net/lk_blog/article/details/7685190](http://blog.csdn.net/lk_blog/article/details/7685190)  
3.  Json转换利器Gson之实例三-Map处理(上) [http://blog.csdn.net/lk_blog/article/details/7685210](http://blog.csdn.net/lk_blog/article/details/7685210)  
4.  Json转换利器Gson之实例四-Map处理(下) [http://blog.csdn.net/lk_blog/article/details/7685224](http://blog.csdn.net/lk_blog/article/details/7685224)  
5.  Json转换利器Gson之实例五-实际开发中的特殊需求处理 [http://blog.csdn.net/lk_blog/article/details/7685237](http://blog.csdn.net/lk_blog/article/details/7685237)  
6.  Json转换利器Gson之实例六-注册TypeAdapter及处理Enum类型 [http://blog.csdn.net/lk_blog/article/details/7685347](http://blog.csdn.net/lk_blog/article/details/7685347)‍  



参考文档：  
[http://www.cnblogs.com/dreamfree/p/4172674.html](http://www.cnblogs.com/dreamfree/p/4172674.html)


