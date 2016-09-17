---
layout: post
title: Thinking in Java - 重载
description:  慎用重载
tag: [note]
comments: true
categories: Java
---

先来看一段代码.

```Java
public class CollectionClassifier {
    public static String classify(Set<?> set) {
        return "set";
    }

    public static String classify(List<?> list) {
        return "list";
    }

    public static String classify(Collection<?> collection) {
        return "Unknown Collection";
    }

    public static void main(String[] args) {
        Collection<?>[] collections = {
                new HashSet<String>(),
                new ArrayList<String>(),
                new HashMap<String, String>().values()
        };

        for (Collection<?> c : collections) {
            System.out.println(classify(c));
        }
    }
}
```

<!-- more -->

输出结果：

```
Unknown Collection
Unknown Collection
Unknown Collection
```

### 慎用重载

为什么会出现上述结果, 因为 classify 方法被`重载`(overloaded)了，而**调用哪个重载(overloading)方法是在编译时决定的**.
对于for循环中的3次迭代，参数的编译时类型都是相同的:`Collection<?>`. 每次迭代的运行时类型不同，但这不影响对重载方法的选择. 

> **对于重载方法(overloaded method)的选择时静态的，而对于被覆盖方法(overridden method)的选择这是动态的.**

最佳实践： **永远不要有2个具有相同参数数目的重载方法**. 如果方法使用可变参数(varargs)，保守的做法是根本不要重载他.

如果对于“**任何一组给定的实际参数将应用于哪个重载方法上**”始终非常清楚，那么，导出多个具有相同参数数目的重载方法就不可能使程序员感到混淆.
例如，对于每一对重载方法，至少有一个参数在类型上根本不同.

### 令人混淆的构造器案例

再来看1段代码.

```Java
    private Confusing(Object o) {
        System.out.println("Object");
    }

    private Confusing(double[] dArray) {
        System.out.println("double array");
    }

    public static void main(String[] args) {
        new Confusing(null);
    }
```

当在调用 Confusing(null) 时，null可以作为Object的实参，也可以应用于double[]版本的重载方法，理论上2个方法都是可行的, 但是jvm肯定不可能2个都执行.
你可能此时会得出结论：该程序不能编译. 事实上，该段代码的结果为`double array`.

> **Java的重载解析过程是以两阶段运行的。第一阶段选取所有可获得并且可应用的方法或构造器。第二阶段在第一阶段选取的方法或构造器中选取最精确的一个。如果一个方法或构造器可以接受传递给另一个方法或构造器的任何参数，那么我们就说第一个方法比第二个方法缺乏精确性.**

在我们的程序中，两个构造器都是可获得并且可应用的。构造器Confusing(Object)可以接受任何传递给Confusing(double[ ])的参数，因此Confusing(Object)相对缺乏精确性。
（每一个double数组都是一个Object，但是每一个Object并不一定是一个double数组。）因此，最精确的构造器就是Confusing(double[ ])，这也就解释了为什么程序会产生这样的输出.

如果你传递的是一个double[ ]类型的值，那么这种行为是有意义的；但是如果你传递的是null，这种行为就有违直觉了。

> **理解本谜题的关键在于在测试哪一个方法或构造器最精确时，这些测试没有使用实际的参数：即出现在调用中的参数。这些参数只是被用来确定哪一个重载版本是可应用的。一旦编译器确定了哪些重载版本是可获得且可应用的，它就会选择最精确的一个重载版本，而此时使用的仅仅是形式参数：即出现在声明中的参数。**

要想用一个null参数来调用 Confusing(Object)构造器，你需要这样写代码：new Confusing((Object)null)。这可以确保只有Confusing(Object)是可应用的。更一般地讲，要想强制要求编译器选择一个精确的重载版本，需要将实际的参数转型为形式参数所声明的类型。
以这种方式来在多个重载版本中进行选择是相当恶心的。在设计API 中，应该确保不会让客户端走这种极端。理想状态下，你应该避免使用重载：为不同的方法取不同的名称。当然，有时候这无法实现，例如，构造器就没有名称，因而也就无法被赋予不同的名称。然而，你可以通过将构造器设置为私有的并提供公有的静态工厂，以此来缓解这个问题。
如果构造器有许多参数，你可以用Builder模式来减少对重载版本的需求量。


### 总结

如果你确实进行了重载，那么请确保所有的重载版本所接受的参数类型都互不兼容，这样，任何两个重载版本都不会同时是可应用的。如果做不到这一点，那么就请确保所有可应用的重载版本都具有相同的行为。

总之，重载版本的解析可能会产生混淆。应该尽可能地避免重载，如果你必须进行重载，那么你必须遵守上述方针，以最小化这种混淆。如果一个设计糟糕的API强制你在不同的重载版本之间进行选择，那么请将实际的参数转型为你希望调用的重载版本的形式参数所具有的类型。

### 参考

1. Thinking in Java

2. Effective Java

3. Java Pluzzlers


