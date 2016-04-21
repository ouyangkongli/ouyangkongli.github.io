---
layout: post
title: Java interview 总结二
description: 总结近期本人面试遇到的典型题目
tag: [Java, interview]
comments: true
categories: Java
---

## 6. Spring MVC基本流程  
1. spring mvc请所有的请求都提交给DispatcherServlet,它会委托应用系统的其他模块负责负责对请求进行真正的处理工作。  
2. DispatcherServlet查询一个或多个HandlerMapping,找到处理请求的Controller.   
3. DispatcherServlet请请求提交到目标Controller.   
4. Controller进行业务逻辑处理后，会返回一个ModelAndView.  
5. Dispathcher查询一个或多个ViewResolver视图解析器,找到ModelAndView对象指定的视图对象。   
6. 视图对象负责渲染返回给客户端。  

## 7. Class.getResource(String path)和ClassLoader.getResource(String path)  

> `Class.getResource(path)` 当path以'/'开头时，从ClassPath根目录下获取资源；否则是该类所在的包下获取资源.    
> `ClassLoader.getResource(path)` 不能以'/'开头，是从ClassPath根目录下获取.  

