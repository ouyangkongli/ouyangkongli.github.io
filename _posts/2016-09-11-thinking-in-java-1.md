---
layout: post
title: Thinking in Java - 变量的初始化顺序
description:  Thing in Java 读书笔记
tag: [note]
comments: true
categories: Java
---

### Java 变量初始化顺序

#### 普通类

1. 静态成员初始化（statis块可看作一特殊的静态成员）

2. 普通成员变量

3. 构造函数

对于静态成员和普通成员， 其初始化的顺序和其在类中定义的顺序有关.

#### 继承类

1. 父类静态成员及static块

2. 子类静态成员及static块

3. 父类普通成员

4. 父类构造函数

5. 子类普通成员

6. 子类构造函数


