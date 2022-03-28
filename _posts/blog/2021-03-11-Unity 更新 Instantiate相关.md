---
layout:     post
title:     Unity 更新 Instantiate相关
description:     Unity 更新 Instantiate相关
date:     2021-03-11
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

## Unity Resources.Load之后为什么需要Instantiate？

   引用https://blog.csdn.net/zhoumf/article/details/106150330

​    一个是加载到内存里，一个是实例化到场景中

​    这是unity的一个缺陷，本来一个gameobject加载之后，只需要addToScene就可以了，但它却选择做一次copy，然后在这个操作内附带addToScene。

​    它这样做的理由无非是，运行修改物体后，原来资源的那一份不会变化，还可以再次使用。所以就强制每次用必须复制。

然而，gameobject复制其实是一个相当耗时的操作，而且必须占用主线程，成为了一个重要的卡顿点。

​    预设(Prefab)是资源(Assets)的一种，场景(Scene)也是资源的一种，Unity运行是以场景为单位的，所有你要把**预设***克隆(Instantiate)* 到**场景**中才能运行物体的逻辑。

​    Instantiate本质上是对一个引用的值进行一个深度Copy（克隆到场景中）。

​    第一个GameObject prefab的Resources.Load是“解释”你要加载的东西是个GameObject并且加载到内存中。

​    第二个GameObject instance是对prefab进行一个深度copy克隆到场景中然后从Instantiate的返回值中持有他的引用。

​    prefab和instance在C#层面是一个类型，因为场景中的物体和资源中的预设都是 GameObject 类型的。

## 如何重复进行 Instantiate

  需要用到 **InvokeRepeating()** 函数

```
InvokeRepeating(methodName: string, time: float, repeatRate: float): void;
//methodName:方法名
//time:多少秒后执行
//repeatRate：重复执行间隔
```



实例：制作一个SpawnManager 物体生成器

```
    public Transform SpawnImg;
    public GameObject m_prefab;
    private float startDelay = 2;
    private float repeatRate = 2;

    void Start()
    {
        InvokeRepeating("SpawnObstacle", startDelay, repeatRate);
    }
    
    void SpawnObstacle()
    {
        Instantiate(m_prefab, SpawnImg.position, this.transform.rotation);
    }
```

结果展示

[![6tBc4O.png](https://s3.ax1x.com/2021/03/11/6tBc4O.png)](https://imgtu.com/i/6tBc4O)