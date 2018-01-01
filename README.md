# hexolog


**本文件夹内容作为 gh-pages 分支的源代码**

其实相对于 master 分支，就只改了 <\_config.yml>文件：
```
	将		url: http://yoursite.com/
	  		root: /
	改成了	url: http://yoursite.com/hexolog/
			root: /hexolog/
```

理论上这个分支没有必要，但还有些知识不了解，所以目前也只有采用这种笨方法了。

> 不了解有如下：
> 1. 在执行 ```hexo generate``` 时，添加哪些参数，可以不用改变<\_config.yml>文件而使生成的 public 文件用的是 ```url: http://yoursite.com/hexolog/ 与 root: /hexolog/``` 部署的。
> 2. 如何设置 netlify，使其部署的网站直接以 https://xxx.netlify.com/hexolog/ 打开。


# 更新

1. 修正 NexT Theme ```themes\next\layout\_macro``` 路径下 ```wechat-subscriber.swig``` 与 ```reward.swig``` 两个模板的一类 bug （当主配置文件的根目录设置为 ```root:/XXXXXX/``` 时，两个模板解析后的资源的相当路径问题）。未修正前，当主配置文件的根目录设置为 ```root:/XXXXXX/``` 时，部署完成后，文章尾部的关注图片与打赏图片加载不出来；修正后可以正常显示。