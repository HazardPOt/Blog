---
layout:     post
title:     Unity HealthBar制作方法
description:     Unity HealthBar制作方法
date:     2021-03-07
author:     POt
header-img:     img/post-bg-kuaidi.jpg
catalog: true
category: blog
tags:     
    -   
        -   

    -   

---

## 添加Image到UI中

**1.**  创建**Cavnas**，然后选择“ **UI”>“Image”** 。这将创建一个**Image GameObject**作为**Canvas**的子级，因为它需要在该**Canvas**上渲染。 

2.将**Sprite**拖入**Image**中，并设置**Set Native Size**，之后调整为合适大小。

[![6K5y6A.png](https://s3.ax1x.com/2021/03/07/6K5y6A.png)](https://imgtu.com/i/6K5y6A)

3.将anchor放到左上角以绑定

[![6K5rSH.png](https://s3.ax1x.com/2021/03/07/6K5rSH.png)](https://imgtu.com/i/6K5rSH)

4.在Health中创建子对象，放入角色图片

5.在Rect Transform中选择第四行第四列的，并将锚点框在角色图片周围，这样缩放时就会与Bar共同缩放并且不会变形

[![6K5Bfe.png](https://s3.ax1x.com/2021/03/07/6K5Bfe.png)](https://imgtu.com/i/6K5Bfe)

[![6K5wFO.png](https://s3.ax1x.com/2021/03/07/6K5wFO.png)](https://imgtu.com/i/6K5wFO)

## 创作Mask

**1.**  首先创建**Health GameObject**的**Image GameObject**子级，并将其命名为**Mask**

**2.**  调整其大小并移动其锚点，以使其适合您的**用户界面**中**健康栏**所在的空白位置：

[![6K56OI.png](https://s3.ax1x.com/2021/03/07/6K56OI.png)](https://imgtu.com/i/6K56OI)

**3.**  最后一步是移动**枢轴**，该**枢轴**是蓝色空圆圈要放在白色方块最左侧的中间。为什么？因为当您稍后使用代码调整大小时，将根据**数据透视表**进行调整。

**注意：例如，**如果将**枢轴**放在中间，并减小**20％**的大小，它将在每一侧调整**10％的**大小。通过将**枢轴**放在左侧，可以确保仅在右侧完成**20％的**调整大小。

**4.**  创建该Mask的新图像子级，添加蓝色血条。然后，不要像以前那样单击“**设置本机大小”** ，而是通过单击图像的“**矩形转换”中**的那个图标来打开锚点菜单：

[![6K520P.png](https://s3.ax1x.com/2021/03/07/6K520P.png)](https://imgtu.com/i/6K520P)

**5.**现在，在**按住Alt键的**同时，在出现的弹出窗口中单击右下角的图标：

**6.**现在再次单击“蒙版”，选择“**Add Component”**并搜索“**Mask”** 。添加它，然后取消选中“**显示蒙版图形”**以隐藏白色正方形。

[![6K5sld.png](https://s3.ax1x.com/2021/03/07/6K5sld.png)](https://imgtu.com/i/6K5sld)

## 编写脚本

```
public class UIHealthBar : MonoBehaviour
{
    public static UIHealthBar instance { get; private set; }
    
    public Image mask;
    float originalSize;

    void Awake()
    {
        instance = this;
    }

    void Start()
    {
        originalSize = mask.rectTransform.rect.width;
    }

    public void SetValue(float value)
    {				      
        mask.rectTransform.SetSizeWithCurrentAnchors(RectTransform.Axis.Horizontal, originalSize * value);
    }
}

```

在RubyController中修改ChangeHealth函数，添加Bar函数

```
UIHealthBar.instance.SetValue(currentHealth / (float)maxHealth);
```

## 在Hierarchy中赋给Health脚本即可完成

最后成品展示：

[![6KoUPO.png](https://s3.ax1x.com/2021/03/07/6KoUPO.png)](https://imgtu.com/i/6KoUPO)