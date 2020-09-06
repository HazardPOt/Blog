---  
layout:     post
title:      搬运修改：@State @Binding @ObservedObject @EnvironmentObject区别
subtitle:       搬运修改：@State @Binding @ObservedObject @EnvironmentObject区别
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

@State:
被修饰的属性会被自动转换成一对setter和getter，并且对这个属性赋值的时候会触发View的刷新，body会被再次调用，它申明的类型为值类型，*因此他不适合在不同对象间传递，状态值将会遵守值语义发生复制。适合在**同一结构体或类**内进行数值更新*
代码示例：

```
@EnvironmentObject var UserData: Todo
    
*@State var title: String = ""
@State var duedate: Date = Date()*
    
var body: some View {
        NavigationView{
            Form{
                Section(header: Text("事项")){
                    *TextField("事项内容", text: self.$title)*
                   * DatePicker(selection: self.$duedate, label: { Text("截止时间") })
*                }
                Section{
                    Button(action: {}){
                        Text("确认")
                    }
                    Text("取消")
                }
            }
        }
    }
```
@Binding:
它做的事情是将值语义的属性“转换”为引用语义。对被声明为 @Binding 的属性进行赋值，改变的将不是属性本身，而是它的引用，这个改变将被向外传递。(**解决@State的限制，用于不同试图之间的参数传递**)。**音译：绑定** 与类型相绑定并监视其状态是否改变，若改变则立即更新

@ObservedObject:
(**引用类型，处理更复杂的数据类型**) 被修饰的对象必须要实现 ObservableObject 协议，SwiftUI 会追踪 View 里面经过 @ObservableObject 修饰过的对象，当该对象里被 @Published 修饰的属性变换时，SwiftUI 会更新相关联的 UI 。
还是追踪监视对象，只不过可以支持更复杂的数据类型。
**多适用于类**

代码示例：

```
  @ObservedObject var UserData: Todo = Todo(data:[SingleTodo(title: "写作业",duedate: Date()),SingleTodo(title: "玩游戏", duedate: Date())]) // 此类型为自定义类Todo，如果传入数据改变，则引用UserData这个对象的View视图显示也相应改变
```

@Published: 被@Published修饰的属性发生变更时，会自动调用objectWillChange.send()这个方法，否则需要自己手动调用 objectWillChange.send()这个方法。
**多用于单个对象**
代码示例：
```
@Published var Todolist: [SingleTodo] //由SingleTodo 组成的数组
```

@EnvironmentObject:
应用于“**跳跃式”跨越多个 View 层级的状态**(有点类似于单例)，避免像ObservedObject一层一层的传递数据。
**在同一项目，不同源文件中进行引用监视**
