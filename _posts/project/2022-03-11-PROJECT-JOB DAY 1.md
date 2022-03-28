---
layout:     post
title:     PROJECT-JOB DAY1
description:     Project JOB
date:     2022-03-10
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: project
tags:     
    -   
        -   

    -   

---
## Java基础Day1
1. **基本数据类型**
byte short int float long double char boolean
**1.1 包装类型**
包装类就是将一个基本数据类型封装成类，可以用这个类进行数据的转换、比较、最大值最小值等操作
**1.1.1 缓冲池**
包装类型内存使用 private static class IntegerCache，声明一个内部使用的缓存池
**1.2 BigDecimal**
为了科学运算而产生的，用于处理解决精度丢失的问题
2. **String**
**2.1 String StringBuffer StringBuilder 的区别**
**String** 用于单线程的小规模的字符串存储
**StringBufer** 用于单线程 操作 字符串缓冲区中大量数据
**StringBuilder** 用于单线程 操作 字符串缓冲区下大量数据
3. **final关键字**
*指声明数据为常量，编译时为常量，运行是被初始化后不能被改变的常量。*
    * **对于基本类型：** 声明后数值不能被改变
    * **对于引用类型：** 不能引用其他对象，但所引用的对象本身是可以修改的
    * **修饰不同位置的作用**
        * final修饰**方法**：方法不能被子类重写
        * final修饰**类**： 类不能被继承
        * private方法被隐式制定为final

4. **static关键字**
**静态变量**：又称类变量，指这个变量时属于这个类的，与类中其他成员共享静态变量，可直接通过类名+变量名访问
实例变量：创建一个实例就会产生一个实例变量
    ```
    public class A {
        private int x;         // 实例变量
        private static int y;  // 静态变量
    }
    ```
    **静态方法**：在类创建时静态方法就存在并创建了，不依赖于其他示例，必须有实现。
    **静态代码块**：在类创建时运行
    **内部静态类**：不用创建外部实例
    **关于类的初始化顺序:**
    * 父类（静态变量、静态语句块）
    * 子类（静态变量、静态语句块）
    * 父类（实例变量、普通语句块）
    * 父类（构造函数）
    * 子类（实例变量、普通语句块）
    * 子类（构造函数）

5. Object通用方法
**equals()**: 
    * 对于基本类型 "==" 判断两个值是否相等 
    * 对于引用类型 ”==“ 判断引用的地址是否相等， .equals()判断地址所指向的值是否相等
    * 如何重写equals方法
        * 判断是否是同一个对象的引用，是就返回true
        * 判断是否为同一类型的引用，不是返回false
        * Object进行转型
        * 判断转型每个关键域是否相等
            ```
            @Override
            public boolean equals(Object o) {
                if (this == o) return true;
                if (o == null || getClass() != o.getClass()) return false;
            
                EqualExample that = (EqualExample) o;
            
                if (x != that.x) return false;
                if (y != that.y) return false;
                return z == that.z;
            }
            ```
    **HashCode**:
    HashCode是JDK根据对象的地址、字符串、数字经过一些运算所得到的Int类型的数
    * 对象按照自己不同的特征尽量的有不同的哈希码，作用是用于快速查找
    * 另一个应用就是hash集合的使用

    重写hashcode方法:
    ```
        private String name;
	private int age;
	
    @Override
	public int hashCode() {
		int result = 17;
		result = result * 31 + name.hashCode();
		result = result * 31 + age;
		
		return result;
	}
    ```

    **toString()**
默认返回 对象名@4554617c 这种形式，其中 @ 后面的数值为**散列码**的无符号十六进制表示。

6. 继承
Java中有三个访问权限修饰符：private、protected、public
    * 不加修饰符，包内可见
    * protected子类可见
    * private类内可见
    * public所有均可见


7. 抽象类与接口
    * 抽象类
        * 如果一个类中包含抽象方法，则一定为抽象类
        * 抽象方法不能被实例化
        * 要用abstract进行声明
    * 接口
        * public static void
        * 方法可以有实现
    * 比较
    从设计层面上看
        * 抽象类是代码复用
        * 接口是对类的行为进行约束

        从使用层面上看
        * 一个类只能有一个父类，但是可以有多个接口
        * 接口是public static void类型
8. super关键字
super()可以执行父类的构造函数
9. 重写与重载
* 重写(override)
是指在继承体系中，*重写父类在方法声明上完全相同的方法*
重写有以下三个限制：
    * 子类方法的访问权限必须大于等于父类方法；
    * 子类方法的返回类型必须是父类方法返回类型或为其子类型。
    * 子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。

