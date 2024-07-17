---
layout: post
title: Java 基础八股文复习(1)
categories: JavaBasics
description: Java 基础八股文复习(1)
keywords: JavaBasics, JVM, JDK, JRE
---

## JVM, JDK, JRE
**JVM (Java Virtual Machine)** 
是运行 Java 字节码的虚拟机，目的是在不同系统下，相同的字节码可以给出相同的结果，针对不同系统具体实现的 JVM 以及 字节码是实现 Java “一次编译，随处可以运行”的关键所在。

**JDK (Java Development Kit)**
JDK 是 Java 能够创建、编译和运行程序的工具包，包含 编译器（javac）和工具（javadoc 和 jdb），以及 JRE (JVM, Java 类库)。

**JRE (Java Runtime Environment)**
JRE 是 Java 能够运行已编译 Java 程序所需的所有内容的集合，包括 JVM，Java 类库，类加载器，java 命令，以及其他一些基础构件.

## 字节码
JVM 可以理解的代码就叫作 **字节码**（即拓展名为.class的文件），他只面向 JVM 而不是特定的处理器。Java 语言通过字节码，在一定程度上**解决了传统解释性语言执行效率低的问题，但又保留了解释型语言可移植的特点**。

## 解释型语言 VS 编译型语言

|   区别    |   解释型语言   | 编译型语言  |
|  ----  | ----  | ----  |
| 定义  | 通过**编译器**将源码一次性转换成该平台执行的机器码  | 通过**解释器**一句一句将代码解释为机器码再执行 |
| 例子  |  C、C++、Go、Rust  | Java、Python、JavaScript、PHP |
| 开发效率  |   低  | 高 |
| 执行速度  |   慢  | 快 |

Java语言即具有编译型语言的特征（Java 需要先进行编译，生成字节码文件），也有解释型语言的特征（JVM 需要使用解释器/JIT来执行字节码文件）

## Oracle JDK VS OpenJDK

|   区别    |   OracleJDK   | OpenJDK  |
|  ----  | ----  | ----  |
| 开源性  |   不完全开源  | 完全开源 |
| 稳定性  |   更稳定，因为由 Oracle 单独研发  |  |
| 响应性和 JVM 性能 |   更好  |  |
| 更新频率 |        |    更高    |

## [Java 语言关键字](https://www.cnblogs.com/lukelook/p/7825498.html)

一、概念

Java关键字（Key Word）：  对Java的编译器有特殊的意义，他们用来表示一种数据类型或者表示程序的结构.

保留字（Reserve Word）：即它们在Java现有版本中没有特殊含义，以后版本可能会作为有特殊含义的词，或者该词虽然在Java中没有特殊含义，以后版本也不打算使用，但在其它语言中有特殊含义，不宜在Java中定义为变量名称等，因为容易混淆。

注意：关键字和保留字均不能用作变量名、方法名、类名、包名和参数。

二、具体的保留字

goto、const

三、具体的关键字（51个）

1.访问修饰符（3个）

public、protected、private

2.类、接口、抽象类（9个）

class、interface、abstract——定义

extends——继承类、implements——实现接口

new——新建一个对象、super——调用父类方法、this——指代当前对象

instanceof——通过返回一个布尔值来指出，这个对象是否是这个特定类或者是它的子类的一个实例。

3.基本数据类型（13个）

void——没有返回值

byte、short、int、long——整型数据

float、double——浮点型数据

char——字符型数据

boolean——判断型数据

enum——枚举

null、true、false——值类型

4.线程（2个）

synchronized——线程同步（修饰方法、代码块，方法、代码块的同步）

volatile——线程同步（修饰属性，属性的同步）

5.异常（5个）

throw——抛出方法代码中的异常给方法自身。使用位置：方法中间

throws——抛出方法中的异常给调用者。使用位置：方法外部

try——捕获{}中代码是否有发生异常

catch——处理try捕获的异常

finally——不管有没有异常发生都会执行的代码块

6.返回（1个）

return

7.循环、条件（10个）

