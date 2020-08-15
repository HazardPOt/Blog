---  
layout:     post
title:      ObservedObject 和 Stated 有何不同？
subtitle:       ObservableObject 语法应用
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

定义：ObservedObject是2020年新的属性装饰器，观测对象修饰器，被ObservedObject修饰的变量只要有变化就会自动更新

@State 和 @ObservedObject 有什么区别？
@State用于View内部，@ObservedObject用于外部。例如数据存储在数据库中，我们就需要用@ObservedObject了。
