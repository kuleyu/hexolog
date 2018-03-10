---
title: Netlify 自动编译部署生成 Web 网站
date: '2017/10/24 17:21:09'
tags: Netlify
draft: false
abbrlink: 31808
categories: 搬运整理
---

[转载自-freehao123.com](https://www.freehao123.com/netlify/)

![Netlify优秀的静态博客托管平台-自动编译部署生成Web网站可绑域名支持SSL][3]

最近因为Coding对免费用户的pages服务强加跳转提示广告，让不少人不得不将自己的博客迁回Github。但是[Github][4]也有一些无法克服的缺陷：一是访问速度不是很稳定；二是想上SSL只能通过CloudFlare实现。并且CloudFlare访问速度和稳定性都不太好。 

今天推荐的netlify则是国外一家提供静态网络托管服务的综合平台，专注于静态网站托管的web服务平台，可以完美的取代Coding。 Netlify完美且免费支持的ssl、域名绑定、http/2和TLS。最重要的就是，管理方式用git方法传递给github、gitlab或者是Bitbucket，然后Netlify就能自动编译并生成静态网站。 

对于想要使用Jekyll、[Hexo][5]、Hugo等静态搭建网站，又害怕复杂的本地环境配置的朋友，Netlify支持自动编译Jekyll、[Hexo][5]、Hugo等常见的静态博客程序真得是太方便了。另外，Netlify也是一个非常好的静态空间，如果你有用[纸小墨 inkPaper][6]或者Html网页，直接就可以上传发布到空间上了。

<!-- more -->

如果你喜欢静态网站和博客，你可以试试以下方法： 

**Netlify优秀的静态博客托管平台-自动编译部署生成Web网站可绑域名支持SSL**

**一、 Netlify使用前准备：熟悉Github和Git**

1、Github官网： 

* 1、官方网站：[https://github.com/](https://github.com/)

2、Netlify代码来自于Github、Gitlab或者BitBucket，所以你需要请先在Github、Gitlab或者BitBucket上建立一个代码库，并将自己的网页内容git上传到这个代码库中。Github建库，请看这篇文章：[Github空间在线写文章][7]。 

3、用过Github空间的朋友，都知道Github上的Repos都是公共的，除非你愿意付费，否则你放在Github上的代码都能被所有人下载到。而Bitbucket的免费版本的用户可以有无限的私有Repos。如果想要使用Bitbucket，参考：[Bitbucket免费代码托管空间][8]。 

**4、Git使用。**Git是目前世界上最先进的分布式版本控制系统，有愿意使用静态的博客的朋友建议系统学习一下Git。如果你只时想要建立一个静态博客，也可以使用Github Desktop，它可以让你像管理FTP那样上传更新代码，新手朋友再也不用害怕命令了。 

**二、申请Netlify免费空间**

1、Netlify官网： 

* 1、官方首页：[https://www.netlify.com/][9]

2、 首先是到netlify申请注册一个账号。这里可以使用Github、Gitlab以及Bitbucket直接授权登陆。然后登录到空间管理中心，点击右上角的"New site from Git"添加网站。 

![Netlify添加新的网站][10]

3、然后根据自己的托管平台，可以选择GitHub、GitLab或者BitBucket。我们以最常用的GitHub为演示例子。（选择GitHub的同学，别忘了勾选下方的"Limit GitHub access to public repositories."选项） 

![Netlify登录到Github][11]

4、点击GitHub之后会弹出一个让你授权的窗口，然后点击"Authorize netlify"之后，就会在netlify中读取你所有的代码库。 （点击放大） 

![Netlify授权登录账号][12]

5、 点击你已经建好的库，选好分支（默认master即可），然后点击"Deploy site"，系统就会自动编译你的静态页面了。同时还会给出你的页面二级域名等信息。（点击放大） 

![Netlify确认编译][13]

6、点击创建后，稍等一会儿，你就可以看到Netlify免费静态空间已经创建成功了，同时你在GitHub的代码也成功在Netlify运行了。 （点击放大） 

![Netlify创建成功][14]

**三、快速在Netlify建立Jekyll、Hexo、Hugo静态博客**

1、开源静态博客程序网站： 

* 1、网站首页：https://www.staticgen.com/

2、StaticGen是Netlify旗下另一个开源的静态博客程序网站，这里汇集了大部分开源的静态博客程序，而Jekyll、Hexo、Hugo等几款常见的博客程序则可以一键部署到Netlify空间上。 

![Netlify一键部署][15]

3、这里以Hexo为例，点击后跳转到Netlify页面，登录你的Github账号。 

![Netlify跳转到登录][16]

4、命令一个新的项目名称。 

![Netlify命名一个项目][17]

5、稍等一会儿，Netlify就会自动编译好Hexo博客，完成后如下图所示，你可以直接访问它给的二级域名了：（点击放大） 

![Netlify完成博客创建][18]

**四、Netlify文件管理-用GitHub Desktop图形化管理Github**

1、Netlify文件管理直接通过Git提交修改到Github，然后Netlify就会自动执行编译和部署了。不想使用Git的朋友，可以使用GitHub Desktop软件来管理Github空间代码。 

3、GitHub Desktop安装后，使用你的Github账号登录，然后你就可以开始下载、修改和上传代码文件了。这个就是GitHub Desktop软件界面了。 

![GitHub Desktop管理界面][19]

4、以修改Hexo主题为例，首先找到一个漂亮的Hexo主题，从Github打包下载下来。 

![GitHub Desktop下载主题][20]

5、然后将你在Github与Netlify连接创建好的项目通过GitHub Desktop下载到本地，进入到theme文件夹中。 

![GitHub Desktop本地文件][21]

6、将下载下来的Hexo主题文件解压放在theme文件夹中。 

![GitHub Desktop进入主题文件夹][22]

7、然后，修改配置文件_config.yml，将里面theme修改为你的主题，保存。 

![GitHub Desktop修改配置文件][23]

8、现在打开GitHub Desktop软件，你会在Changes中看到文件变化，提示你提交变化。（点击放大） 

![GitHub Desktop提示变化][24]

9、输入描述，点击提交。 

![GitHub Desktop确定提交][25]

10、点击GitHub Desktop右上角的同步按钮，将文件上传到GitHub中，稍等一会儿，你就可以在Netlify那边也看到代码执行后的结果了。 

![GitHub Desktop执行成功][26]

**五、Netlify绑定域名方法**

1、点击Netlify页面左上角netlify的logo，你便可以进入静态空间的设置中心。然后请先记录下系统默认给你分配的二级域名，稍后会用到。在接下来点击"set up domain"，输入你要绑定的域名，点击"save"保存即可。 

![Netlify绑定域名][27]

2、输入你想要绑定的域名。 

![Netlify输入自己的域名][28]

3、确认后，你就可以看到域名已经成功绑定上了。你可以随时修改域名绑定。 

![Netlify绑定成功][29]

4、到你的域名DNS解析处，修改域名的CNAME记录，记录值就是刚刚你记下的Netlify二级域名。 

![Netlify作域名DNS记录][30]

5、域名绑定完成。 

![Netlify解析完成][31]

**六、Netlify添加免费SSL证书**

1、设置完成域名绑定之后，设置中心的选项会有少许变化。会增加一个Enable HTTPS的选项。 

![Netlify开启Https][32]

2、点进去，然后选择"Let's Encrypt Certificate"按钮，系统会自动签发[Let's Encrypt][33]证书给你的站点。 

![Netlify签发域名][34]

3、不过前提条件是你的域名已经解析生效了。如果解析还没有生效，那么体会提示"签发失败"的错误。 

![Netlify生成证书成功][35]

4、如果你想一直使用Https访问的话，可以勾选强制所有的访问转为Https。 

![Netlify强制跳转][36]

5、Netlify添加免费SSL证书成功。 

![Netlify添加证书成功][37]

6、Netlify空间演示： 

* 1、博客演示：https://blog.ps/ 
* 2、主页演示：https://preacher-chipmunk-31460.netlify.com/freehao123/ 
* 3、绑定域名：https://netlify.freehao123.info/ 

**七、Netlify空间使用技巧**

**1、Netlify可以直接添加html代码。**在Netlify面板可以直接一键添加html代码到你的网站之前，这样你就可以很方便的把网站统计代码加到网站上了。 

![Netlify添加代码][38]

**2、Netlify可以设置变量及命令。**Netlify的免费用户可以为自己的网站设置环境变量、hooks等等。而付费用户则可以为网站设置更加详细的SEO优化。（点击放大） 

![Netlify设置变量][39]

**3、Netlify的访问速度不错。**在首页的地图标注上，是有国内节点的（当然也有可能是宣传画而已）。实测大陆访问全部解析到Amazon日本东京节点上，速度表现尚可。 

![Netlify速度快][40]


[2]: http://www.cloud.cc
[3]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_00.gif
[4]: https://github.com/
[5]: https://hexo.io/
[6]: http://www.chole.io/
[7]: https://www.freehao123.com/github-farbox-dropbox/#toc-1
[8]: https://bitbucket.org/
[9]: https://www.netlify.com/
[10]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_01.gif
[11]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_02-530x300.gif
[12]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_03-530x300.gif
[13]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_05-530x300.gif
[14]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_05_1-530x300.gif
[15]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_06-530x300.gif
[16]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_07.gif
[17]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_08.gif
[18]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_09-530x300.gif
[19]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_10-530x300.gif
[20]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_12.gif
[21]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_11.gif
[22]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_13.gif
[23]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_16.gif
[24]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_14-530x300.gif
[25]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_15.gif
[26]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_17.gif
[27]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_19.gif
[28]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_20.gif
[29]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_21.gif
[30]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_22.gif
[31]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_23.gif
[32]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_24.gif
[33]: https://www.freehao123.com/tag/lets-encrypt/
[34]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_25.gif
[35]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_26.gif
[36]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_27.gif
[37]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_28.gif
[38]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_30.gif
[39]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_31-530x300.gif
[40]: https://www.freehao123.com/wp-content/uploads/2017/05/netlify_29.gif