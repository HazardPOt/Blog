---
layout:     post
title:     点击RecyclerView中的item滑动跳转到对应界面至屏幕最上方
description:     实习项目总结
date:     2022-07-27
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   





---

1. 创建一个接口Listener，在回调函数中定义我要从item中传出什么参数，用于回调进行参数的传递
2. 在Adapter中创建一个接口实例，并且创建set函数给这个接口附实例
3. 在RecyclerView中的Adapter里面itemClickListener，ClickListener中执行回调函数
4. 在VIewHolder中重写回调函数，拿到相关数据后在公共数据DataManager中通过字段匹配查询到是否有相应的item，如果有，那么就返回相应界面item的position
5. 拿到position之后就可以通过RecyclerView.scrollToPosition进行非线性滑动跳转，因为还有的Bar的存在，所以还要再往下滑动一个bar的高度
6. 针对刘海屏做优化：如果是刘海屏，那么就要进行相应的offset，向下滑动，不让刘海屏的摄像头挡住相应内容