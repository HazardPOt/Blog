---  
layout:     post
title:      SwiftUI ObservableObject 应用
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

根据适用范围和存储状态的复杂度的不同，需要选取合适的方案。@State 和 @Binding 提供 View 内部的状态存储，它们应该是被标记为 private 的简单值类型，仅在内部使用。

前面所提到的@State 只能应用于层级内部或跨越一个层级

**ObservableObject** 和 **@ObservedObject** 则针对跨越 View 层级的状态共享，它可以**处理更复杂的数据类型**，其引用类型的特点，*也让我们需要在数据变化时通过某种手段向外发送通知* (比如手动调用 objectWillChange.send() 或者使用 @Published)，来触发界面刷新。

代码示例：
```
class Todo: ObservableObject{
    @Published var Todolist: [SingleTodo] //由SingleTodo 组成的数组
    var count = 0  //存储TodoList到底有多少元素
```

**作为储存数据的类Todo，它需要实时更新，这样就需要ObservableObject 随时“盯紧” Todo类和 Todolist数组，只要更新就会找到所有应用这些的界面然后将所有界面刷新一遍**
