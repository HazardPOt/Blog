---
layout:     post
title:     Unity Awake()和Start()区别
description:     Unity Awake()和Start()区别
date:     2021-02-14
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   
---  

# Unity Awake()和Start()区别



Awake()

当一个脚本实例被载入时Awake被调用。

Awake用于在游戏开始之前初始化变量或游戏状态。在脚本整个生命周期内它仅被调用一次.Awake在所有对象被初始化之后调用，所以你可以安全的与其他对象对话或用诸如 GameObject.FindWithTag 这样的函数搜索它们。每个游戏物体上的Awke以随机的顺序被调用。因此，你应该用Awake来设置脚本间的引用，并用Start来传递信息。Awake总是在Start之前被调用。它不能用来执行协同程序。


Start()

Start仅在Update函数第一次被调用前调用。Start在behaviour的生命周期中只被调用一次。它和Awake的不同是Start只在脚本实例被启用时调用。
你可以按需调整延迟初始化代码。Awake总是在Start之前执行。这允许你协调初始化顺序。