---
layout: post
title: Java 基础八股文复习--集合专题
categories: JavaBasics
description: Java 基础八股文复习--集合专题
keywords: JavaBasics, JVM, JDK, JRE
---

## 集合框架体系

1. **为什么需要集合**：单单使用 Array（数组）**灵活性**不够，因为固定了长度，想要增加元素和删除元素，都很麻烦。
比如：想要增加一个元素，而且超过了数组规定的长度，那就必须 new 一个新数组 :(

2. **Java的集合类主要分为两大类**：

    - **Collection（单列集合：也就是集合中放的都是单个的元素)**
    ![](/assets/images/javaBasics/单列集合.png)

    Collection实现类的遍历方式：
    * 第一种：因为 Collection 接口继承了 Iterable 接口，因此 Collection 实现类可以使用 Iterator 方法，来获取到 **Iterator** 对象，从而遍历集合元素。
    ```
        ArrayList list = new ArrayList<>();
        Iterator i = list.iterator();

        // 必须先用hasNext去检查是否还有元素，再用next去获取元素，否则会报错：NoSuchElementException
        i.next();   

        // 正确示范
        while(i.hasNext()) {
            i.next();
        }
    ```
    * 第二种：增强 for 循环 (for i : list) {...}，底层还是用的 Iterator 实现。
    * 还有一种：普通 for 循环。对于 set 接口实现类，因为不支持索引，因此不能使用普通for循环；list 接口实现类因为支持索引，因此可以使用普通 for 循环。

    - **Map（双列集合：集合中放的都是双个的元素<key, val>）**
    ![](/assets/images/javaBasics/双列集合.png)

## Java集合类详讲--**List接口**

- 结构：List接口 **继承自** Collection接口

- 常用实现类有哪些？**ArrayList、LinkedList、Vector**

- 特点
    * List实现类中的元素是**有序的**，即添加顺序和取出顺序一致
    * List实现类中的元素**可以重复**
    * List实现类中的每个元素都有其对应的顺序索引，即**支持索引**
    
- **List 常用方法**：
    * void add(int index, Object ele)               // 在index位置插入ele元素
    * boolean add()
    * boolean addAll(int index, Collection eles)    // 在index位置插入eles集合中的所有元素
    * int indexOf(Object obj)                       // 返回obj在当前集合中第一次出现的位置
    * int lastIndexOf(Object obj)                   // 返回obj在当前集合中最后一次出现的位置
    * Object remove(int index)                      // 移除指定index位置的元素，并返回该元素
    * Object set(int index, Object ele)             // 设置指定index位置的元素为ele
    * List subList(int fromIndex, int toIndex)      // 返回从[fromIndex,toIndex)位置的子集合

- **【重点】ArrayList实现类**
    * ArrayList permits adding all elements, including null。
    * ArrayList 底层是由**数组**来实现数据存储的
    * ArrayList 基本等同于 Vector，只是ArrayList是**线程不安全的，执行效率因此会高一些**
    * **[重点] ArrayList 扩容机制**
    ![](/assets/images/javaBasics/ArrayList属性.png)
    图中红色圈出来的，是 ArrayList 的重要属性 **transient Object[] elementData**，目的是实现 ArrayList 的动态扩容机制：
        - 当使用 ArrayList 无参构造器时：new ArrayList<>()，则初始 elementData 容量为 0。第一次添加元素时，则扩容 elementData 容量为 10。如需要再扩容，则直接扩容elementData 为 1.5 倍。
        ![](/assets/images/javaBasics/ArrayList扩容1.png)
        ![](/assets/images/javaBasics/ArrayList扩容2.png)
        ![](/assets/images/javaBasics/ArrayList扩容3.png)
        ```
            源码：
            // 无参构造器创建的 ArrayList 大小变化为：0 -> 10 -> 15 -> ...
            public ArrayList() {
                this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;   // DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {} 空数组
            }
        ```

        - 当使用 ArrayList 有参构造器并制定大小时：new ArrayList<>(10)，则初始elementData 容量为指定大小。如需要再扩容，则直接扩容elementData 为 1.5 倍。
        ```
            源码：
            // 有参构造器创建的 ArrayList 大小变化可能为：8 -> 12 -> 18 -> ...
            public ArrayList(int initialCapacity) {
                if (initialCapacity > 0) {
                    this.elementData = new Object[initialCapacity];
                } else if (initialCapacity == 0) {
                    this.elementData = EMPTY_ELEMENTDATA;   // EMPTY_ELEMENTDATA = {} 空数组
                } else {                                    // 不合法的参数会报错
                    throw new IllegalArgumentException("Illegal Capacity: "+
                                                    initialCapacity);
                }
            }
        ```

