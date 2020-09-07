---  
layout:     post
title:     SwiftUI Array.firstIndex 用法
description:   SwiftUIArray.firstIndex 用法
date:     2020-09-07
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
    -   
    -   
---  

Returns the first index where the specified value appears in the collection.
**返回指定值在集合中出现的索引。**

/// After using `firstIndex(of:)` to find the position of a particular element
/// in a collection, you can use it to access the element by subscripting.
/// This example shows how you can modify one of the names in an array of students.
**在使用' firstIndex(of:) '查找集合中特定元素的位置之后，可以使用它通过下标访问或修改元素。这个示例展示了如何修改学生数组中的一个名称。**



```
var students = ["Ben", "Ivy", "Jordell", "Maxime"]
if let i = students.firstIndex(of: "Maxime") {
         students[i] = "Max"
     }
     print(students)
     Prints "["Ben", "Ivy", "Jordell", "Max"]"
```     
///Parameter element: An element to search for in the collection.
///Returns: The first index where `element` is found. If `element` is not
///found in the collection, returns `nil`.
**在集合中找到element时，返回下标，否则返回nil.**

    @inlinable public func firstIndex(of element: Element) -> Int?
