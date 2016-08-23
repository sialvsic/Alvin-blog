title: java 基础
author: Alvin
tags:
  - java
categories:
  - java
date: 2016-08-23 14:47:00
---
# Java

## what is java?

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

编译器可以使用javac命令
虚拟机可以使用java命令

## java history
java 1.02  
java 1.1
java 2 version 1.2 ~ 1.4
java 5 version 1.5

tip：因为销售人员所以没有java 3 或4

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
1. 先使用javac 命令，生成一个MyfirstApp.class的字节码文件

```
javac MyfirstApp.java
```
2. 使用java命令，`此处不需要指定后缀` 此命令执行的是含有main函数的class

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
类
类是对象的蓝图

对象
对象本身已知的事物被称为： 实例变量（状态）
对象可以实行的动作被称为：方法（行为）

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
final标志符用于标识为一个继承树的末端，不能被继承
final 标志整个类说明整个类的方法都不能被继承
final 标志一个方法说明这个方法不能被覆盖

###abstract 标志符
abstract 标志符用于说明抽象的事物
abstract 标志整个类说明该类是一个抽象类
abstract 标志一个方法说明一个方法是一个抽象方法，该方法一定是要被覆盖的

## 变量
变量定义规则：
rule 1：
变量必须拥有类型
rule 2：
变量必须有名称

变量分为两种：
primitive 和 引用类型
primitive类型：

`boolean`    true or false
`char`   16 bits   0 ~ 65535    定义时使用单引号
`byte`   8 bits  -128-127
`short`   16 bits -32768-32767
`int`   32 bits -2147483648-2147483647
`long`  64 bits
`float`  32bits
`double`  64bits

变量命名规则：
- 名称必须以字母、下划线或$开头，不能用数字开头
- 避开java 的关键字

引用类型：
- 没有对象变量这种东西
- 只有引用到对象的变量
- 对象引用变量保存的是存取对象的方法
- 他并不是对象的容器，而是类似指向对象的指针。或者说是地址

注意：
Dog dog = new Dog();
System.out.println(dog);   //Dog@7852e922 显然 dog 的值类似于指针的存储的真正的对象的地址  他本身是个引用

Dog a = null;   //这样是允许的

Dog b = dog；  
System.out.println(b);    //Dog@7852e922   它的值和 dog 本身的值是一样的，因为引用的是同一个对象

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

integers  0
floating points 0.0
booleans false
reference  null

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

## Intellij IDEA的快捷键
- option  + Enter  快速导包


## Temp
java的简写：

sout   -> System.out.print()
