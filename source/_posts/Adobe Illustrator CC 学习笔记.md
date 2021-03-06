---
title: "Adobe Illustrator CC 学习笔记"
tags: ["Illustrator"]
abbrlink: 3a977c25
date: 2018-01-23 15:21:09
categories: ["学习笔记"]
---

>视频资料[Illustrator CC实例教程-我要自学网][01]，里面都是满满的干货，非常受用。

## Adobe Illustrator CC 介绍

Adobe Illustrator CC 是矢量图软件，而 Photoshop CC 是位图软件。矢量图可以无损缩放，体积也很小，但不适用于色彩复杂的图形创作。

<!--more-->

### 软件的基本操作方法

创建项目时，有两种颜色模式：RGB（红绿蓝-白，色光三原色、加法三原色）、CMYK（青品红黄-黑，色料三原色、减法三原色、印刷四分色）

"存储为"与"导出"操作，可以导出为多种格式的文件。常用的有：jpg（有损）、tif（无损）、png（透明）、psd（图层）、`pdf(一个项目中有多个画板时，导出成pdf格式，会存储在一个pdf文档中)`。

### *选择工具*与*直接选择工具*的区别

**共有**
1. `多边形圆角化`（鼠标左键拖动尖角内靠近的带圆心的小圆圈）；
2. `快速复制`（鼠标左键<kbd>+ Art</kbd>拖动图形）；
3. `移动`；
3. `框选`；
4. 右键选项一样（变换与再次变换(<kbd>Ctrl+D</kbd>)很方便）。

**选择工具** 主要针对整体
`鼠标左键`可以实施的操作：缩放（<kbd>+ Shift</kbd>等比缩放）、旋转；不方便操作锚点。

**直接选择工具** 主要针对锚点
`分割锚点`: 选择图像 &rArr; 选择锚点 &rArr; 点击上方菜单栏中的分割锚点图标 &rArr; 然后拖动线条，就分开了 ；
`连接锚点`: 框选住（或者用套索工具套索住）两个分开的锚点 &rArr; 点击上方菜单栏中的连接锚点图标；
>注意：将图片放大(<kbd>Art+鼠标上滚</kbd>)后，操作锚点更准确快速

`拖拉锚点`: 选中锚点后拖拉，使图像变形。

### 标准图像的绘制

1. 矩形工具
2. 圆角矩形（`直接鼠标左键拖动`后，仍按住左键不放，同时按住`方向上/左/右`可以调整圆角半径大小）
3. 椭圆工具
4. 多边形工具（`点击+输入`方法绘制，可设置边数；边数达到一定数时相当于圆形；n条边就有n个锚点）
5. 星形工具（`点击+输入`方法绘制，可设置边角数；绘制爆炸图形）
6. 光晕工具（选取部分可以模拟眼睛形状）

普通绘制方式有两种：**点击+输入**、**直接鼠标左键拖动**。
对于这两种方式而言，都是从`点击点`开始，从左至右、从上至下绘制，不方便绘制`同心图形`。

**正多边形或圆的绘制**：借助辅助键<kbd>Shift</kbd>

**同心图形的绘制**：鼠标移至上个图形的几何中心 &rArr; <kbd>Art</kbd>+`鼠标左/右键`拖动。所以要画同心圆就需要用到<kbd>Art</kbd>+<kbd>Shift</kbd>+`鼠标左/右键`拖动。
> (<kbd>Ctrl + C</kbd> - <kbd>Ctrl + F</kbd>)可以将复制的元素粘贴在原位置;按住<kbd>Art</kbd>缩放可以以中心为定点缩放。

### 钢笔工具（主要用于描线绘图）

1. 钢笔工具[钢笔+星号]
2. 添加锚点工具[钢笔+加号]
3. 删除锚点工具[钢笔+减号]
4. 锚点工具[尖角号]（锚点的手柄即切线）

**钢笔工具**
钢笔工具+辅助键<kbd>Art</kbd>,可以临时切换到锚点工具，松开<kbd>Art</kbd>后又会自动回到钢笔工具；

钢笔工具画线有两种方式，分别对应直线与曲线。
1. 直线：点击鼠标(取一个端点) &rArr; 松开鼠标 &rArr; `拖动鼠标` &rArr; 点击鼠标(取一个终点)；
2. 曲线：点击鼠标(取一个端点) &rArr; 松开鼠标 &rArr; `点击鼠标拖动`(点击取一个切点，拖动取的是切线方向) &rArr; 点击鼠标(取一个终点)或者继续`点击鼠标拖动`；

