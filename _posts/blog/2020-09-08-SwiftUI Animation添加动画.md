---  
layout:     post
title:     SwiftUI Animation添加动画
description:     SwiftUI Animation添加动画
date:     2020-09-08
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
    -   
    -   
---  

# 隐式动画
> 隐式动画通过 **.animation()** modifier 指定。每当视图的某个 animatable 参数改变时，SwiftUI 会动画化呈现旧值到新值的变化。这些 animatable 参数包括尺寸，偏移量，颜色，缩放值，等等。

![](https://pic1.zhimg.com/v2-a3a5c897691e40ba2ea943976f8038c2_b.jpg)

常用:
```
.animation(.spring()) //弹簧类型动画
.animation(.easeInOut) //渐入动画
```

代码示例：

```
VStack{
   ForEach(self.UserData.Todolist){item in
        if !item.deleted{                  
           SingleCardView(index: item.id, editingMode: self.$editingMode, selection: self.$Selection)
                            .environmentObject(self.UserData)
                            .padding(.top)
                            .padding(.horizontal)// 循环SingleCardView
                            **.animation(.spring())**
                            //在Button或类的后面用.引用
                            .transition(.slide)
                        }
                         }
                       }
```

# 显式动画
> 显式动画通过 **withAnimation { ... }** 闭包指定。只有那些依赖 withAnimation 闭包中的值变化的参数才会被动画化。让我们举例说明：

![](https://pic2.zhimg.com/v2-13dc3dbe1c1a120c66d4e16955d4671d_b.jpg)

代码示例：
```
struct Example2: View {
    @State private var half = false
    @State private var dim = false

    var body: some View {
        Image("tower")
            .scaleEffect(half ? 0.5 : 1.0)
            .opacity(dim ? 0.5 : 1.0)
            .onTapGesture {
                self.half.toggle()

               ** withAnimation(.easeInOut(duration: 1.0)) {
                    self.dim.toggle()
                }** //只有透明度在animation闭包中，so只有它动画化       
            }
    }
}
```

**假如你需要禁用动画，可以使用 .animation(nil)**
