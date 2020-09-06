---  
layout:     post
title:      SwiftUI Identifiable协议
subtitle:       SwiftUI Identifiable协议
date:       2020-09-06
author:     POt
header-img: img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:       
    -   
    -   
    -   
---

这是另外一个让 List 知道怎么去标识每一行的 row是唯一的，我觉得我们在以后的使用中会更喜欢这种方式：

当去实现Identifiable 协议的时候，**我们需要去实现一个 id 的属性**，这个变量不一定需要是 Int 类型。这个协议只要求 id 的类型必须是 Hashable 的。(Int,Double,String等等这些都是Hashable的)

但实现了Identifiable 协议之后，我们不再需要去告诉 List 我们数组元素中的元素（符合协议的）是如何标识唯一的。

代码示例：

```struct SingleTodo: Identifiable{
    var title: String = ""
    var duedate: Date = Date()
    var isChecked: Bool = false
    var deleted: Bool = false
    **var id: Int = 0**
}
init(data: [SingleTodo]){
        self.Todolist = []
        for item in data{
            self.Todolist.append(SingleTodo(title: item.title, duedate: item.duedate, **id: self.count**)) 
//创建数组TodoList 使SingleTodo中的数据与 ID 匹配
            count = count + 1
    }
```