* 重载(overload)
存在于一个类中，指一个方法与已经存在的方法名称上相同，但是*参数、个数、顺序*至少有一个不同
**应该注意的是，返回值不同，其它都相同不算是重载。**
举例：
```
    public static int sayHello(int arg) {
        System.out.println("this is int " +arg);
        return 1;
    }

    public static float sayHello(int arg) {
        System.out.println("this is long " +arg);
        return 1.0;
    }

    public static void main(String[]args) {
        sayHello('a');
    }
```
这就不是一个重载，**要保证所传参数不同**
1. 反射
反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的.class不存在也可以加载进来


    *将一个类中私有方法进行调用*
* 反射的优点：
可扩展性：应用程序可以利用全限定名创建可扩展对象的实例，如com.demo.Test。
调试器和测试工具： 调试器需要能够检查一个类里的私有成员。测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。
开发工具：如IDEA开发工具可以从反射中获取类的信息，帮助开发人员代码编写。

* 反射的缺点：*如果一个功能可以不用反射完成，那么最好就不用。*
性能开销：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。因此，反射操作的效率要比那些非反射操作低得多。
安全限制：使用反射技术要求程序必须在一个没有安全限制的环境中运行。
内部暴露：反射破坏了封装性，可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性

11. 异常
Throwable 可以用来表示任何可以作为异常抛出的类，分为两种： **Error** 和 **Exception**。
*Error用来表示JVM无法处理的错误*
*Exception分为两种*
    * 受检异常 ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
    * 非受检异常 ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复
    * 运行时异常（runtime exception）出现编程错误，需要检查程序中的错误


try语句return问题：
如果try语句里有return，返回的是try语句块中变量值。详细执行过程如下：
* 如果有返回值，就把返回值保存到局部变量中；
* 执行jsr指令跳到finally语句里执行；
* 执行完finally语句后，返回之前保存在局部变量表里的值。
* 针对对象引用的返回，如果finally中有修改值，返回的是引用的对象。 如果try，finally语句里均有return，忽略try的return，而使用finally的return.


12. 泛型
泛型的本质是**参数化类型**，也就是所操作的数据类型**被指定为一个参数**。
**在集合中存储对象并在使用前进行类型转换是不方便的。泛型防止了那种情况的发生**。它提供了编译期的类型安全，确保你只能把正确类型的对象放入集合中，避免了在运行时出现ClassCastException。
使用T, E or K,V等被广泛认可的类型占位符。
13. 注解
 * @Override - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
* @Deprecated - 标记过时方法。如果使用该方法，会报编译警告。
* @SuppressWarnings - 指示编译器去忽略注解中声明的警告。


14. 线程
![thread](https://camo.githubusercontent.com/d9e4154205059eaa6f75ad531cb0130217efeb73e001a082702babc4f53ba36e/68747470733a2f2f67697465652e636f6d2f72626d6f6e2f66696c652d73746f726167652f7261772f6d61696e2f6c6561726e696e672d6e6f74652f6c6561726e696e672f636f6e63757272656e742f74687265616453746174652e6a7067)
* 新建（NEW）：创建后尚未启动。
* 可运行（RUNABLE）：正在 Java 虚拟机中运行。但是在操作系统层面，它可能处于运行状态，也可能等待资源调度（例如处理器资源），资源调度完成就进入运行状态。所以该状态的可运行是指可以被运行，具体有没有运行要看底层操作系统的资源调度。
* 阻塞（BLOCKED）：请求获取 monitor lock 从而进入 synchronized 函数或者代码块，但是其它线程已经占用了该 monitor lock，所以出于阻塞状态。要结束该状态进入从而 RUNABLE 需要其他线程释放 monitor lock。
* 无限期等待（WAITING）：等待其它线程显式地唤醒。
* 限期等待（TIMED_WAITING）：无需等待其它线程显式地唤醒，在一定时间之后会被系统自动唤醒。


    > 调用 Thread.sleep() 方法使线程进入限期等待状态时，常常用“使一个线程睡眠”进行描述。调用 Object.wait() 方法使线程进入限期等待或者无限期等待时，常常用“挂起一个线程”进行描述。睡眠和挂起是用来描述行为，而阻塞和等待用来描述状态。


**创建一个线程的开销**
JVM 在背后帮我们做了哪些事情：
* 它为一个线程栈分配内存，该栈为每个线程方法调用保存一个栈帧
* 每一栈帧由一个局部变量数组、返回值、操作数堆栈和常量池组成
* 一些支持本机方法的 jvm 也会分配一个本机堆栈
* 每个线程获得一个程序计数器，告诉它当前处理器执行的指令是什么
* 系统创建一个与Java线程对应的本机线程
* 将与线程相关的描述符添加到JVM内部数据结构中
* 线程共享堆和方法区域
15. 枚举类
