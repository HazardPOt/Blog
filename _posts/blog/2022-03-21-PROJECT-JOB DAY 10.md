---
layout:     post
title:     PROJECT-JOB DAY10
description:     Project JOB
date:     2022-03-21
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: project
tags:     
    -   
        -   

    -   

---
## 数据库复习
https://github.com/wolverinn/Waking-Up/blob/master/Database.md
## 操作系统复习
https://github.com/wolverinn/Waking-Up/blob/master/Operating%20Systems.md

## JAVA基础
### HashMap和HashSet的区别
HashMap继承于Map方法，HashMap是以键值对的形式存储的，使用put()方法时，会先调用Object的HashCode方法，计算Key的HashCode，检查HashMap里有无相同的HashCode值，若无则插入、
HashSet继承于Set方法，HashSet是无序的，不可重复的。使用add()方法时会调用Object中的HashCode方法，计算Value的HashCode值，若不等则调用equals()方法比较值是否相等，是则插入，不是则拒绝

### ArrayList和LinkedList的区别，以及应用场景
ArrayList是数组形式有序存储，LinkedList是以双向链表的形式有序存储，ArrayList线程不安全
ArrayList读快写满，LinkedList读慢写快
若需要向索引各个位置进行无序的操作，ArrayList合适
若需循环遍历并插入，则LinkedList合适


ArrayList 是线性表（数组）
get() 直接读取第几个下标，复杂度 O(1)
add(E) 添加元素，直接在后面添加，复杂度O（1）
add(index, E) 添加元素，在第几个元素后面插入，后面的元素需要向后移动，复杂度O（n）
remove（）删除元素，后面的元素需要逐个移动，复杂度O（n）

LinkedList 是链表的操作
get() 获取第几个元素，依次遍历，复杂度O(n)
add(E) 添加到末尾，复杂度O(1)
add(index, E) 添加第几个元素后，需要先查找到第几个元素，直接指针指向操作，复杂度O(n)
remove（）删除元素，直接指针指向操作，复杂度O(1)

### 数组和链表的区别
数组是将元素在内存中连续存储的，查找效率高，但内存空间固定，容易越界
链表是动态申请内存空间的，根据需要动态申请或删除内存空间，数据的增加和删除比数组灵活

### HashMap如何均匀分布
hashcode & hashmap.size() - 1
HashMap的空间为什么是2的次方呢？因为这样不同的Key所与出来的index碰撞概率小，冲突小

### 为什么HashMap线程不安全（hash碰撞与扩容导致）
两个线程同时操纵HashMap，其中一个时间片用尽，进入wait状态，另一个扩容成功，等待第一个回复线程后，再次操纵Hashmap，会导致数组编程循环链表

### 自动装箱与拆箱
装箱：将基本类型用它们对应的引用类型包装起来；
拆箱：将包装类型转换为基本数据类型；

### 在一个静态方法内调用一个非静态成员为什么是非法的？
因为静态方法在类创建时就已经存在了，而非静态成员在写程序的时候还不知道要指向谁附多少值，只有程序运行后才能知道，所以让一个已经存在的方法调用一个不存在的变量时非法的。

### 构造方法有那些特性？
1. 名字和类名相同
2. 没有返回值，不能用void声明构造函数
3. 生成类的对象时自动执行，无需调用

### 泛型中extends和super的区别
extends确定上界，必须是T或者T的子类型
super确实下界，必须是T或者T的父类型

### 说下泛型原理，并举例说明
泛型就是将类型变成参数传入，使得可以使用的类型多样化，从而实现解耦。
*  类型擦除
泛型只在编译器中存在，而在JVM中不存在此概念
泛型规范了集合允许放入的类型，放入集合后都会将原类型擦除，替换为Object类型，并在输出的替换回原类型。
public T methodName(T extends Father t){}时除外。

### 说下你对Collection这个类的理解。
Collection是所有集合类的顶层接口，里面包含add, remove, clear等方法，它的子接口有两个，一个是Set，一个是List。特点是元素有序、有索引且可以重复

