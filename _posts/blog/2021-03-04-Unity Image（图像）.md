---
layout:     post
title:     Unity Image（图像）
description:     Unity Image（图像）
date:     2021-03-04
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

## 设置Raycast Target

  Image选项中有一个Raycast Target 选项，如果为选中状态，则无法单机到窗口后面的按钮。

## 设置UI图像为Sliced类型

  Image默认类型为Simple，在这种模式下，如果空间尺寸与贴图像素比例不一致，就会产生拉伸，如图所示:

[![6VXBcj.png](https://s3.ax1x.com/2021/03/04/6VXBcj.png)](https://imgtu.com/i/6VXBcj)

  另一种类型是Sliced（切片，俗称九宫格），在这种模式下，改变UI图像长宽，图像的边界像素可以保持不变，中间的像素拉伸显示。这种模式的优点是可以使用尺寸较小的贴图制作尺寸较大的按钮和窗口，并且可以适应不同的比例。

  设置为Slice类型，必须要在Sprite Editor 窗口手动设置贴图的边界尺寸，同时设置Pixels Per Unit的值。

## 设置UI图像为Tiled类型

  使用这种类型，在拉伸Img控件尺寸时，贴图会重复显示。

[![6VjPVP.png](https://s3.ax1x.com/2021/03/04/6VjPVP.png)](https://imgtu.com/i/6VjPVP)

## 设置UI图像为Filled类型

  Img控件的Filled类型非常适合用来制作进度条或者生命值显示，Fill Amount 值在0-1之间，通过设置这个可以只显示图像的一部分。Fill Method 提供了多种填充方式

```
public Image image()
{
    void Start()
    {
         image.fillAmount = 0.5f;
    }
}
```

## Mask（蒙版）

 蒙版的作用时使用一张贴图的Alpha作为剪切区域，UI上的目标图像与该贴图Alpha区域重叠的部分会被剪切掉。蒙版贴图必须含有Alpha，或使用一张灰度图来表示Alpha。

## Raw Image

  Raw Image 和 Image 功能类似，不同的是，它可以接受任何类型的2D贴图，而 Image 只能接受Sprite类型作为它的贴图