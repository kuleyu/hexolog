---
title: win10有线网卡及无线网卡mac地址的伪装
comments: true
abbrlink: c6b45260
date: 2018-01-07 01:01:42
categories: 杂七杂八
tags:
- windows
- win10
- mac地址
---

> mac地址是硬件地址，又被称物理地址是用来定义网络设备的位置。现在很多学校喜欢把用户账号与电脑的mac地址绑定，以确定一台电脑登录一个账号。其实只需要把mac地址修改成他电脑的mac地址就可以使用小伙伴的网线，用小伙伴的账号登录了。
> 另外当发现自己笔记本连的WiFi被限速或者被拉黑连不上了，同样可以修改无线网卡的mac地址来解除这些限制，因为路由器的限速及拉黑对象都是基于mac地址来识别的。下面看详细教程。

<!--more-->

**查看mac地址**
打开命令提示符工具（<kbd>Win</kbd>+<kbd>R</kbd>输入*cmd*调出），进入cmd窗口,输入命令“*ipconfig /all* ”,回车如下：
![cmd-mac][01]
**查看网卡驱动名**
设备管理器 --> 网络适配器: 
![网络适配器][02]

## 伪装有线网卡mac地址
设置 --> 网络和Internet --> 更改适配器选项（或者：控制面板 --> 网络和 Internet --> 网络连接）进入如下窗口：
![网络连接][03]
右键单击以太网 --> 属性，进入如下窗口：
![以太网属性][04]
单击配置，进入如下窗口：
![以太网属性配置][05]
选择 高级 --> Network Address，在“值”里面填写你想要的任意mac地址。注意：
> 1. 每两位之间不需要短横线“-”；
> 2. 第二位数值只能是 “2”，“6”，“A”，“E”中的一个，否则设置无效

接下来按照**查看mac地址**方法验证一下，会发现以太网的mac地址确实变成我们刚才设的那个值了。

## 伪装无线网卡mac地址
当你用同样的步骤去修改无线网卡mac地址时，你会发现找不到 “Network Address”属性，因为对于绝大多数无线网卡,其驱动以及windows的支持并不把修改mac作为可行,这时我们就需要通过修改注册表来添加“Network Address”属性。下面介绍如何设置。~~***请先保证每步都看懂了,备份注册表后,再进行尝试。注册表修改要慎重,尤其是本次涉及windows的网络模块,很可能做错了导致启动异常,所以保证看懂了,会用了,再去尝试。***~~

**打开注册表编辑器**
<kbd>Win</kbd>+<kbd>R</kbd>输入*regedit*调出，
![打开注册表编辑器][06]
定位到\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}位置，如下：
![有线网注册表配置位置][07]
并查看此下所有主键（0000，0001，0002，......，0017）,一般都有十个左右的主键,由于我个人笔记本用的外接网卡比较多,所以主键多点，这里有17个。从0000起,鼠标选中,查看右侧,查看名称为**DriverDesc**的项目数据,看看是不是刚刚在设备管理器中看到的网卡名称。看我的,我在0001中找到了自己的有线网卡**Realtek PCle GBE Family Controler**,0001主键中还有一项**Network Address**,其值是我的mac地址,此处可以修改固定网卡mac。
![网卡注册表配置位置][08]
对比下0001/Ndi/params/主键下的内容,发现了吧,嘿嘿,完全对应windows的高级选项卡中的所有选项,这样相信大家已经有了大概想法了。
![有线网卡注册表配置对应][09]
再按刚才的方法找无线网卡的主键,找到了0002主键，对应**Intel(R) Dual Band Wireless-AC 3160**无线网卡。
![无线网卡注册表配置对应][10]
发现Ndi/params/下没有**networkaddress**项目（图片上是我后来添加的）,于是,我们想到了手动添加这些信息。如下图：
![添加无线网卡注册表配置1][11]
在左侧 0002/Ndi/params下新建项目名称为**NetworkAddress**。
![添加无线网卡注册表配置2][12]
接着选中**NetworkAddress**,右侧设置完全复制那个有线网卡的内容即可。其中default的值（即默认macd地址）可设可不设。
OK，搞定！现在我们可以按照*伪装有线网卡mac地址*一样的方法来*伪装无线网卡mac地址*了。
![无线网卡属性配置][13]
接下来按照**查看mac地址**方法验证一下，会发现以太网的mac地址确实变成我们刚才设的那个值了。如果没有，重启电脑试一下。**最后再次强调：mac地址的第二位数值必须设置成 “2”，“6”，“A”，“E”中的一个，否则设置无效**



**参考**
1. [怎么修改电脑mac地址，如何修改win10的mac地址][14]
2. [无线网卡的 mac 地址在 Win10中怎么修改？][15]
3. [教你修改无线网卡的MAC地址的方法][16]



[01]: http://opifddwc7.bkt.clouddn.com/18-1-7/6728232.jpg
[02]: http://opifddwc7.bkt.clouddn.com/18-1-7/15538728.jpg
[03]: http://opifddwc7.bkt.clouddn.com/18-1-7/44018010.jpg
[04]: http://opifddwc7.bkt.clouddn.com/18-1-7/82473460.jpg
[05]:http://opifddwc7.bkt.clouddn.com/18-1-7/85200316.jpg
[06]: http://opifddwc7.bkt.clouddn.com/18-1-7/81487741.jpg
[07]: http://opifddwc7.bkt.clouddn.com/18-1-7/24331254.jpg
[08]: http://opifddwc7.bkt.clouddn.com/18-1-7/67669703.jpg
[09]: http://opifddwc7.bkt.clouddn.com/18-1-7/33467140.jpg
[10]: http://opifddwc7.bkt.clouddn.com/18-1-7/35680674.jpg
[11]: http://opifddwc7.bkt.clouddn.com/18-1-7/56032009.jpg
[12]: http://opifddwc7.bkt.clouddn.com/18-1-7/90792947.jpg
[13]: http://opifddwc7.bkt.clouddn.com/18-1-7/84508004.jpg
[14]: https://jingyan.baidu.com/article/fcb5aff7abdc2dedaa4a71f4.html
[15]: https://www.zhihu.com/question/36405648
[16]: http://www.jb51.net/softjc/155004.html







