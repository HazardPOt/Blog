---  
layout:     post
title:      ScrollView 滚动条
subtitle:       ScrollView 滚动条的用法
date:       2020-08-16
author:     POt
header-img: img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:       
    -   
    -   
    -   
---

* 为什么要使用ScrollView滚动？
因为屏幕空间有限又要放下更多显示内容
* 详解：
 ScrollView()//默认模式
 
```
ScrollView(<#T##axes: Axis.Set##Axis.Set#>, showsIndicators: <#T##Bool#>, content: <#T##() -> _#>)
```

axes: Axis.Set -> 设置横向滚动还是纵向滚动
纵向.vertical 横向.horizontal

showsIndicators: -> 是否需要滚动条
Bool类型 True or False
