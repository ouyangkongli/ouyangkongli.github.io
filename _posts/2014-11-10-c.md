---
layout: post
title: c++继承中的隐藏规则
category: c++
tags: [c++, 继承]
date: 2014-11-10
---


---
**转载自：[http://blog.csdn.net/jackystudio/article/details/17243251](http://blog.csdn.net/jackystudio/article/details/17243251)**

---

###1.重载和覆盖
在了解隐藏之前，得先分清楚重载和覆盖。  

> 1. 重载：对同一个类而言的，函数名相同，函参不同，因为是在同一个类中，所以是否是virtual并不重要。  
> 2. 覆盖：对基类和派生类而言。函数名相同，函参相同，必须是virtual函数。  

如下示例，1和2是重载关系，而1和3是覆盖关系。  
{% highlight cpp %}
class A{
public:
	virtual void fun(){cout<<"A::fun()";}//1
	void fun(int a){cout<<"A::fun(a)";}//2
};

class B : public A{
public:
	void fun(){cout<<"B::fun()";}//3
};
{% endhighlight %}

###2.隐藏
首先隐藏也是发生对基类和派生类而言的，所以主要就是要理清隐藏和覆盖的差异。  

> 1. 如上所述，覆盖要求是virtual函数，所以如果是不带virtual的同名同参函数，那么就是隐藏。  
> 2. 不管有没有virtual标识，函参不一样，那么也是隐藏。

<!-- more -->
如下示例，1和3同名同参，但是无virtual标识，所以是隐藏。2和4函参不同，所以也是隐藏。  
{% highlight cpp %}
class A{
public:
	void fun1(){cout<<"A::fun1()";}//1
	virtual void fun2(int a){cout<<"A::fun2(a)";}//2
};

class B : public A{
public:
	void fun1(){cout<<"B::fun1()";}//3
	void fun2(){cout<<"B::fun2()";}//4
};
{% endhighlight %}

###3.一种简单区分调用方法
如果对上面的重载，覆盖，隐藏还是搞不清楚，再加上多态的使用，更会是云里雾里。这里提供了一种方便理解的区分调用方法（From C++ primer）。  

> 1. 首先确定进行函数调用的对象、引用或指针的静态类型。  
> 2. 在该类中查找函数，如果找不到，就在直接基类中查找，如此循着类的继承链往上找，直到找到该函数或者查找完最后一个类。如果不能在类或其相关基类中找到该名字，则调用是错误的。  
> 3. 一旦找到了该名字，就进行常规类型检查，查看如果给定找到的定义，该函数是否合法。  
> 4. 假定函数调用合法，编译器就生成代码。如果函数是虚函数且通过引用或指针调用，则编译器生成代码以确定根据对象的动态类型运行哪个函数版本，否则，编译器生成代码直接调用函数。  

###4.示例
{% highlight cpp %}
class A{
public:
	virtual void fun1(){cout<<"A::fun1()";}//1
	void fun2(){cout<<"A::fun2()";}//2
	virtual void fun3(){cout<<"A::fun3()";}//3
};

class B : public A{
public:
	void fun1(){cout<<"B::fun1()";}//4
	void fun3(int a){cout<<"B::fun3(a)";}//5
};

int _tmain(int argc, _TCHAR* argv[])
{
	B b;
	b.fun1();
	b.fun2();
	b.fun3(1);

	A* pa=new B();
	pa->fun1();
	pa->fun2();
	pa->fun3();
}
{% endhighlight %}
按照上述方法分析：  

> 1. B b。B类型，所以所有的调用要先在B类中查找。  
fun1存在，所以调用4。  
fun2不存在，所以在基类A中查找，存在fun2()，所以调用2。  
fun3存在，但是要求带参，所以只能调用fun3(int a)，如果b.fun3()则会报错，因为一旦找到名字就会停下，进行类型匹配检查。所以调用5。
> 2. A* pa=new B()。A类型指针，所以所有的调用要现在A类中查找（此时不会再到B中查找，但不意味着不调用B中函数）fun1存在，因为new的是B的对象，而且是虚函数，所以到B中查找是否进行了覆盖，发现确实进行了覆盖，所以调用4。  
fun2存在，调用2。  
fun3存在，因为new的是B的对象，而且是虚函数，所以到B中查找是否进行了覆盖，发现没有有进行覆盖，所以调用3。

![cppresult](/images/cppresult_2014_11_10.jpg)  
