---
layout:     post
title:     Unity 新GUI Canvas（画布）
description:     Unity 新GUI Canvas（画布）
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

>   因为Canvas使任何GUI的根节点，所有UI控件都是Canvas的子物体，所以创建UI的第一步是创建一个Canvas，我们可以将所有UI控件放入一个Canvas,也可以将不同类别的UI控件放入不同的Canvas，场景中允许有多个Canvas。

  

## 创建Canvas

【GameObject】=》【UI】=>【Canvas】

## 设置Canvas

  Canvas组件有一个Render Mode（渲染模式），包含三种模式

[![6VceTP.png](https://s3.ax1x.com/2021/03/04/6VceTP.png)](https://imgtu.com/i/6VceTP)

- Overlay Mode（叠加）渲染模式

  这种模式下所有UI永远显示在3D模型的前面，UI的显示与场景中的摄像机位置无关

  ~Pixel Perfect（抗锯齿模式）

  ~Sort Order（排序），值越大Canva界面就越优先显示。

- Camera 渲染模式

  【Screen Space - Camera】 在这种模式下必须指定场景中的一个摄像机

    使用相机模式可以将3D模型直接按空间位置放在UI前面，在UI前面显示出3D模型

-  World Space 渲染模式

    World Space 渲染模式的用法与Camera模式类似，也需要指定一个摄像机，但不同之处在于World Space可以旋转摄像机角度，使UI看起来带有透视效果

## Canvas的屏幕适应模式

  在手机平台上，屏幕分辨率各式各样，很难有一个统一的标准，因此对于2D的UI界面，如何能够做到适配各种分辨率是非常重要的。

  Canvas默认的UI Scale Mode是Constant Pixel Size，该模式无论屏幕分辨率多少，UI的大小始终如一，如果要适配不同的分辨率，不能采用这种模式。

  选择UI Scale Mode中的【Scale With Screen Size】模式，将根据预设的分辨率调整UI尺寸，将自动缩放屏幕分辨率

 [![6V7vg1.png](https://s3.ax1x.com/2021/03/04/6V7vg1.png)](https://imgtu.com/i/6V7vg1)

## Canvas层级内UI控件的排序

  UI控件的层级位置越下方，越优先显示出来。