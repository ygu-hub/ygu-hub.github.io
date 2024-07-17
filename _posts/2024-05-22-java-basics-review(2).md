---
layout: post
title: Java 基础八股文复习(2)
categories: JavaBasics
description: Java 基础八股文复习(2)
keywords: JavaBasics
---

## Java基本数据类型

Java 中一共 8 种基本数据类型，每一种基本类型都有对应的 wrapper class 包装类

- 6 种数字类型：
    * 4 种**整数型**：
        * byte: 1 byte ｜ 包装类：Byte
        * short: 2 bytes ｜ 包装类：Short
        * int: 4 bytes ｜ 包装类：Integer   （**整数常量默认是int**）
        * long: 8 bytes ｜ 包装类：Long
    * 2 种**浮点型**: 
        结构：符号位 + 指数位 + 尾数位（可能丢失）
        * float: 4 bytes ｜ 包装类：Float
        * double: 8 bytes ｜ 包装类：Double （**浮点常量默认是double**）
- 1 种**字符型**：
    * char ｜ 包装类：Character
        使用不同**编码表**，需要占用的字节数不同：
        - ASCII：统一 1 byte
        - Unicode：统一 2 bytes
        - UTF-8：字母 1 byte，汉字 3 bytes

- 1 种**布尔型**：
    * boolean: 1 byte，但取决于 JVM 的具体实现 ｜ 包装类：Boolean

## 自动类型转换

定义：java程序进行变量赋值或者运算时，精度小的类型 会自动转换成 精度大的 数据类型
【重要，需要背以下细节】
**char -> int -> long -> float -> double**
**byte -> short -> int -> long -> float -> double**

1. 有多种类型的数据混合运算时，系统先把所有数据换成精度最大的数据类型，再进行计算
2. 不能把精度大的数据类型赋值给精度小的数据类型，就像不能强行把大的物体塞到小的容器中一样
2. (byte, short) 和 char 之间不会自动类型转换
3. byte，short，char 三者可以进行计算，**但在计算时首先转换为int类型**
4. char 可以保存 int 的常量值，比如 char c1 = 100; **但不能保存int的变量值，比如 int m = 100; c1 = m; 需要强制类型转换！**

## 强制类型转换
可能会导致数据溢出或者精度损失问题

## 字符串和基本类型的转换
从**字符串**转换成**基本数据类型**，就是用基本数据类型包装类的**parseXXX(String s)**方法即可。
**注意**：
1. 字符类型不能用这个方法转换，而是直接用 String 类的 charAt 方法。就比如 “abc” 也不能转换成一个单独的字符。
2. 要注意转换的格式要正确，虽然编译不会有问题，但运行时会抛异常。

例子：
```
    long l = Long.parseLong("12.1");
    int i = Integer.parseInt("100");
```

## 基本类型 VS 包装类型

|   区别    |   基本类型   | 包装类型  |
|  ----  | ----  | ----  |
| 默认值  |   默认值  |  null |
| 泛型  |   不可用于泛型  |  可用于泛型 |
| 存储位置  |   如果是非 static 的成员变量，存储在 heap 上  |  不被 static 修饰时，存储在 heap 上 |

## 包装类型的缓存机制

Java 基本数据的包装类（浮点类没有）都用了**缓存机制**，从而可以节省分配的内存，从而提升性能。

```
Integer缓存源码：
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);  // 范围以外的会再分配新内存
}
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static {
        // high value may be configured by property
        int h = 127; 
    }
}
```

Byte , Short , Integer , Long 这 4 种包装类默认创建了数值 [-128，127] 的相应类型的缓存数
据，Character 创建了数值在 [0,127] 范围的缓存数据， Boolean 直接返回 True or False。

## 自动装箱（boxing）和自动拆箱（unboxing）

**自动装箱**：将基本数据数据类型 包装成 对应的引用类型，其实就是**调用了包装类的 valueOf(基本数据类型参数) 方法**。
**自动拆箱**：将引用类型 转换为 对应的基本数据类型，其实就是调用了**调用了包装类的 xxxValue() 方法**。

```
例子：
    Integer i = 10 等价于 Integer i = Integer.valueOf(10) 
    int n = i 等价于 int n = i.intValue() ;
```

## 浮点数运算时精度丢失的风险和解决方法

**精度丢失问题的原因**：这个和计算机保存浮点数的机制有很大关系。我们知道计算机是二进制的，而且计算机在表示一个数
字时，宽度是有限的，无限循环的小数存储在计算机时，只能被截断，所以就会导致小数精度发生损
失的情况。这也就是解释了为什么浮点数没有办法用二进制精确表示。

**解决办法**：使用 **BigDecimal** 来进行浮点数运算，不会造成精度丢失。

## 超过 long 整数型数据的表示方法：

**使用BigInteger类**：BigInteger 内部使用 int[] 数组来存储任意大小的整形数据，但相对于常规整数型运算来说， BigInteger 类的运算效率偏低
```
import java.math.BigInteger;
BigInteger bigInteger = new BigInteger("12345678901234567890");
```