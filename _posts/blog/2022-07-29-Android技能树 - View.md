---
layout:     post
title:     Android技能树 - View
description:     Android技能树
date:     2022-07-29
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   






---



## ViewRoot

实现类是ViewRootImpl，是DecorView和WindowManager之间的纽带，是DecorView的管理者

## View的绘制流程

VIew的绘制流程是从`ViewRoot`的`performTraversals`方法开始的，`performTraversals`方法开始依次调用`performMeasure`,`performLayout`和`performDraw`三个方法

performMeasure() -> measure() -> onMeasure() 所以要重写onMeasure()方法

### 顶级View是DecorView

DecorView是一个FrameLayout，里面包含一个竖向的LinearLayout，LinearLayout中有一个titleBar和具体的content，**我们在Activity里面设置布局setContentView就是把我们的布局加到这个id为android.R.id.content的FrameLayout里面**

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/1/31/1614ac2079ae78e3~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

## View的大小

我们用MeasureSpec来决定我们的View大小，那我们的MeasureSpec和气球一样，也受到二个因素的影响：

1. ViewGroup的影响
2. 自身的LayoutParams

总结起来就是一句话：在测量过程中，系统会将View的LayoutParams根据父容器ViewGroup所施加的规则下，转换得出相对应的MeasureSpec，然后根据这个MeasureSpec来测量出View的高/宽。

### MeasureSpec知识

MeasureSpec 测量规格，是View宽高的决定因素

1. MeasureSpec是由SpecMode和SpecSize组合成的。
2. SpecMode的种类：UNSPECIFIED,EXACTLY,AT_MOST。
3. 普通的View是由父容器限制和自身的LayoutParams来生成相应的MeasureSpec，而DecorView因为是顶层View了。我们可以想象哪来的父容器啊，在外面一层就直接是屏幕了，所以是由屏幕的尺寸和自身的LayoutParams决定。

#### MeasureSpec的决定因素

* 如果是顶级View（DecorView），其MeasureSpec由顶级View 的窗口尺寸和自身的LayoutParams决定
* 如果是普通View，其MeasureSpec由父容器的MeasureSpec和自身的LayoutParams决定

![](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/1/31/1614b40f3e2d81e1~tplv-t2oaga2asx-zoom-in-crop-mark:3024:0:0:0.awebp)

## View的测量

去哪里创建MeasureSpec并给子View去做约束呢，在onMeasure()方法中

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);
}       
```

这里传入的`widthMeasureSpec`和`heightMeasureSpec`为当前重写方法所在View进行一系列操作后得到的`MeasureSpec`

最后通过`setMeasureWidth()`,`setMeasureHeight()`两个方法或者`setMeasuredDimension()`来设置测量的宽和高

我们可以不是通过正规的测量过程来决定测量的宽和高，我们就是任性的直接定了宽高是100。但是这样就不符合规则流程了。而且做出来的东西也不会特别好。比如这时候，你在xml中对你的view设置`match_parent`,`wrap_content`,`200dp`就会都无效，因为代码最后都是用了100。

## onMeasure()方法的构成

自定义View是要重写`onMeasure()`方法的，再仔细分析一下

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    
    //我们一般会自己写的代码
    ........
    ........
    .......
    
}
```

### super.onMeasure()分析

#### 情况一：我们的自定义View直接继承了View.java：

super.onMeasure():

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    setMeasuredDimension(
        getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),      
        getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec)
    );
}
```

调用了`setMeasureDimension()`方法进行宽高的设置

我们可以看到主要是三个方法：

1. 先`getSuggestMinimumWidth`方法获取了某个值
2. 再通过`getDefaultSize`方法来对第一步获取到的值和约束一起处理后得到的最终值
3. 通过`setMeasureDimension`方法把我们最终的值给赋值进去

回头看`getSuggestMiniumWidth()`方法

```java
protected int getSuggestedMinimumWidth() {
    return (mBackground == null) ? mMinWidth : max(mMinWidth, mBackground.getMinimumWidth());
}
```

如果View中没有设置background，那么就会取xml中设置的`android:width`的值，如果设置了background，就会获取`mBackground.getMinimumWidth()`的值（是background的Drawable的原始宽度），然后返回二者最大值

#### 如果自定义View继承了现有的控件，比如ImageView.java

```java
public class Image2View extends ImageView {
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }
}

```

这时候`onMeasure()`中的`super.onMeasure`则调用的是`ImageView`的`onMeasure()`

我们发现如果我们的View直接继承ImageView，ImageView已经运行了一大堆已经写好的代码测出了相应的宽高。我们可以在它基础上更改即可。

比如我们的Image2View是一个自定义的正方形的ImageView,：

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {

    //这里已经帮我们测好了ImageView的规则下的宽高，并且通过了setMeasuredDimension方法赋值进去了。
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);

    //我们这里通过getMeasuredWidth/Height放来获取已经赋值过的测量的宽和高
    //然后在ImageView帮我们测量好的宽高中，取小的值作为正方形的边。
    //然后重新调用setMeasuredDimension赋值进去覆盖ImageView的赋值。
    //我们从头到位都没有进行复杂测量的操作，全靠ImageView。哈哈
    int width = getMeasuredWidth();
    int height = getMeasuredHeight();
    if (width < height) {
        setMeasuredDimension(width, width);
    } else {
        setMeasuredDimension(height, height);
    }
}
```

## 具体实现自定义View的测量

### 如果是继承现有的控件，比如ImageView，实现一个正方形的ImageView

```java
public class Image2View extends ImageView {

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        //这里的super.onMeasure()方法里面，已经是调用了ImageView的onMeasure()方法。
        //所以已经进行了测量了。并且在这个方法最后调用了setMeasuredDimension(widthSize, heightSize);
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        //所以你不写任何东西，这个测量结果都已经确定过了，因为已经执行过了setMeasuredDimension。
        //但比如你想要在ImageView的基础上，让这个ImageView变成一个正方形的ImageView。
        //因为测出来的宽高可能不同，是一个矩形。我们就需要手动的再去设置一次宽和高。
        int width = getMeasuredWidth();//获取ImageView源码里面已经测量好的宽度
        int height = getMaxHeight();//获取ImageView源码里面已经测量好的高度
        if (width < height) {
            setMeasuredDimension(width, width);
        } else {
            setMeasuredDimension(height, height);
        }
    }
}
```

### 直接继承了View

如果是直接继承了View，就可以执行View测量宽高三部曲：

1. 设置默认值：因为wrap_content只是说不超过某个最大值，如果不设置默认值，那么效果与matchparent一样
2. 调用resolveSize()方法：把MeasureSpec和默认值放进去
3. 调用setMeasureDimension方法赋值宽和高
