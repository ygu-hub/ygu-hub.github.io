---
layout: post
title: Java 基础八股文复习(3)
categories: JavaBasics
description: Java 基础八股文复习(3)
keywords: JavaBasics
---

## 面向对象三大特征

1. 封装（Encapsulation）
封装是指把一个对象的状态信息(也就是属性)隐藏在对象内部，不允许外部对象直接访问对象的内 部信息。但是可以提供一些可以被外界访问的方法来操作属性。达到合理隐藏，合利暴露的效果。
2. 多态（Polymorphism）
通过继承和实现关系，表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。
3. 继承（Inheritance）
    * 继承的定义：public class B extends A
        * A 类 被称为 父类，B 类被称为 子类
    * 继承的特点
        * 子类 可以继承 父类 的 非私有属性和方法
        * 子类对象 是由 parent 和 child class 共同构成的
        * Java 支持 单继承 和 多层继承，但不支持多继承
    * 继承的Advantage：
        * 减少重复代码的编写，增加代码的复用性

## 接口 VS 抽象类

* 共同点
    * 都不能被实例化
    * 都可以包含抽象方法
    * Java 8 之后，都可以使用 default 关键字，来定义默认实现的方法
* 不同点：
    * 接口目的是为了获得相应的行为（strategy 设计模式），抽象类是为了通过继承来实现代码复用（template 设计模式）
    * 一个类只可以继承一个抽象类，但可以继承多个接口
    * 接口中的成员变量只能是常量（public static final）

## 引用拷贝 VS 深拷贝 VS 浅拷贝

![](/assets/images/shallowDeepCopy.png)

**引用拷贝**
引用拷贝就是直接让两个不同的引用指向同一个对象

**浅拷⻉**
浅拷⻉会在堆上创建一个新的对象(区别于引用拷⻉的一点)，不过，如果原对象内部的属性是引用类型的话，浅拷⻉会直接复制内部对象的引用地址，也就是说拷⻉对象和原对象共用同一个内部对象。

**深拷⻉**
深拷⻉会在堆上创建一个新的对象，并完全复制整个对象，包括这个对象所包含的内部对象。

## Object 类

**Object 类是一个特殊的类，是所有类的父类**

**Object 类提供了 11 个方法**
getClass：返回当前运行时对象的 Class 对象，是一个 final method
toString
hashcode
equals
clone
notify, notifyAll, wait, finalize

## hashCode() 的作用
hashCode() 的作用是**获得哈希码，作用是确定该对象在哈希表中的索引位置**。

例子：
当把对象加进 HashSet 时，会先用 hashCode() 来判断对象加入的位置，如果发现有相同 hashCode 值的对象，这时会调用 equals() 来检查对象是否真的相同。

## hashCode() 和 equals()
两者都是用来比较对象是否相等的方法，只是**有了 hashCode() 就可以快速判断对象是否存在，然后再使用 equals() 来判断元素是否真的相同**，减少了查找成本和提高查找效率。

**注意！！** 两个对象的 hashCode 返回值相同不代表两个对象相等! 因为哈希碰撞的缘故，同一个 hashCode 方程可能会让多个对象传回相同的哈希值。

**重写 equals 时也必须重写 hashCode 方法的原因：**
因为两个相等的对象，hashCode 值也必须相等，因此重写 equals 时不重写 hashCode 有可能会导致两个对象的 equals 返回 true，但他们各自的哈希值不相等。

## String VS StringBuffer VS StringBuilder
**String**: 
* 不可变: String 是个 final 类，因此子类继承不了也更改不了 String 的值; 并且 String 使用 private final 字符数组保存字符串，并且没有提供任何修改字符串的方法
* 线程安全（不可变）
* 性能低（每次对 String 对象进行更改都会返回并重新指向一个新的 String 对象）

**StringBuilder**: 
* 可变，继承自 AbstractStringBuilder 类，使用字符数组（char[]）保存字符串
* 线程不安全（对方法及调用方法没加同步锁）
* 性能高（对同一个对象进行操作）

**StringBuffer**: 
* 可变，继承自 AbstractStringBuilder 类，使用字符数组（char[]）保存字符串
* 线程安全（对方法及调用方法加了同步锁）
* 性能高（对同一个对象进行操作）

## 字符串拼接要用 String “+” 还是 StringBuilder + append

**String “+”**：

**在编译后会被转换成创建 StringBuilder 调用 append 方法的实现，拼接完成后使用 toString() 获取 String 对象**。
但要注意的是，再循环内使用这种方式进行字符串拼接，会**导致创建过多的 StringBuilder 对象**。

**StringBuilder + append**

使用 StringBuilder 进行字符串拼接，就只会对应创建一个 StringBuilder 对象

## 字符串常量池
字符串常量池 是 JVM 为了提升性能和减少内存消耗针对字符串(String 类)专⻔开辟的一块区域， 主要目的是为了避免字符串的重复创建。

