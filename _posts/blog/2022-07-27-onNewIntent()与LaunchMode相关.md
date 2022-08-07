---
layout:     post
title:     onNewIntent()与LaunchMode相关
description:     Android技能树
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

onNewIntent()，onNewIntent并不是页面进入每次都调用的，如果希望每次都调用，应该使用onResume()。

## 如果是A中再次启动A

在launchMode是standard时则依次调用了：

1. A1 onPause
2. A2 onCreate
3. A2 onStart
4. A2 onResume
5. A1 onStop

在launchMode非standard时则依次调用了：

1. A onPause
2. A onNewIntent
3. A onResume

## 如果是A启动B再启动A

在launchMode standard/singleTop时调用跟A再次启动A类似：

1. B onPause
2. A2 onCreate
3. A2 onStart
4. A2 onResume
5. B onStop

在launchMode singleTask/singleInstance时依次调用了：

1. B onPause
2. A onNewIntent
3. A onStart
4. A onResume
5. B onStop

## 如果是A启动B再返回A

1. B onPause
2. A onStart
3. A onResume
4. B onStop