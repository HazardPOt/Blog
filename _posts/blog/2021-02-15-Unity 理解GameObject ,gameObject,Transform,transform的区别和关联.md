---
layout:     post
title:     Unity 理解GameObject ,gameObject,Transform,transform的区别和关联
description:     Unity 理解GameObject ,gameObject,Transform,transform的区别和关联
date:     2021-02-15
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

# Unity 理解GameObject ,gameObject,Transform,transform的区别和关联

转自：http://blog.csdn.net/lxl_815520/article/details/53638481

## 一.概述

  我们在monoDevelop中书写脚本语言时,GameObject与gameobject, Transform和transform是同时存在的如下图, 那么它们有什么区别和联系呢?
  **1>GameObject与gameObject**
  ![img](https://img-blog.csdn.net/20161214141407868?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTFhMXzgxNTUyMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
  **2>Transform与transform**
  ![img](https://img-blog.csdn.net/20161214141412665?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTFhMXzgxNTUyMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

## 二.GameObject与gameObject

   Gameobject是一个类型，所有的游戏物件都是这个类型的对象。
   gameobject是一个对象， 就跟java里面的this一样， 指的是这个脚本所附着的游戏物件
   **示例**:   

```csharp
public class ShowSliderValue : MonoBehaviour
{   
	private GameObject  obje; //定义GameObject类型的指针
	void Start(){
		Text  lal =gameObject.GetComponent<Text> (); //通过gameObject获取到Text组件.
		Debug.Log ("Text" + lal.text); //打印获取到组件的中的text的属性
	}
}
```

打印结果:
 Text中的值
  ![img](https://img-blog.csdn.net/20161214143035986?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTFhMXzgxNTUyMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
 输出台:

![img](https://img-blog.csdn.net/20161214143042908?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTFhMXzgxNTUyMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

注意:
 Text  lal =gameObject.GetComponent<Text> () 中不使用gameObject , 直接通过GetComponent<Text> (),也是可以的.

## 三.Transform与transform

   Transform是一个类，用来描述物体的位置，大小，旋转等等信息。
   transform是Transform类的对象，依附于每一个物体。也是当前游戏对象的一个组件(每个对象都会有这个组件).

## 四.transform与gameObject

   1>**二者的含义**
    transform :  当前游戏对象的transform组件
    gameobject ：当前游戏对象的实例

   2>**两者的联系和区别**
    \* 在unity中每个游戏对象都是一个gameobject. monodevelop中的gameobject就代表着本脚本所依附的对象. 每个gameobject都包含各种各样的组件，但从这点可以看出transform是gameobject的一个组件，控制着gameobject的位置，缩放，和旋转，而且每个gameobject都有而且必有一个transform组件
    \* gameobject.find用来获取场景中那个我们需要查找的对象（object）。而transform.find方法则是获取当前对象的子对象下我们需要获取的目标对象位置信息。
    \* 注意: 在update（）中尽量不使用find（）方法，影响性能.

   3>**gameobject.transform与transform.gameobject**
    \*  gameobject.transform,是获取当前游戏对象的transform组件.
      *所以在start函数中 gameobject.transform 和this.transform,指向的都是同一个对象。即：<u>gameobject.transform == this.transform == transform</u>*

​    \* transform.gameobject:可以这么理解为：获取当前transform组件所在的gameobect
​      所以在start函数中（）transform.gameobject == this.gameobject == gameobect
​    示例:

```csharp
public class ShowSliderValue : MonoBehaviour
{   
	private GameObject  obje; //定义GameObject类型的指针
	private Transform   trans;//定义Transform类型的指针
	void Start(){
		Debug.Log ("gameObject.name:" + gameObject.name);
		Debug.Log ("gameObject.transform.gameObject.name:" + gameObject.transform.gameObject.name);
		Debug.Log ("ThisGame.name:" + this.gameObject.name);
	}
}
```

 打印结果:

![img](https://img-blog.csdn.net/20161214161752032?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTFhMXzgxNTUyMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)