Vector:内部是数组数据结构，是同步的（线程安全的）。增删查询都很慢。
ArrayList:内部是数组数据结构，是不同步的（线程不安全的）。替代了Vector。查询速度快，增删比较慢。
LinkedList:内部是链表数据结构，是不同步的（线程不安全的）。增删元素速度快。

而Set的是特点元素无序，元素不可以重复
HashSet：内部数据结构是哈希表，是不同步的。
Set集合中元素都必须是唯一的，HashSet作为其子类也需保证元素的唯一性。

判断元素唯一性的方式：
通过存储对象（元素）的hashCode和equals方法来完成对象唯一性的。
如果对象的hashCode值不同，那么不用调用equals方法就会将对象直接存储到集合中；
如果对象的hashCode值相同，那么需调用equals方法判断返回值是否为true，
若为false, 则视为不同元素，就会直接存储；
若为true， 则视为相同元素，不会存储。
如果要使用HashSet集合存储元素，该元素的类必须覆盖hashCode方法和equals方法。一般情况下，如果定义的类会产生很多对象，通常都需要覆盖equals，hashCode方法。建立对象判断是否相同的依据。

TreeSet：保证元素唯一性的同时可以对内部元素进行排序，是不同步的。
判断元素唯一性的方式：
根据比较方法的返回结果是否为0，如果为0视为相同元素，不存；如果非0视为不同元素，则存。
TreeSet对元素的排序有两种方式：
方式一：使元素（对象）对应的类实现Comparable接口，覆盖compareTo方法。这样元素自身具有比较功能。
方式二：使TreeSet集合自身具有比较功能，定义一个比较器Comparator，将该类对象作为参数传递给TreeSet集合的构造函数

## Java高级
### 说说你对Java反射的理解
反射就是在类外可以拿到类中所有方法和成员。
反射的作用：开发过程中，经常会遇到某个类的成员变量，方法或属性是私有的，或只对系统应用开放，这里就可以利用Java的反射机制通过反射来获取所需要的私有成员或者方法。

获取类的Class对象实例 `Class clz = Class.forName("com.zhenai.api.Apple");` 
根据Class对象实例获取Constructor对象  `Constructor appConstructor = clz.getConstructor();`
使用Constructor对象的newInstance方法获取反射类对象 `Object appleObj = appConstructor.newInstance();`
获取方法的Method对象  `Method setPriceMethod = clz.getMethod("setPrice", int.class);`
利用invoke方法调用方法  `setPriceMethod.invoke(appleObj, 14);`

通过getFields()可以获取Class类的属性，但无法获取私有属性，而getDeclaredFields()可以获取到包括私有属性在内的所有属性。带有Declared修饰的方法可以反射到私有的方法，没有Declared修饰的只能用来反射公有的方法，其他如Annotation\Field\Constructor也是如此。

### 说说你对Java注解的理解
注解是通过@interface关键字来进行定义的，形式和接口差不多，只是前面多了一个@

注解的作用：
1）提供信息给编译器：编译器可利用注解来探测错误和警告信息
2）编译阶段：软件工具可以利用注解信息来生成代码、html文档或做其它相应处理；
3）运行阶段：程序运行时可利用注解提取代码

### Java中堆和栈
静态存储区：主要存放静态数据，全局static数据和常量
栈区：方法体内的局部变量都在栈上创建
堆区：通常就是指在程序运行时直接new出来的内存

#### 栈内存，堆内存的区别
在方法体内定义的（局部变量）一些基本类型的变量和对象的引用变量都是在方法的栈内存中分配的。
堆内存用来存放所有由new创建的对象（包括该对象其中的所有成员变量）和数组，在堆中分配的内存将由Java垃圾回收器来自动管理。

