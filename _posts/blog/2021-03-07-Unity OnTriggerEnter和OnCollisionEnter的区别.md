---
layout:     post
title:     Unity OnTriggerEnter和OnCollisionEnter的区别
description:     Unity OnTriggerEnter和OnCollisionEnter的区别
date:     2021-03-07
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

转自：https://blog.csdn.net/honey199396/article/details/51259586?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.baidujs&dist_request_id=1328602.27803.16150168172668245&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.baidujs

## 1、测试OnTriggerEnter和OnCollisionEnter的区别

  测试：如果两个物体A、B ，都有碰撞体collider 和 刚体（Rigidbody）。

  A或者B中**有一个**<u>勾选isTrigger或者两者都勾选isTrigger A和B都可以进入OnTriggerEnter方法，但是不可进入OnCollisionEnter方法。</u>

  A和B**都不勾选**isTrigger，<u>A和B能进入OnCollisionEnter方法但是不能进入OnTriggerEnter方法。</u>

  **结论： OnCollisionEnter方法必须是在两个碰撞物体都不勾选isTrigger的前提下才能进入，反之只要勾选一个isTrigger那么就能进入OnTriggerEnter方法。 OnCollisionEnter和OnTriggerEnter是冲突的不能同时存在的。**

## 2、OnTriggerEnter和OnCollisionEnter的选择。

  <u>如果想实现两个刚体物理的实际碰撞效果时候用OnCollisionEnter，Unity引擎会自动处理刚体碰撞的效果。</u>

  <u>如果想在两个物体碰撞后自己处理碰撞事件用OnTriggerEnter。</u>

## 3、一些技巧。

### 3.1：刚体（Rigidbody）的使用。

 两个碰撞的物体A 和 B

 现在我们就可以分两种模式来分析了，就是OnTrigger模式和OnCollision模式，因为上面已经详细介绍了两者是对立的模式， 

**在OnCollision模式下：**

  *测试1：如果只有A有刚体（Rigidbody），那么当A去碰撞B时，发现A弹开B没有动。A和B都进入OnCollisionEnter方法*

**结论1：只有刚体能实现真实的物理碰撞。**

  *测试2：如果只有A有刚体（Rigidbody），那么当B去碰撞A时，发现没有碰撞效果，A和B都没有进入OnCollisionEnter方法。*

**结论2：实现碰撞的条件是，发起碰撞方必须具有刚体。**

  这里我猜测了刚体是用来实现物理真实碰撞的Component，但是这个想法是错误的，因为OnTriggerEnter也必须有一个物体具有刚体，所以猜测刚体应该是一个判断是否实现碰撞的是与否的标志。



**在OnTrigger模式下：**

  A和B必须有一个有刚体（Rigidbody），A和B都可以进入OnTriggerEnter方法。
两个碰撞的物体A 和 B

  现在我们就可以分两种模式来分析了，就是OnTrigger模式和OnCollision模式，因为上面已经详细介绍了两者是对立的模式。

## 4、知识扩展。

上面的内容中有的实验是A，B有一个有刚体，有的实验是A，B都有刚体，那么为什么不干脆把两个物体都加刚体就没这么多麻烦了？

其实是这样的，真实游戏里面，有太多的物体，而这些物体如果都有刚体那么对系统的开销是很大的，如果可以减少一半的开销是很不错的选择。

比如地面就可以不设置刚体，因为地面是永远不动的，把人物设置刚体就可以实现真实的物理碰撞效果了。

