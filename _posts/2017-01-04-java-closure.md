---
layout: post
title: java 闭包 
description:  java 闭包
tag: [closure]
comments: true
categories: Java
---

### 1. 闭包

不严谨的说法：

> 1. 一个与外部环境自由变量的函数
> 2. 这个函数能访问外部环境里的自由变量

javascript中的闭包例子：

```javascript
function add(y) {
    return function(x) {
        return x + y;
    }
}
```

对于内部函数function(x) 来说: 1. y是自由变量 2. 其返回值依赖于外部自由变量y 3. add(y) 正好是包含y的环境. 此时，外部函数add(y)对内部函数function(x)形成了闭包.

### 2. java中的闭包

#### 2.1 类和对象

```java
class Add {
    private int x = 1;
    public int add() {
        int y = 2;
        return x + y;
    }
}
```

变量x在add() 外部，add() 方法中通过this关键字访问x. 对象即是闭包.

#### 2.2 内部类

```java
public class Outer {
    private int x = 100;
    class Inner {
        private int y = 1;
        public int innerAdd() {
            return x + y;
        }
    }
}
```

内部类通过包含一个指向外部类的引用，做到自由访问外部类的所有字段，变相的把环境中的自由变量封装到函数里，形成一个闭包.

内部类闭包示意图：

![内部类闭包示意图](/images/20170104/01-closure.png)

#### 2.3 匿名内部类

```java
class Outer {
    public AnnoInner getAnnoInner(final int x) {
        final int y = 2;
        return new AnnoInner() {
            @Override
            public int addXYZ() {
                int z = 3;
                return x + y + z;
            }
        };
    }
}
```
getAnnoInner() 对内部匿名类构成了一个闭包. 但这里别扭的地方是这两个x和y都必须用final修饰，不可以修改。
这是为什么呢？因为这里Java编译器对闭包的支持不完整。说支持了闭包，是因为编译器编译的时候其实悄悄对函数做了手脚，偷偷把外部环境方法的x和y局部变量，拷贝了一份到匿名内部类里。

> ***Java编译器实现的只是capture-by-value，并没有实现capture-by-reference。***

而只有后者才能保持匿名内部类和外部环境局部变量保持同步。
内部类会自动拷贝外部变量的引用，为了避免：1. 外部方法修改引用，而导致内部类得到的引用值不一致 2.内部类修改引用，而导致外部方法的参数值在修改前和修改后不一致。于是就用 final 来让该引用不可改变。
为了同步，Java就不许大家改外围的局部变量。

#### 2.4 其他和匿名内部类相似的情况

除了匿名内部类，还有几种情况，来自外部闭包环境的自由变量必须是final的。

a. 在外部类成员方法中的内部类.

```java
class Outer {
    public void function(final int x) {
        final int y = 2;
        class MethodInner {
            public int add() {
                int z = 3;
                return x + y;
            }
        }
    }
}
```


b. 在一个代码块block里的内部类.

```java
class Outer {
    private int x = 0;

    {
        final int y = 2;
        class MethodInner {
            public int add() {
                int z = 3;
                return x + y;
            }
        }
    }
}

```