- **【重点】Vector 实现类**
    * Vector 是线程同步的，也就是**线程安全**的，Vector 类的操作方法都带有 synchronized 关键字，因此执行效率高一些（操作需要做线程安全检查）
    * **[重点] Vector 扩容机制**
    ![](/assets/images/javaBasics/Vector和ArrList扩容机制.png)
    - 当使用 Vector 无参构造器时：new Vector<>()，则初始 elementData 容量为 10。第一次添加元素时，则扩容 elementData 容量为 2 倍。
    ![](/assets/images/javaBasics/Vector扩容1.png)
    ![](/assets/images/javaBasics/Vector扩容2.png)
    ![](/assets/images/javaBasics/Vector扩容3.png)
    capacityIncrement 默认等于 0，代表一次扩容的大小，可以修改～
    - 当使用 Vector 有参构造器并制定大小时：new Vector<>(15)，则调用Vector<>(15, 0)，初始elementData 容量为指定大小。如需要再扩容，则直接扩容elementData 为 2 倍。

- **【重点】LinkedList 实现类**
    ```
        // LinkedList 类结构
        public class LinkedList {
            int size;
            Node<E> first;
            Node<E> last;
            long serialVersionUID;   // 序列化
        }
        // Node 类结构
        class Node<E> {
            Node<E> prev;
            Node<E> next;
            Node<E> item;
        }
    ```
    * LinkedList 底层实现了双向链表和双端队列的特点
    * 可以添加任意元素（可重复），包括null
    * **线程不安全**，没有实现线程同步
    * 因为 LinkedList 实现了 List 接口，可以用 Iterator 进行遍历

    * **LinkedList 增删查改方法源码**
        - 增
        ```
            // LinkedList 类
            // 第一步：
            public boolean add(E e) {
                LinkLast(e);
                return true;
            }
            
            // 第二步：
            void linkLast(E e) {
                // 创建一个新节点，prev 指向 last，next 指向 null
                Node<E> l = last;
                Node<E> newNode = new Node<>(l, e, null);
                // 更新尾节点，指向新节点
                last = newNode;

                // 如果链表为空
                if(last == null) {
                    first = newNode;    // 则也需要更新头节点
                }
                else{
                    l.next = newNode;   // 否则，将链表原本尾部的 next 指向新节点
                }
                size++; // 代表链表大小加一
                modCount++; // 代表链表更改次数加一
            } 
        ```
        - 删
        ```
            // 删除链表第一个元素
            public E remove() {
                return removeFirst();
            }

            public E removeFirst() {
                Node<E> f = first;
                // 如果链表为空，抛异常
                if(f == null) {
                    throw new NoSuchElementException();
                }
                // 如果不为空，删除头节点
                return unlinkFirst(f);
            }

            // 默认链表不为空的 helper method
            private E unlinkFirst(Node<E> f) {
                // 提前存下删除的头节点的值，最后作为返回值返回
                E element = f.item;
                // 新的头节点
                Node<E> next = f.next;

                // 更新 first 指向新的头节点
                first = next;

                // 如果删除头节点后，链表变成空链表了，那就要更新尾节点指向 null
                if(next == null) {
                    last = null;
                }
                // 如果不是，就要让新的头节点的 prev 指向 null
                else {
                    next.prev = null;
                }
                // 更新大小和操作数
                size--;
                modeCount++;

                return element;
            }
        ``` 
- **[需要掌握] LinkedList 和 LinkedList 的比较**
    ![](/assets/images/javaBasics/ArrayList和LinkedList比较.png)
总结：因为ArrayList底层是由数组实现，因此增删的效率低 O(n)。但根据索引的改查的时间复杂度是 O(1)，因为直接可以定位到元素(Random Access)；**相对的**，因为 LinkedList 底层是双向链表，因此增删的效率普遍是 O(1)，但根据索引的改查的时间复杂度是 O(n)，因为要一个个往后遍历直到目标节点(Sequential Access)。

## Java集合类详讲--**Set接口**

- 结构：Set 接口 **继承自** Collection接口

- 常用实现类有哪些？**HashSet，TreeSet**

- 特点【注意和ArrayList的区别哦】
    * **无序**（添加和取出的顺序不一致，但取出的顺序是固定的），因此**不支持索引**（你都是无序的了，还怎么用索引获取对应元素对吧，没意义）
    * 不允许重复元素，所以最多包含一个null
    * Set遍历方式有：1）使用 Iterator 进行遍历 2）增强 for 循环

- **Set 常用方法**:
    * boolean add(E e)
    * remove(Object)
    * size()
    * isEmpty()
    * contains(Object)
    * Iterator<E> iterator()

- **【重点】HashSet 实现类**
    * 底层由 HashMap 实现，而 HashMap 是由 数组 + 链表 + 红黑树实现
    ```
        // HashSet 构造方法：
        public HashSet() {
            map = new HashMap();    // 注意这里！
        }
    ```
    * 所以这里在分析 HashSet 源码
    