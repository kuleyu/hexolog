# hexolog
a blog powered by Hexo and Netlify
本博客采用**Netlify**自动部署

# 更新
### update on 2017-12-26
1. 参考![hexo-abbrlink介绍][1]![hexo链接持久化终极解决之道][2]添加 hexo-abbrlink 插件，优化链接问题。
2. 修改模板，给文章添加编辑功能（单击编辑按钮后直接跳转到文章在GitHub的编辑窗口）

# 遇到的坑

> 当重新将整个项目git clone下来，想要在本地操作的时候遇到了一下坑点：
> ```bash
	 D:\Github\Hexo\hexolog (master)
	 λ npm install
	 npm WARN registry Unexpected warning for https://registry.npmjs.org/: Miscellaneous Warning EINTEGRITY: sha512-L3hKV5R/p5o81R7O02IGnwpDmkp6E982XhtbuwSe3O4qOtMMMtodicASA1Cny2U+aCXcNpml+m4dPsvsJ3jatg== integrity checksum failed when using sha512: wanted sha512-L3hKV5R/p5o81R7O02IGnwpDmkp6E982XhtbuwSe3O4qOtMMMtodicASA1Cny2U+aCXcNpml+m4dPsvsJ3jatg== but got sha1-NgSLv/TntH4TZkQxbJlmnqWukfE=. (2872 bytes)
	 npm WARN registry Using stale package data from https://registry.npmjs.org/ due to a request error during revalidation.
	 added 569 packages in 45.318s
	 
	 D:\Github\Hexo\hexolog (master)
	 λ hexo server
	 ERROR Script load failed: themes\next\scripts\tags\exturl.js
	 Error: Cannot find module 'hexo-util'
	     at Function.Module._resolveFilename (module.js:536:15)
	     at Function.Module._load (module.js:466:25)
	     at Module.require (module.js:579:17)
	     at require (D:\Github\Hexo\hexolog\node_modules\hexo\lib\hexo\index.js:216:21)
	     at D:\Github\Hexo\hexolog\themes\next\scripts\tags\exturl.js:8:12
	     at D:\Github\Hexo\hexolog\node_modules\hexo\lib\hexo\index.js:232:12
	     at tryCatcher (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\util.js:16:23)
	     at Promise._settlePromiseFromHandler (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:512:31)
	     at Promise._settlePromise (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:569:18)
	     at Promise._settlePromise0 (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:614:10)
	     at Promise._settlePromises (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:693:18)
	     at Promise._fulfill (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:638:18)
	     at Promise._resolveCallback (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:432:57)
	     at Promise._settlePromiseFromHandler (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:524:17)
	     at Promise._settlePromise (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:569:18)
	     at Promise._settlePromise0 (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:614:10)
	     at Promise._settlePromises (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:693:18)
	     at Promise._fulfill (D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\promise.js:638:18)
	     at D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\bluebird\js\release\nodeback.js:42:21
	     at D:\Github\Hexo\hexolog\node_modules\hexo\node_modules\graceful-fs\graceful-fs.js:78:16
	     at FSReqWrap.readFileAfterClose [as oncomplete] (fs.js:511:3)
	 INFO  Start processing
	 INFO  Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.

	 D:\Github\Hexo\hexolog (master)
	 λ npm install hexo-util --save
	 npm WARN registry Unexpected warning for https://registry.npmjs.org/: Miscellaneous Warning EINTEGRITY: sha512-L3hKV5R/p5o81R7O02IGnwpDmkp6E982XhtbuwSe3O4qOtMMMtodicASA1Cny2U+aCXcNpml+m4dPsvsJ3jatg== integrity checksum failed when using sha512: wanted sha512-L3hKV5R/p5o81R7O02IGnwpDmkp6E982XhtbuwSe3O4qOtMMMtodicASA1Cny2U+aCXcNpml+m4dPsvsJ3jatg== but got sha1-NgSLv/TntH4TZkQxbJlmnqWukfE=. (2872 bytes)
	 npm WARN registry Using stale package data from https://registry.npmjs.org/ due to a request error during revalidation.
	 npm ERR! Cannot read property '0' of undefined

	 npm ERR! A complete log of this run can be found in:
	 npm ERR!     C:\Users\q1402\AppData\Roaming\npm-cache\_logs\2017-12-26T07_37_22_166Z-debug.log
```


> 最终使用 cnpm install 成功解决了。 




[1]: https://segmentfault.com/a/1190000005799711
[2]: http://blog.csdn.net/yanzi1225627/article/details/77761488
