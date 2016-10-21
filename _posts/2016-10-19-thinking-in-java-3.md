---
layout: post
title: Thinking in Java - Exception 
description:  java exception
tag: [note]
comments: true
categories: Java
---

### 异常

#### 异常的限制

1. 当子类覆盖父类方法时，只能抛出在父类方法的异常说明中列出的那些异常。但是此限制对构造器函数不起作用，子类构造器的异常说明必须包含父类构造器中的异常说明。

2. 子类构造器不能catch基类构造器抛出的异常。

3. 只针对异常的情况下才使用异常，永远不要把异常用于正常的控制流。

4. 对可恢复的情况使用受检异常，对编程错误使用运行时异常。
