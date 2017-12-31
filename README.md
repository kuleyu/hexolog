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
