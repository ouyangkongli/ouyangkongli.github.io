---
layout: post
title: Java interview 总结一
description: 总结近期本人面试遇到的典型题目
tag: [Java, interview]
comments: true
categories: Java
---

## 1. Jdk和Jre
今天面试的一道题目，惭愧ing，java开发近2年，对于这2个单词，最熟悉不过，也知道其中的含义，但是如何二者的区别和联系是什么？  

> JDK： Java Development Kit  
> JRE:  Java Runtime Environment   

JRE是java运行时环境，包含了java虚拟机，java基础类库。是使用java语言编写的程序运行所需要的软件环境，是提供给想运行java程序的用户使用的。  
JDK是java开发工具包，是程序员使用java语言编写java程序所需的开发工具包，是提供给程序员使用的。JDK包含了JRE，同时还包含了编译java源码的编译器javac，还包含了很多java程序调试和分析的工具：jconsole，jvisualvm等工具软件，还包含了java程序编写所需的文档和demo例子程序。

## 2. Http协议特点
无连接，无状态 
详细说明见： 
+ [如何理解HTTP协议的 “无连接，无状态” 特点？](http://blog.csdn.net/tennysonsky/article/details/44562435)

<!-- more -->

## 3. Jsp 内置对象  

request表示HttpServletRequest对象。它包含了有关浏览器请求的信息，并且提供了几个用于获取cookie, header, 和session数据的有用的方法。   
response表示HttpServletResponse对象，并提供了几个用于设置送回 浏览器的响应的方法（如cookies,头信息等）   
out对象是javax.jsp.JspWriter的一个实例，并提供了几个方法使你能用于向浏览器回送输出结果。    
pageContext表示一个javax.servlet.jsp.PageContext对象。它是用于方便存取各种范围的名字空间、servlet相关的对象的API，并且包装了通用的servlet相关功能的方法。   
session表示一个请求的javax.servlet.http.HttpSession对象。Session可以存贮用户的状态信息   
applicaton 表示一个javax.servle.ServletContext对象。这有助于查找有关servlet引擎和servlet环境的信息   
config表示一个javax.servlet.ServletConfig对象。该对象用于存取servlet实例的初始化参数。   
page表示从该页面产生的一个servlet实例

## 4. Servlet生命周期   
web容器加载servlet，init()->service()(doGet() or doPost())->destroy()