---  
layout:     post
title:      NavigationView 用法
subtitle:       NavigationView 用法
date:       2020-08-18
author:     POt
header-img: img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:       
    -   
    -   
    -   
---

常见类型： Form、List
* NavigationView form
  eg：
```
NavigationView{
            Form{
                TextField("事项内容", text: self.$title)
                TextField("事项内容", text: self.$title)
                TextField("事项内容", text: self.$title)
                TextField("事项内容", text: self.$title)
            }
```
![](https://pic.imgdb.cn/item/5f3b7d3914195aa594e4918a.jpg)

* NavigationView List


``` 
NavigationView{
            List{
                TextField("事项内容", text: self.$title)
                TextField("事项内容", text: self.$title)
                TextField("事项内容", text: self.$title)
                TextField("事项内容", text: self.$title)
            }
```

![](https://pic.imgdb.cn/item/5f3b7d5114195aa594e496ec.jpg)