### Java内存回收机制
![](https://camo.githubusercontent.com/976960c4029887eadaee37e4a2aadfb99500226fbfa5ac3b33d01902d1e1ca34/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f34363435312d376633306363663865313161303432382e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)
### Java内存泄露的原因
内存回收的本质是判断一个对象是否还在被引用，让JVM误以为它还在被引用就会发生内存泄漏。
长生命周期的对象持有短生命周期对象的引用就很可能发生内存泄露
比如：静态集合类，若容器为静态的，则他们的生命周期与程序一致，容器中的对象持有短生命周期对象的引用，不能在程序结束之前被释放，则容易发生内存泄漏。

### 进程和线程的区别
扩展1：开启线程的三种方式？
分别是继承Thread类重写run方法、实现Runable接口和使用线程池

扩展2：如何保证线程安全？
synchronized加锁关键字；Object方法中的wait,notify；ThreadLocal机制 来实现的。

扩展3：如何实现线程同步？
1、synchronized关键字修改的方法。
2、synchronized关键字修饰的语句块
3、使用特殊域变量（volatile）实现线程同步

扩展4：run()和start()方法区别
start()方法被用来启动新创建的线程，而且start()内部调用了run()方法，这和直接调用run()方法的效果不一样。
当你调用run()方法的时候，只会是在原来的线程中调用，没有新的线程启动，start()方法才会启动新线程。

扩展5：如何控制某个方法允许并发访问线程的个数？
semaphore.acquire() 请求一个信号量，这时候的信号量个数-1（一旦没有可使用的信号量，也即信号量个数变为负数时，再次请求的时候就会阻塞，直到其他线程释放了信号量）
semaphore.release() 释放一个信号量，此时信号量个数+1

扩展6：什么导致线程阻塞？线程如何关闭？
阻塞式方法是指程序会一直等待该方法完成期间不做其他事情，ServerSocket的accept()方法就是一直等待客户端连接。
这里的阻塞是指调用结果返回之前，当前线程会被挂起，直到得到结果之后才会返回。此外，还有异步和非阻塞式方法在任务完成前就返回。
如何关闭：一种是调用它里面的stop()方法；另一种就是你自己设置一个停止线程的标记 （推荐这种）；
使用中断interrupt(),但是它只是传递中断请求消息，并不代表立马停止目标线程。

扩展7：线程的状态？
New:新建状态，new出来，还没有调用start
Runnable:可运行状态，调用start进入可运行状态，可能运行也可能没有运行，取决于操作系统的调度
Blocked:阻塞状态，被锁阻塞，暂时不活动，阻塞状态是线程阻塞在进入synchronized关键字修饰的方法或代码块(获取锁)时的状态。
Waiting：等待状态，不活动，不运行任何代码，等待线程调度器调度，wait sleep
Timed Waiting:超时等待，在指定时间自行返回
Terminated:终止状态，包括正常终止和异常终止

扩展8：线程中sleep和wait的区别
(1)这两个方法来自不同的类，sleep是来自Thread，wait是来自Object；
(2)sleep方法没有释放锁，仅仅释放了CPU资源或者让当前线程停止执行一段时间，
而wait方法释放了锁，需要调用notify后才能继续进入锁。
(3)wait,notify,notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用。


### 线程间通信
我们知道线程是CPU调度的最小单位。在Android中主线程是不能够做耗时操作的，子线程是不能够更新UI的。而线程间通信的方式有很多，比如广播，Eventbus，接口回掉，在Android中主要是使用handler。handler通过调用sendmessage方法，将保存消息的Message发送到Messagequeue中，而looper对象不断的调用loop方法，从messageueue中取出message，交给handler处理，从而完成线程间通信。

### Thread为什么不能用stop放停止
从SUN的官方文档可以得知，调用Thread.stop()方法是不安全的，这是因为当调用Thread.stop()方法时，会发生下面两件事：
1. 即刻抛出ThreadDeath异常，在线程的run()方法内，任何一点都有可能抛出ThreadDeath Error，包括在catch或finally语句中。
2. 释放该线程所持有的所有的锁。调用thread.stop()后导致了该线程所持有的所有锁的突然释放，那么被保护数据就有可能呈现不一致性，其他线程在使用这些被破坏的数据时，有可能导致一些很奇怪的应用程序错误。

## 剑指