if、else、switch、case、break、default、continue、while、do、for

8.包（2个）

package、import

9.瞬时的（1个）

transient 关键字只能修饰变量,而不能修饰方法和类。

10.断言（1个）

assert

11.调用底层代码（C\C++）（1个）

native

12、不可变的——final（1个）

修饰属性、常量、局部变量、参数——作用：数据是不可改变的

修饰类——作用：修饰的类不能被继承

修饰普通方法——作用：修饰的方法不能被重写

13.静态的——static（1个）

修饰属性、常量

修饰内部类

修饰普通方法

作用：所有使用static关键字修饰的内容会最先执行。static修饰的内容在内存中只有唯一的一份（存储在静态内存空间中）。

14.格式规范——strictfp（1个） 即 strict float point (精确浮点)。

修饰类、接口或方法。

修饰方法时，该方法中所有的float和double表达式都严格遵守FP-strict的限制,符合IEEE-754规范。

## 成员变量 VS 局部变量

|   区别    |   成员变量   | 局部变量  |
|  ----  | ----  | ----  |
| 语法形式 |    属于类：可以被访问修饰符、static、以及 final 修饰    |    属于在代码块或者方法中被定义的变量：可以被 final 修饰    |
| 存储方式 |    如果是 static 修饰的，则存在heap的方法区的运行时常量池中。否则，存在 heap 上    |   存在 stack 上    |
| 生存时间 |    如果是 static 修饰的，则跟随类的生存时间。否则，跟随实例对象的生存时3间    |  跟随方法调用的生存时间    |
| 默认值 |    会自动以类型的默认值赋值，例外：final 修饰的成员变量必须显性赋值    |  不会自动赋值    |

## 静态方法(static) VS 实例方法
**静态方法的属性**

静态方法 意思就是属于整个 class 的 method。
main method 就是典型的 class method，我们在 跑程序的时候，例如 Test.java 时, java 虚拟机会自动 call Test.main(args)，从而启动程序。

**静态方法 VS 实例方法 的 使用规则**
1. 静态方法中不可以直接访问实例成员，因为是属于 object 私有的
2. 实例方法中既可以直接访问实例成员，也可以直接访问类成员
3. 实例方法中可以出现 this 关键字，静态方法不可以出现 this 关键字
```
    // file1: Student.java
    public class Student{
        // 类方法
        public static void printHello(){
            System.out.println("Hello");
        }

        // 实例方法
        public void printBye(){
            System.out.println("Bye"); 
        }
    }

    // file2: 
    public class Test{
        public static void main(){
            Student s = new Student();
            Student.printHello(); // Hello
            s.printHello(); // Hello

            s.printBye(); // Bye
            // Student.printBye(); // 会报错

        }
    }
```

## Overload(重载) VS Override(重写)
**重载**

**发生在同一个类中，使用同样的方法名，但不同参数类型、数量、以及顺序**。方法的返回值和访问修饰符可以不同。**在编译阶段就会挑选出具体执行的重载方法**。

**重写**

**发生在子类继承父类时，使用同样的方法名，同样的参数列表。** 子类的返回值类型 <= 父类的返回值类型、子类抛出异常类范围 <= 父类抛出异常类范围、子类的访问修饰符范围 >= 父类的访问修饰符范围。**在运行阶段才会选择执行重写的方法**

如果父类访问修饰符是 private/final/static，或者是父类的 constructor，则子类不能重写此方法。

## 可变长参数
从 Java5 开始，Java 支持定义可变⻓参数，所谓可变⻓参数就是**允许在调用方法时传入不定⻓度的参数**。就比如下面的这个 printVariable 方法就可以接受 0 个或者多个参数。

```
public static void method1(String... args) {
    //......
}
```

* 可变参数只能作为函数的最后一个参数，但其前面可以有也可以没有任何其他参数。
```
public static void method2(String arg1, String... args) {
    //......
}
```
* 遇到方法重载的情况，编译期间会优先匹配固定参数的方法
* Java 的可变参数编译后实际会被转换成一个数组