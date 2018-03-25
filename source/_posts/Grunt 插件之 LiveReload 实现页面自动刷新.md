---
title: "Grunt 插件之 LiveReload 实现页面自动刷新"
draft: false
abbrlink: 5109
date: 2017-07-21 14:54:53
categories: ["搬运整理"]
tags: ["grunt"]
---



[转载 - 蓝色梦想](http://www.bluesdream.com/blog/grunt-plugin-livereload-wysiwyg-editor.html "Permalink to Grunt插件之LiveReload 实现页面自动刷新，所见即所得编辑 | 蓝色梦想")

**注意文末的更新**

苦B的前端每次在制作和修改页面时，都有一个特定的三部曲：coding-save-F5。很多时候都希望自己一改东西，页面就能立刻显示，而现在LiveReload就能做到这点。

LiveReload会监控你指定的目录中文件，如果有文件被更改，它就自动触发浏览器刷新页面。但如果你修改的是CSS或者图片，页面内容会即时更新，无需重新加载。这样我们不用每次修改文件后，都要去按下F5刷新页面，而是直接就能显示，有点类似所见即所得的编辑模式，特别适合使用双屏coding的人。另外同时结合Sublime text和Emmet LiveStyle，能提高不少开发效率。

<!-- more -->

**LiveReload安装前的准备工作：**

安装Node.js和Grunt，如果第一次接触，可以参考：[Windows下安装Grunt的指南和相关说明][1]，根据步骤操作，创建完package.json 和 Gruntfile.js这2个文件就行。

接下来，开始配置LiveReload所需要的环境和相关插件。这里所提供的有两种安装方案，根据自己需求进行选择。

**方案一：grunt-livereload + Chrome Plug-in**

优点：安装、配置简单方便。  
缺点：需要配合指定的浏览器插件（Firefox也有相关插件，IE么你懂的）。

1\. 需要安装2个插接件：grunt-contrib-watch、connect-livereload

执行命令：npm install --save-dev grunt-contrib-watch connect-livereload

2\. 安装浏览器插件：[Chrome LiveReload][2]

3\. 配置一个Web服务器（IIS/Apache），LiveReload需要在本地服务器环境下运行（对file:///文件路径支持并不是很好）。

4\. 修改Gruntfile.js文件：    
```javascript
    module.exports = function(grunt) {
        // 项目配置(任务配置)
        grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
            watch: {
                client: {
                    files: ['*.html', 'css/*', 'js/*', 'images/**/*'],
                    options: {
                        livereload: true
                    }
                }
            }
        });
    
        // 加载插件
        grunt.loadNpmTasks('grunt-contrib-watch');
    
        // 自定义任务
        grunt.registerTask('live', ['watch']);
    
    };
```

5\. 执行：grunt live

看到如下提示，说明已经开始监听任务：  
```
    Running "watch" task  
    Waiting...
```

6\. 打开我们的页面，例如：http://localhost/

7\. 再点击Chrome LiveReload插件的ICON，此时ICON圆圈中心的小圆点变成实心的，说明插件执行成功。此时你改下网站文件看看，是不是实时更新了？

**方案二：grunt-contrib-watch + grunt-contrib-connect + grunt-livereload**

优点：自动搭建静态文件服务器，不需在自己电脑上搭建Web服务器。  
不需要浏览器插件的支持（不现定于某个浏览器）。  
不需要给网页手动添加livereload.js。  
缺点：对于刚接触的人，配置略显复杂。

1\. 安装我们所需要的3个插件：grunt-contrib-watch、grunt-contrib-connect、connect-livereload

执行命令：npm install --save-dev grunt-contrib-watch grunt-contrib-connect connect-livereload

2\. 修改Gruntfile.js文件：
```javascript
    module.exports = function(grunt) {
    
        // LiveReload的默认端口号，你也可以改成你想要的端口号
        var lrPort = 35729;
        // 使用connect-livereload模块，生成一个与LiveReload脚本
        // http://www.google-analytics.com/ga.js"> src="http://127.0.0.1:35729/livereload.js?snipver=1" type="text/javascript">
        var lrSnippet = require('connect-livereload')({ port: lrPort });
        // 使用 middleware(中间件)，就必须关闭 LiveReload 的浏览器插件
        var lrMiddleware = function(connect, options) {
            return [
                // 把脚本，注入到静态文件中
                lrSnippet,
                // 静态文件服务器的路径
                connect.static(options.base[0]),
                // 启用目录浏览(相当于IIS中的目录浏览)
                connect.directory(options.base[0])
            ];
        };
    
        // 项目配置(任务配置)
        grunt.initConfig({
            // 读取我们的项目配置并存储到pkg属性中
            pkg: grunt.file.readJSON('package.json'),
            // 通过connect任务，创建一个静态服务器
            connect: {
                options: {
                    // 服务器端口号
                    port: 8000,
                    // 服务器地址(可以使用主机名localhost，也能使用IP)
                    hostname: 'localhost',
                    // 物理路径(默认为. 即根目录) 注：使用'.'或'..'为路径的时，可能会返回403 Forbidden. 此时将该值改为相对路径 如：/grunt/reloard。
                    base: '.'
                },
                livereload: {
                    options: {
                        // 通过LiveReload脚本，让页面重新加载。
                        middleware: lrMiddleware
                    }
                }
            },
            // 通过watch任务，来监听文件是否有更改
            watch: {
                client: {
                    // 我们不需要配置额外的任务，watch任务已经内建LiveReload浏览器刷新的代码片段。
                    options: {
                        livereload: lrPort
                    },
                    // '**' 表示包含所有的子目录
                    // '*' 表示包含所有的文件
                    files: ['*.html', 'css/*', 'js/*', 'images/**/*']
                }
            }
        }); // grunt.initConfig配置完毕
    
        // 加载插件
        grunt.loadNpmTasks('grunt-contrib-connect');
        grunt.loadNpmTasks('grunt-contrib-watch');
    
        // 自定义任务
        grunt.registerTask('live', ['connect', 'watch']);
    };
```

5\. 执行：grunt live

看到如下提示，说明Web服务器搭建完成，并且开始监听任务：  
```
    Running "connect:livereload" (connect) task  
    Started connect web server on 127.0.0.1:8000.
    
    Running "watch" task  
    Waiting...
```
注：执行该命令前，如果你有安装过LiveReload的浏览器插件，必须关闭。

6\. 打开我们的页面，例如：http://localhost:8000/ 或 http://127.0.0.1:8000/  
注：这里所打开的本地服务器地址，是我们刚才通过connect所搭建的静态文件服务器地址，而不是之前你用IIS或Apache自己搭建Web服务器地址。

7\. 开始体验吧。

相关插件文档（GitHub）：  
[grunt-contrib-watch][3]  
[grunt-contrib-connect][4]  
[connect-livereload][5]

参考资料：  
[Grunt by Example - A Tutorial for JavaScript's Task Runner][6]

**更新于：2015-08-17**  
总有小伙伴，通过各种方式给我留言，说自己配置出来运行报错。但通常都无法正确表述问题在哪，这让我很难帮到你什么。另外，我也无法保证所有问题都能即时回复，所以大家可以先通过以下3个步骤，进行自查。  
1\. 核对下各个插件是否都顺利安装。  
2\. 检查一边是否有遗漏的步骤和地方。  
3\. 我把本文的配置信息都上传至了Github，有需要的朋友可以下载下来进行对比：[Github：2015-08-17][7]

**更新于：2015-10-12**  
grunt-contrib-connect 0.11.x 版本开始，静态文件服务器的创建，需要安装 serve-static 插件支持，否则会出现错误提示"connect.static is not a function Use." 另外，启用目录浏览，也需要独立安装 serve-index 插件才能支持。

所以这次补充2个新的插件：serve-static(用于创建静态文件服务器)、serve-index(用于启用目录浏览)

执行命令：npm install --save-dev serve-static serve-index

Gruntfile.js文件更新及调整部分：
```javascript
        // 使用 middleware(中间件)，就必须关闭 LiveReload 的浏览器插件
        var serveStatic = require('serve-static');
        var serveIndex = require('serve-index');
        var lrMiddleware = function(connect, options, middlwares) {
            return [
                lrSnippet,
                // 静态文件服务器的路径 原先写法：connect.static(options.base[0])
                serveStatic(options.base[0]),
                // 启用目录浏览(相当于IIS中的目录浏览) 原先写法：connect.directory(options.base[0])
                serveIndex(options.base[0])
            ];
        };
```

完整版请查看：[Github：2015-10-12][8]

[1]: http://www.bluesdream.com/blog/windows-installs-the-grunt-and-instructions.html
[2]: https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei
[3]: https://github.com/gruntjs/grunt-contrib-watch
[4]: https://github.com/gruntjs/grunt-contrib-connect
[5]: https://github.com/intesso/connect-livereload
[6]: http://www.brianchu.com/blog/2013/07/11/grunt-by-example-a-tutorial-for-javascripts-task-runner/
[7]: https://github.com/zhonglimh/Grunt-LiveReload/tree/2caa3b75e6c58218e4d609736f69e48766fb9c15 "Github"
[8]: https://github.com/zhonglimh/Grunt-LiveReload "Github"