---
title: Java核心技术卷一学习笔记（一）
date: 2021-02-08 12:31:14
tags: Java学习
---

# Java的基本程序设计结构

<!--more-->

## 1 一个简单的Java应用程序

- Java应用程序中的全部内容都必须放置在类中

- 命名规范：类名必须以大写字母开头，若名字有多个单词组成，则每个单词首字母都大写，驼峰命名法

- 源代码文件名必须与该文件中的公共类名字相同

- Java1.4以后，main方法必须是public

- 输出：println方法会在输出后增加一个换行符，若不需要，使用print方法

## 2 数据类型

- Java是一种强类型语言。每一个变量都需要声明一种类型。

- Java与C++不同，整型不会根据不同的处理器选择不同大小，int统一为4字节，short为2字节，long为8字节，byte为1字节。

- 从Java7开始，用0b开头表示二进制数

- 从Java7开始，数字字面量可以添加下划线，编译器会去除下划线，如1_000_000等价于1000000

- Integer类和Long类都提供了处理无符号数的方法

## 3 浮点类型

- 没有后缀F的默认为double型

- 不能使用 x==Double.NaN来判断一个特定值是否不为数字，而应该使用Double.isNaN（x）

## 4 Unicode与char类型

- 码点（codePoint）是指一个与编码表中的某个字符对应的代码值

- char类型代表一个字符，即一个代码单元

- 不可用char来存储一个码点，因为一个码点可能对应两个代码单元

## 5 变量

- 如果可以从变量的初始值判断数值类型，就不需要再声明类型，使用var关键字即可

```
var x = 12;//x为int型
var y = "hello";//y为string
```

- 与C++不同，Java常量关键字使用final，若某个常量需要被多个方法使用，声明为static final，若需要在类外使用，声明为public static final ，通过Object.*cast*使用即可。

- 枚举类型：枚举类型变量只能储存在这个类型声明中给定的某个枚举值或者null

## 6 位运算符

- “>>>”运算符会用0填充高位，而“>>“会根据符号位填充

- 移位运算符的右操作数要完成模32的运算（若左操作数为long，则要模64），如1<<35 等价于 1<<3

## 7 字符串

- Java的String是一个类，不是一个字符串，与C++不同，String也不是一个字符数组，而类似与字符串指针，但不能采用指针的方式运算。

- Java的String类是不可变的，因此字符串在存储池中是可以共享的，如果某个字符块不再使用，Java会自动回收，不会造成内存泄漏。

- 由于String对象类似指针，因此不可使用==来比较两个字符串是否相等，因为两个对象可能共享同一个字符串，需要使用equals方法。

- 如果在一个null值上调用方法，会抛出异常。

- String、StringBuffer、StringBuilder对比

    - String为不可变的，虽然两个String对象可以相加，但每次拼接，都会构建一个新的字符串，耗时且浪费空间。可以等价于：

    ```
    public final class String
    {
        private final char value[];
        ···
    }
    ```

    - 而StringBuffer和StringBuilder底层等价于一个普通数组，是可变的，因此拼接字符串时是对其本身对象进行操作，不会产生新的对象。使用.append方法即可拼接字符串。

    - StringBuffer与StringBuilder，前者是线程安全的，因为其中很多方法被synchronized修饰，相当于锁机制，而后者没有，但后者效率偏高。

    - 如果会多次修改字符串，且不考虑线程安全，使用StringBuilder，考虑线程安全应该使用StringBuffer。

## 8 输入输出

- 输入需要创建Scanner对象：

```
Scanner in = new Scanner(System.in);
String name = in.nextLine();//一行字符串，包括空格
String name = in.next();//一个单词，不包括空格
int age = in.nextInt();//输入数字
```

如果需要输入密码，应该使用Console，在控制台不会显示出输入的密码，输入完成后应立即覆盖数组：

```
Console cons = System.console();
String username = cons.readLine("user name:");
char [] password = cons.readPassword("Password:");
```

- 格式化输出：可以用一个格式字符串指示要格式化的索引

```
Syetem.out.println("%1$s %2$tB %2$te %2$tY","Dut Date:",new Date());
```

- 文件输入输出：同样需要一个Scanner对象：

```
Scanner in = new Scanner(Path.of("myfile.txt"),StandardCharsets.UFT-8);
```

注意：可以构造一个参数为字符串的Scanner对象，但会把字符串解析为数据而不是文件名

## 9 控制流程

- Java中不允许在嵌套的块内有多个同名变量

- 循环时，检测两个浮点数要格外小心，因为舍入误差可能造成死循环

```
for(double x = 0;x!=10;x+=0.1) ...
```

- Java中没有goto语句，但break可以有标签，在标签指定的循环中使用 break 标签可以跳出最外层循环

## 10 数组

- 一旦创建数组，就不能改变长度，若需要改变长度，使用list

- 使用=可以完成浅拷贝，若要深拷贝，需要使用Array.copyOf（）方法

- 生成随机数：Math.random()返回0到1之间随机的浮点数，n*Math.random()就可以得到0到n-1的一个随机数。


