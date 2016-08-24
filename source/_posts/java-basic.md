title: java 基础
author: Alvin
tags:
  - java
categories:
  - java
date: 2016-08-23 14:47:00
---
# Java Basic


---------------


## what is java?
Java 编程语言是个简单、完全面向对象、分布式、解释性、健壮、安全与系统无关、可移植、高性能、多线程和动态的编程语言，Java可以撰写跨平台应用软件

- java EE
- java ME
- java SE

## why use java?
- 面向对象的设计思想
- 垃圾自动回收机制

## when to use java?

## where to use java?

## who create java?

## how to use java?

## java 的工作方式

1.源代码  ->  2.编译器  ->  3.输出 -> 4.java 虚拟机

1. 编写源码文件
2. 用编译器运行源代码同时检查错误，如果存在错误就会报错
3. 编译器会产生字节码。 字节码与平台无关
4. 通过java 的虚拟机可以运行字节码

```
  编译器可以使用javac命令
  虚拟机可以使用java命令
```
## java history
- java 1.02  
- java 1.1 
- java 2 version 1.2 ~ 1.4 
- java 5 version 1.5

tip：因为销售人员所以没有java 3 或4

## 核心概念
```
JDK   (java Development Kit)          java 软件开发包
JRE   (java Runtime Envirment)
JVM  (java virtual machine)

JDK 包含 JRE 包含 JVM
```
## 基础代码

main 函数

```
//MyfirstApp.java
public class MyfirstApp {
  public static void main(String[] args) {
        System.out.println("hello world!!");
      }
}
```
过程：

1.先使用javac 命令，生成一个MyfirstApp.class的字节码文件

```
javac MyfirstApp.java
```
2.使用java命令，`此处不需要指定后缀` 此命令执行的是含有main函数的class

```
java MyfirstApp
```

注意：
- 文件名应该和类名一样
- 每个语句必须有；号
- 字符串使用双引号
- while()  括号中不能为int 型   只能为boolean类型  可以写成 while(x==4)
- main函数可以出现任意一个名字的类中，但是只能有一个main函数
- java 是一种强类型语言，所以特别要注意数据类型

## 面向对象
### 优势
- 使用自然的方法设计
- 加入新功能时不会影响已有的代码
- 类可以复用

面向对象的设计
思想：专注于程序中出现的事物而不是过程。

## 类和对象
```
类
类是对象的蓝图

对象
对象本身已知的事物被称为： 实例变量（状态）
对象可以实行的动作被称为：方法（行为）
```
## 面向对象的特性
### 继承
覆盖 override

> 什么时候用继承？
> 判断 Is-A 的关系
Note： Is-A的关系是一个单向先下的关系！

合理的继承设计是可以通过 Is-A 的层级检测的

如果在子类中打算引用父类的方法，然后再加上额外的行为时，可以按照如下的方式

```
pulic void run(){
    super.run();
    // add your special action
}
```

> 当你定义出一组类的父型时，你可以用子类型的任何类来填补任何需要或期待的父型位置

### 多态

运用多态时，引用类型可以是实际对象类型的父型
eg：
有一个Dog 类和 Animal类，Dog类是Animal 的子类，在新建对象时可以这么建立

    Animal dog = new Dog(); 

多态可以运用在参数多态和返回值多态，详见P187页（head first java）

#### 覆盖 (override)
覆盖的要求：
- 参数一样，返回类型兼容
- 不能降低方法的存储权限

#### 重载（overload）
重载是两个方法的方法名字相同，但参数不同

> 重载与多态无关

重载的要求
- 返回类型不同
- 不能只改变返回类型
- 可以更改存取权限

### 封装


## 存储权限

## 语法
### 数组定义

```
String [] arr = {"1","2","3"};
int [] nums = new int[7];
Dog [] pets = new Dog[7];
```

虽然元素本身是primitive主数据类型，但是`数组却是一个对象`

### final 标识符

```
final标志符用于标识为一个继承树的末端，不能被继承
final 标志整个类说明整个类的方法都不能被继承
final 标志一个方法说明这个方法不能被覆盖
```

### abstract 标志符
```
abstract 标志符用于说明抽象的事物
abstract 标志整个类说明该类是一个抽象类
abstract 标志一个方法说明一个方法是一个抽象方法，该方法一定是要被覆盖的
```

## 变量
```
变量定义规则：
rule 1：
变量必须拥有类型
rule 2：
变量必须有名称

变量分为两种： 
primitive 和 引用类型
primitive类型：
```

```
`boolean`    true or false
`char`   16 bits   0 ~ 65535    定义时使用单引号
`byte`   8 bits  -128-127
`short`   16 bits -32768-32767
`int`   32 bits -2147483648-2147483647
`long`  64 bits 
`float`  32bits
`double`  64bits 
```

变量命名规则：
- 名称必须以字母、下划线或$开头，不能用数字开头
- 避开java 的关键字

引用类型：
- 没有对象变量这种东西
- 只有引用到对象的变量
- 对象引用变量保存的是存取对象的方法
- 他并不是对象的容器，而是类似指向对象的指针。或者说是地址

注意：
```
Dog dog = new Dog();
System.out.println(dog);   //Dog@7852e922 显然 dog 的值类似于指针的存储的真正的对象的地址  他本身是个引用

Dog a = null;   //这样是允许的

Dog b = dog；  
System.out.println(b);   
//Dog@7852e922   它的值和 dog 本身的值是一样的，因为引用的是同一个对象
```

