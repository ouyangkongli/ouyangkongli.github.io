---
layout: post
keywords: exception
description:
title: "java匿名内部类"
categories: [java]
tags: [java, 匿名内部类]
group: archive
icon: file-alt
---
{% include oukongli/setup %}

转载自：[http://www.cnblogs.com/chenssy/p/3390871.html](http://www.cnblogs.com/chenssy/p/3390871.html)


####1. 使用匿名内部类

匿名内部类创建方式如下：
{% highlight java %}
new 父类构造器（参数列表）|实现接口（）{
   //匿名内部类的类体部分
}
{% endhighlight %}

匿名内部类中，必须要继承一个父类或者实现一个接口，当然也仅能只继承一个父类或者实现一个接口。同时它也是没有class关键字，这是因为匿名内部类是直接使用new来生成一个对象的引用。
**示例**：
{% highlight java %}
public abstract class Bird {
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public abstract int fly();
}

public class Test {
    public void test(Bird bird){
        System.out.println(bird.getName() + "能够飞 " + bird.fly() + "米");
    }
    public static void main(String[] args) {
        Test test = new Test();
        test.test(new Bird() {
            public int fly() {
                return 10000;
            }
            public String getName() {
                return "大雁";
            }
        });
    }
}
------------------
Output：
大雁能够飞 10000米
{% endhighlight %}

在Test类中，test()方法接受一个Bird类型的参数，同时一个抽象类是没有办法直接new的，我们必须要先有实现类才能new出来它的实现类实例。

<!-- more -->

####2.注意事项
1. 使用匿名内部类时，我们必须是继承一个类或者实现一个接口，但是两者不可兼得，同时也只能继承一个类或者实现一个接口。
2. 匿名内部类中是不能定义构造函数的。
3. 匿名内部类中不能存在任何的静态成员变量和静态方法。
4. 匿名内部类为局部内部类，所以局部内部类的所有限制同样对匿名内部类生效。
5. 匿名内部类不能是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法。

####3.参数为final
我们给匿名内部类传递参数的时候，若该形参在内部类中需要被使用，那么该形参必须要为final。也就是说：当所在的方法的形参需要被内部类里面使用时，该形参必须为final。  
首先我们知道在内部类编译成功后，它会产生一个class文件，该class文件与外部类并不是同一class文件，仅仅只保留对外部类的引用。当外部类传入的参数需要被内部类调用时，从java程序的角度来看是直接被调用：  
{% highlight java %}
public class OuterClass {
    public void display(final String name,String age){
        class InnerClass{
            void display(){
                System.out.println(name);
            }
        }
    }
}
{% endhighlight %}
从上面代码中看好像name参数应该是被内部类直接调用？其实不然，在java编译之后实际的操作如下：
{% highlight java %}
public class OuterClass$InnerClass {
    public InnerClass(String name,String age){
        this.InnerClass$name = name;
        this.InnerClass$age = age;
    }
    public void display(){
        System.out.println(this.InnerClass$name + "----" + this.InnerClass$age );
    }
}
{% endhighlight %}
所以从上面代码来看，内部类并不是直接调用方法传递的参数，而是利用自身的构造器对传入的参数进行备份，自己内部方法调用的实际上时自己的属性而不是外部方法传递进来的参数。  
直到这里还没有解释为什么是final？在内部类中的属性和外部方法的参数两者从外表上看是同一个东西，但实际上却不是，所以他们两者是可以任意变化的，也就是说在内部类中我对属性的改变并不会影响到外部的形参，而这从程序员的角度来看这是不可行的，毕竟站在程序的角度来看这两个根本就是同一个，如果内部类该变了，而外部方法的形参却没有改变这是难以理解和不可接受的，所以为了保持参数的一致性，就规定使用final来避免形参的不改变。  
简单理解就是，拷贝引用，为了避免引用值发生改变，例如被外部类的方法修改等，而导致内部类得到的值不一致，于是用final来让该引用不可改变。  
故如果定义了一个匿名内部类，并且希望它使用一个其外部定义的参数，那么编译器会要求该参数引用是final的。  

####4.匿名内部类初始化
匿名内部类是没有构造器的！那怎么来初始化匿名内部类呢？使用构造代码块！利用构造代码块能够达到为匿名内部类创建一个构造器的效果。

{% highlight java %}
public class OutClass {
    public InnerClass getInnerClass(final int age,final String name){
        return new InnerClass() {
            int age_ ;
            String name_;
            //构造代码块完成初始化工作
            {
                if(0 < age && age < 200){
                    age_ = age;
                    name_ = name;
                }
            }
            public String getName() {
                return name_;
            }

            public int getAge() {
                return age_;
            }
        };
    }

    public static void main(String[] args) {
        OutClass out = new OutClass();

        InnerClass inner_1 = out.getInnerClass(201, "chenssy");
        System.out.println(inner_1.getName());

        InnerClass inner_2 = out.getInnerClass(23, "chenssy");
        System.out.println(inner_2.getName());
    }
}
{% endhighlight %}