**上色**：
描线绘图的上色需要用到实时上色工具（<kbd>K</kbd>）与实时上色选择工具<kbd>Shift+L</kbd>。
>用选择工具选择需要上色的描绘图 &rArr; [顶部菜单栏-对象-实时上色-建立]<kbd>Ctrl+Alt+X</kbd> &rArr; 调用左侧菜单中的实时上色工具或实时上色选择工具操作；也可以省掉建立实时上色而直接调用实时上色工具（因为它会自动帮我们建立实时上色）。

### 路径菜单[顶部菜单栏-对象-路径]

> 一旦对闭合路径上色，则这个区域就成为了面，之前的路径就成了现在这个面的边。未上色前，可以在路径内部区域实施框选操作，而上色后则无法实施框选操作（点击拖动变成了移动面的操作）。此时要想只选择部分线条或锚点，则只有套索工具可以完成了。

1. 连接（只能连接同意路径或两个不同路径的两个端点）
2. 平均
  * 水平
  * 垂直
  * 两者均有（可以将多个锚点拉伸集中到一个点）
3. 轮廓化描边（执行后，放大可以看到路径或图形边界的外部又有了一个边界路径；结合曲率工具<kbd>Shift + ~</kbd>可以拉拽出漂亮的图形）
4. 偏移路径（按照现有路径或图形的形状，在其轮廓外画出一条与之隔有一定距离的闭合路径。与轮廓化描边比较类似。）*很方便有两条垂直交叉直线画出一个空心十字架哦*
5. 简化（减少锚点）
6. 添加锚点（对于所有直线，均匀的添加锚点）
7. 移去锚点（用的少）
8. 分割下方对象（对一条贯穿`闭合路径或图像`的路径实施此操作，可以将`闭合路径或图像`分割成两部分。例子：由正方形画七巧板）
9. 分割成网格



### 直线工具箱与描边窗口

1. 直线段工具
直线的绘制方式也有两种：**点击+输入**、**直接鼠标左键拖动**。前者可以更精确的确定长度及倾斜角度。

所有的线或路径都可以在描边窗口中调整线的粗细、虚实、箭头、端点与边角的形状、外观、渐变等等。`描边窗口`可在右侧菜单栏的三横杠图标调出，也可在顶部菜单栏 &rArr; 窗口 &rArr; 描边窗口找到。

2. 弧形、螺线形、矩形网格工具、极坐标工具

绘制网格有两种方法：
  1. 矩形网格工具：是由线段组成的网格。
  2. 路径--分割为网格：是由小矩形块组成的网格。（用于标准制图的网格用该方法来绘制）

### 画笔工具

可以置入图片(注意置入时把链接单选框取消)来自建画笔，也可以用项目内创建的图形来自建画笔。


## 基本绘图

### 路径查找器的使用`【在LOGO设计中很重要】`

路径查找器（窗口-路径查找器）主要用来做两个或多个路径或图形的剪裁、合并、拼接等等。
1. 形状模式
  * 联集（合并）
  * 交集 
  * 差集
  * 减去顶层(沿顶层边缘裁去顶层)
  前三种模式都是取顶层的颜色为操作后的颜色。

2.路径查找器
  * 分割（分割后，取消编组，就可以分离了）
  * 合并（可以很快就做出空心十字架）
  * 修边
  * 剪裁（与交集的区别在于留取了顶层图形的差集部分，只是将它的颜色去掉了）
  * 轮廓
  * 减去后方对象(沿底层边缘裁去底层)

### 渐变颜色的使用

1. 类型（线性、径向）
2. 角度
3. 渐变滑块
  * 删减：在渐变条的下边缘点击，可以添加渐变滑块；按住渐变滑块向下甩，可以删除它。
  * 控制面板：双击渐变滑块，可以调出它的控制面板。
    * 不透明度
    * 颜色：灰度、HSB、RGB、CMKL、Web安全RGB
    * 色板


## 测距

- **度量工具**（在吸管栏的工具里有个度量工具）
  任意单击两个位置，即可显示两位置的水平与垂直距离。

> 相关: Adobe Animate CC 2017 入门教程——[Animate动画案例教程][02] 





[01]: http://www.51zxw.net/list.aspx?cid=586
[02]: http://www.51zxw.net/list.aspx?cid=632