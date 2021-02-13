---
title: Java核心技术卷一学习笔记（二）
date: 2021-02-13 13:38:06
tags: Java学习
---

# 对象与类

<!--more-->

## 1 面向对象程序设计概述

- 在Java中，所有的类都扩展自Object类

- 在类之间有三种常见的关系

    - 依赖（uses-a）：如果一个类的方法使用或操纵另一个类的对象，就说一个类依赖于另一个类
    
    - 聚合（has-a）：一个类包含着另一个类的对象

    - 继承（is-a）

- 对象变量并没有实际包含一个对象，它只是引用一个对象。在Java中，任何对象变量的值都是对储存在另外一个地方的某个对象的引用。区别于C++的引用，Java的对象变量更类似与C++的对象指针，且两者初始化语法类似。

    C++：Date* birthday = new Date();

    Java: Date birthday = new Date();

## 2 使用预定义类及用户自定义类

- 尽量不要用public标记实例字段，这会破坏封装性

- Java构造器工作方式与C++一样，但是所有Java对象都是在堆中构造，构造器总是结合new操作符一起使用，不可像C++一样忘记new操作符：A a(5);//C++,not java

- 在所有方法中都不要使用与实例字段同名的变量

- 同样可以使用var关键字声明对象，但只能用于方法中的局部变量。参数和字段的类型必须声明。

- 在C++中，通常在类的外面定义方法，在类体内定义方法会自动内联。但是，在Java中，所有方法都必须在类的内部定义，但并不表示它们是内联方法。是否将某个方法设置为内联方法是Java虚拟机的任务。即时编译器会监视那些简短、经常调用而且没有被覆盖的方法调用，并进行优化。

- 方法可以访问所属类任何对象的私有特性，不仅限于隐式参数。

- 可以将实例字段定义为final，这样的字段必须在构造对象时初始化。但对于可变的类，使用final修饰符会造成混乱。例如：

```
private final StringBuilder evaluations;
```
final关键字只是表示存储在evaluations变量中的对象引用不会再指示另外一个不同的StringBuilder对象。不过这个对象本身可以更改：
```
evaluations.append(LocalDate.now()+":Gold star!\n");
```

## 3 静态字段与静态方法

- static修饰的字段，每个类只有一个这样的字段，与C++类似。

- 静态方法是不在对象上执行的方法，可以认为静态方法是没有this参数的方法，但是，静态方法可以访问静态字段。

- 在下面两种情况下可以使用静态方法：

    - 方法不需要访问对象状态，因为它需要的所有参数都通过显示参数提供

    - 方法只需要访问类的静态字段

- 静态方法还有另一种常见的用途。类似NumberFormat的类使用静态工厂方法来构造对象。

```
NumberFormat currencyFormatter = NumberFormat.getCurrencyInstance();
NumberFormat percentFormatter = NumberFormat.getPercentInstance();
double x = 0.1;
System.out.println(currencyFormatter.format(x));//$0.10
System.out.println(percentFormatter.format(x));//10%
```

为什么要使用静态工厂方法？

1. 无法命名构造器。构造器的名字必须与类名相同。但是这里希望有两个不同的名字，分别得到货币实例和百分比实例。

2. 使用构造器时，无法改变所构造对象的类型。而工厂方法实际上将返回DecimalFormat类的对象。

## 4 方法参数

- Java总是采用按值调用。方法不能修改传递给它的任何参数变量的内容。
