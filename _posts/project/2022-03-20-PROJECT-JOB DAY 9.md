# PROJECT-JOB DAY 9
## 数据库复习
https://github.com/wolverinn/Waking-Up/blob/master/Database.md
## 操作系统复习
https://github.com/wolverinn/Waking-Up/blob/master/Operating%20Systems.md
## Java基础PLUS
### 面对对象和面对过程的区别
* 面对过程
性能高，维护困难，代码不容易复用
* 面对对象
封装、继承、多态
### java的8种基本数据类型，以及int，char，long 各占多少字节数？
**char 2个字节，int 4个字节，long 8个字节**
![](https://camo.githubusercontent.com/faddb853104e55149697348516a1dfeee1541c4f37f9f1dc00c6f14c8f369f06/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f34363435312d373662353135616131376363313239322e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)

### int 和 Integer有什么区别
int是基本类型
Integer是包装类
### String、StringBuffer、StringBuilder区别
String **private final char value[]**
StringBuilder是线程不安全
StringBuffer是线程安全的
StringBuffer和StringBuilder都是继承自AbstractStringBuilder类，此类也是使用字符数组保存字符串**char[] value**，但是没有用final关键字修饰，所以这2种对象都是可变的。


对于三者使用的总结：
1. 操作少量的数据 = String
2. 单线程操作字符串缓冲区下操作大量数据 = StringBuilder
3. 多线程操作字符串缓冲区下操作大量数据 = StringBuffer

### 扩展：String为什么要设计成不可变的？
1、**字符串池的需求**
字符串池是方法区（Method Area）中的一块特殊的存储区域。当一个字符串已经被创建并且该字符串在 池 中，该字符串的引用会立即返回给变量，而不是重新创建一个字符串再将引用返回给变量。如果字符串不是不可变的，那么改变一个引用（如: string2）的字符串将**会导致另一个引用（如: string1）出现脏数据**。

2、**允许字符串缓存哈希码**
在java中常常会用到字符串的哈希码，例如： HashMap 。String的不变性保证哈希码始终一，因此，他可以不用担心变化的出现。 这种方法意味着**不必每次使用时都重新计算一次哈希码——这样，效率会高很多**。

3、**安全**
String广泛的用于java 类中的参数，如：网络连接（Network connetion），打开文件（opening files ）等等。如果String不是不可变的，网络连接、文件将会被改变——这将会导致一系列的安全威胁。操作的方法本以为连接上了一台机器，但实际上却不是。由于反射中的参数都是字符串，同样，**也会引起一系列的安全问题**。

### 什么是内部类？内部类的作用
在一个类的内部定义另外一个类，被嵌套的类就是内部类。

1，内部类可以用多个实例，每个实例都有自己的状态信息，并且与其他外围对象的信息相互独立
2，在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或者继承同一个类。
3，创建内部类对象的时刻并不依赖于外围类对象的创建
4，内部类提供更好的封装，除了该外围类，其他类都不能访问
内部类在编译完后也会产生.class文件，但文件名称是：外部类名称$内部类名称.class。

### final，finally，finalize的区别
final 关键字，用final定义后，常量不可改变，方法不可重写，类不可继承
finally，try..catch..finally，无论是否捕获异常，finally语句块都会被执行
finalize，java垃圾回收之前会调用此方法

### static
static变量：当被定义成static关键字后，内存中只存储一份，JVM只静态分配一次，
static代码块：在类初始化时执行一次
static方法：直接通过类名.方法名调用，只能访问同为static的变量和方法，不可使用this和super关键字。
static方法可以被继承但是不可以被重写（override）

### == equals hashcode区别
基本常量：== 比较值是否相等
引用变量：== 比较地址是否相等，equals()比较值是否相等
Hashcode是object类的一个方法，返回一个离散的int类型函数，**用在集合中，提升查找速度**
两者值相等，Hashcode必相等。值不相等，hashcode值可能不相等。hashcode值相等，值可能相等。hashcode值不相等，值必不相等。

### 哪些情况下的对象会被垃圾回收机制处理掉
1. **所有实例都没有活动线程访问。**
2. **没有被其他任何实例访问的循环引用实例。**
3. Java 中有不同的引用类型。判断实例是否符合垃圾收集的条件都依赖于它的引用类型。

### 父类的静态方法能否被子类重写是否可以被继承，为什么？
可以被继承，不可以被重写。

### 抽象类的定义
只要存在抽象方法的类就叫抽象类

### 接口和抽象类的区别？
1. 只要存在抽象方法的类就叫抽象类
2. 接口的变量是public static final类型的
3. 抽象类中可以有非抽象类方法，接口的方法必须都是抽象的
4. 接口中不能含有静态方法和静态代码块，抽象类可以有静态方法和静态代码块
5. 一个类只能实现多个接口，只能实现一个抽象类
6. 一个如果实现接口就得实现接口所有的方法，而抽象类不用……
7. 抽象类可以对方法进行实例化，而接口只能存在抽象方法。

### 接口的意义和特点
* 增加扩展性
* 提供一种规范
* 便于维护
* 弥补了Java不能多继承的缺陷
* 安全性

### 什么是多态，如何实现多态，Java中实现多态的机制是什么？
多态就是类运行时有多种状态，变量在编写程序时不确定，在运行程序时才能确定
重写（override），重载（overlode）
![](https://camo.githubusercontent.com/7a9fb4ecbe7b2a170b1b7185cfad9de4b44f2f10d8714db3e14db60b13b25c4a/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f34363435312d643934313339626562323337383166332e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)
### Serializable 和Parcelable 的区别
**Serializable**： Java序列化接口，其本质上使用了反射，序列化过程慢，内部执行大量的i/o操作，效率很低。
**Parcelable**：Android 序列化接口 效率高 使用麻烦 在内存中读写（AS有相关插件 一键生成所需方法） ，对象不能保存到磁盘中
**若仅在内存中使用，如activity\service间传递对象，优先使用Parcelable，它性能高。若是持久化操作，优先使用Serializable**

### List，Set，Map的区别
Set是最简单的一种集合，实现的Set接口，只能存放不重复的对象
Set接口主要实现两个类
* HashSet 按照哈希值来存取集合中的对象，存取速度快
* TreeSet 对集合中的对象实现排序

List的特征是线性方式存储，集合中可以存放重复对象
* ArrayList() 长度可以改变的数组，访问较快，存取较慢
* LinkedList() 采用链表数据结构，存取较快，访问较慢

Map是一种把键对象和值对象映射的集合，不允许有重复键值
Map没有继承Collection接口
* HashMap Map基于散列表的实现
* LinkedHashMap 与HashMap差不多，遍历时比HashMap略慢
* TreeMap 基于红黑树的实现，被排序

### ArrayMap与HashMap对比
1. 存取方式不同，HashMap内部维护一个HashMapEntry<Key,Value>[]的数组，每一对键值都要存储在一个对象中
2. 扩容效率不同，HashMap扩容时要重新new一个Hash对象，ArrayMap直接copy
3. ArrayMap可以紧缩数组，当执行remove或clear时，会紧缩数组，节约空间
4. ArrayMap采用二分查找

### HashMap和HashTable的区别
HashMap线程不安全，HashTable线程安全，HashTable效率较慢，且不允许有NULL值
### HashMap和HashSet的区别
HashMap实现Map接口，存储键值对，使用put()方法将元素放入map中，在放入时HashMap会调用hash函数计算key的hash值，若不相等则插入，相等则替换value值
HashSet实现Set接口，使用add()方法将元素放入set中，使用add时调用hash方法计算hash值，若不相等则插入，相等再调用equals方法判断相似值，不相等则插入

## 剑指 61 63