## 方法
Java 的方法中的参数是值传递，并不是引用传递

对于简单类型（primitive）主类型数据而言： 形参是对实参的拷贝
对于对象类型而言： 实质上传入的是远程控制的拷贝

## 数据隐藏
要将程序从不良数据改成可以保护数据并且让你还能修改数据的方式是简单的，使用getter 和setter 即可

将实例变量标记为private
将getter和setter 设置为public

## 数据初始化
`实例变量`是存在默认值的，局部变量是没有默认值的

```
integers  0
floating points 0.0
booleans false
reference  null
```

注意： String 的初始化为 null

## 变量比较

primitive 类型的数据使用 == 进行比较

如果是对象比较的话 需要使用equals()方法


## 内置对象的使用
 
ArrayList 
- add
- remove
- contains
- isEmpty
- indexOf
- size
- get

ArrayList  不能存储primitive类型，但是可以存储`类类型`
注意ArrayList 需要导入

```
    import java.util.ArrayList;
    
    ArrayList<int> arr = new ArrayList<int>();    //error
	ArrayList<Integer> arr = new ArrayList<Integer>();
	
```


## java package 包
在java 的API中，类是被包装在包中的，要使用API中的类，你必须知道它被放在了哪个包中。

除了java.lang 包外，其余的所有的java 包都需要在使用时(import)导入 或者使用全称
如何导入包
> import java.util.ArrayList

包的意义：
- 帮助组织项目
- 制造出命名空间，避免类的冲突
- 限制同一命名空间下的类才能相互存取

java 的命名包的传统


## 接口

是一个100%的抽象类,

有些类不应该被初始化，不能被new 出来创造出该类的实例

抽象类是必须被继承的
抽象方法是必须要被覆盖的


```
    abstract class Canine extends Animal(){
    }
```

接口的定义：
```
    public interface Pet { ...}
```
接口的实现：
```
public class Dog extends Canine implements Pet {...}
```

extend 只能有一个，但是implement 可以有多个

实现某接口的类必须实现它所有的方法


## 对象的前世今生
> 对象有生有死

### java 的堆与栈
```
java中使用了两种内存区域： 堆与栈
堆：对象的生存空间
栈：方法调用及变量的生存空间

堆上存储：对象；实例变量；
当一个新建对象带有对象引用的变量时，会在堆上存储这个引用类型的变量的空间
栈上存储：局部变量；方法调用；对象的引用；                                                                                
```
### 构造函数
构造函数的要求：
- 没有返回值
- 名称与类名一致
- 如果你自己写了一个传参的构造函数，那么无参的构造函数也必须自己写

```
public class Car{
    public Car(){
      //构造函数
    }
}
```

构造函数不会被继承

构造函数链
如果新建（new）一个继承的类的对象时，父类的构造函数是被依次执行的

实践：如果要在构造对象时传入参数，那么最好写两个构造函数以适应传参和不传参两种情况

如何调用父类的构造函数 `super()`，当然如果没写，编译器会帮我们自动加上的
**this**可以使用this()来从某个构造函数调用同一个类的另外一个构造函数；
this()只能用在构造函数中，且必须在第一行语句中。
super和this不能兼得

## 静态方法和非静态方法
方法名前加入static关键字，申明这个方法的调用不需要实例对象
```
静态的方法不能调用非静态的变量
静态的方法也不能调用非静态的方法
```

```
非静态的方法可以调用静态的变量
非静态的方法可以调用静态的方法
```
## 静态变量
静态变量被所有的实例对象所共享


## final
静态的final变量是常数，初始化后不能被改变
```
  public static final double PI = 3.1415926;
```

```
final 的变量代表你不能改变它的值；
final 的方法代表你不能覆盖掉该方法；
final 的类代表你不能继承该类；
```
## AutoBoxing
每一个primitive主类型数据都有一个包装用的类
类名为：
- Boolean
- Character
- Byte
- Short
- Integer
- Long
- Float
- Double


包装与分开包装

```
//包装
int i = 128;
Interer iWrap = new Interger(i);
//分开包装
int unWrapped = iWrap.intValue()；
```




## Intellij IDEA的快捷键
- option  + Enter  快速导包

## 经验
1.如何防止一个类被初始化？
- 设置该类为抽象类
- 或设置该类的构造函数为私有

2.类型转换 
String 转换为 primitive类型 ： 
> Integer.parseInt("4536.4864")
> Double.parseDouble("420.20")

primitive转换为String类型
> X.toString()

3.数字的格式化

使用String 的format方法
```
  String s = String.format("%,f", 10000000.124);
  常用的：
  %d
  %f  %.2f  %,.2f
  %x
  %c
```


4.日期格式化
使用String 的format方法
```
  String.format("xx",new Date());
  xx 常用的：
  %tc 完整的日期与时间：   Sun Nov 28 14:52:41 MST 2016
  %tr 只有时间 03：01：47 PM
  %tA， %tB %td  周、月、日  Sunday，november 28
  
  
```

5.如何处理时间
在java中处理时间使用的是继承过的Calendar 的对象
eg：
```
Calendar cal  = new Calendar();     //error   因为Calendar是一个抽象类
Calendar cal  = Calendar.getInstance();
```

## 常识
1.构造函数不能被标记为是静态的
2.构造函数是在静态变量的初始化之后进行的

## Temp

java的简写：

sout   -> System.out.print()