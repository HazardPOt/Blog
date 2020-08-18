---  
layout:     post
title:      Binding String 用法
subtitle:       Binding String 用法
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

* 要Binding的String类型前要加@State 表示要同步更新
* 引用String类型变量时前面要加$符号

代码示例：

```   @State var title: String = ""
    
    var body: some View {
        NavigationView{
            Form{
                TextField("事项内容", text: self.$title)
            }
        }
    }
```