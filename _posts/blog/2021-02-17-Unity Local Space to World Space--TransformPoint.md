---
layout:     post
title:     Unity Local Space to World Space--TransformPoint
description:     Unity Local Space to World Space--TransformPoint
date:     2021-02-17
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

# Unity Local Space to World Space--TransformPoint

转自https://www.cnblogs.com/koeltp/p/9716180.html  

```
 1 public class MoveX : MonoBehaviour
 2 {
 3     void Update()
 4     {
 5         if (Input.GetMouseButtonDown(0))
 6         {
 7             var two = new Vector3(2, 2, 2);
 8             var localSpace = transform.InverseTransformPoint(two);
 9             var worldSpace = transform.TransformPoint(localSpace);
10             log(localSpace);
11             Debug.Log("-----空间分割线------");
12             log(worldSpace);
13         }
14     }
15 
16     void log(Vector3 v)
17     {
18         Debug.Log(string.Format("x={0} y={1},z={2}", v.x, v.y, ));
19     }
20 }
```

在unity3d Editor中，我们创建一个cube的GameObject，它的坐标是(3,3,3)，然后MoveX脚本赋给此cube。

首先我创建了一个三维坐标都是2的点，然后把它传给了transform.InverseTransformPoint(two)，通过打印localSpace坐标点我们可以看到的信息为：x=-1 y=-1,z=-1，从这我们可以看出InverseTransformPoint的意思是：将transform的坐标作为原点(即世界空间的(3,3,3))，计算two坐标(即世界空间的(2,2,2))相对于transform原点的坐标。如下图所示：

![img](https://img2018.cnblogs.com/blog/130654/201809/130654-20180927225645041-2128926494.png)

接下来我将上面计算出来的相对于transform的坐标的localSpace赋给了transform.TransformPoint(localSpace)，然后计算得到一个worldSpace坐标，通过打印worldSpace坐标点我们可以看到的信息为：x=2 y=2,z=2。刚好还原得到我们原来two坐标点。