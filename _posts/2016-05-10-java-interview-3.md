---
layout: post
title: Java interview 总结三
description: 总结近期本人面试遇到的典型题目
tag: [java, interview]
comments: true
categories: Java
---

1. 	多系统之间调用超时问题处理多系统之间调用超时问题处理  
	如系统A调用系统B服务，系统B需要处理大量数据(或耗时较长的DB call)，设计出现超时情况，如何处理?
	（系统B事务回滚、系统A重试机制、aop等）
	
2.	控制service最大并发量  
	(容器级别、单一service级别、线程池)
	
3.	1亿条数据中查找1条数据，查询时间响应问题，可能解决方案  
	（硬件、index、表分片、分布式DB）

<!-- more -->

