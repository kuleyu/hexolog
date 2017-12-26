---
title: "Windows下安装Grunt的指南和相关说明 "
date: 2017-07-21T22:58:24+08:00
draft: false
---



[转载 - 蓝色梦想](http://www.bluesdream.com/blog/windows-installs-the-grunt-and-instructions.html "Permalink to Windows下安装Grunt的指南和相关说明 | 蓝色梦想")

Grunt基于Node.js，其中 npm 是 Node.js 的包管理器，而Grunt和Grunt插件就通过 npm 安装并管理。

Grunt 0.4.x 必须配合Node.js >= 0.8.0版本使用。

**安装Node.js：**  
去[Node.js官网][1]，点击INSTALL下载并安装，现在的Node.js会自动安装npm。

安装完成之后，打开命令行，进行后续的操作（开始->输入CMD 或 开始->所有程序 ->命令提示符）。

进入Node.js的安装目录（默认路径为"C:Program Filesnodejs"）：  
cd pro*nod*  
  
可以通过以下2个命令，查看 node.js 和 npm 的版本号：  
node -v
npm -v

<!-- more -->

**安装Grunt：**  
如果你之前安装过老的0.3版本，请先卸载：  
npm uninstall -g grunt

安装Grunt命令行（CLI）：  
npm install -g grunt-cli

注1：-g代表全局安装，Grunt有二个版本：服务器端版本（grunt）和客户端版本（grunt-cli）。

注2：安装grunt-cli并不等于安装了grunt！grunt CLI的任务很简单：调用与Gruntfile在同一目录中的grunt。这样带来的好处是，允许你在同一个系统上同时安装多个版本的grunt。而grunt使用模块结构，除了安装命令行界面以外，还要根据需要安装相应的模块。这些模块应该采用局部安装，因为不同项目可能需要同一个模块的不同版本。

上述命令执行完后，grunt 命令就被加入到你的系统路径中了，以后就可以在任何目录下执行此命令了。

**创建新的Grunt项目：**  
假设这个项目安装在D盘根目录，我们首先进度D盘：  
d:

创建项目文件夹：  
mkdir testProject

进入文件夹：  
cd testProject

接着在你的项目文件夹根目录下添加两个文件：package.json 和 Gruntfile。

package.json: 此文件被npm用于存储项目的元数据，以便将此项目发布为npm模块。  
Gruntfile: 此文件被命名为 Gruntfile.js 或 Gruntfile.coffee，用来配置或定义任务（task）并加载Grunt插件。

**创建package.json文件：**  
package.json应当放置于项目的根目录中，与Gruntfile在同一目录中，并且应该与项目的源代码一起被提交。大部分 grunt-init 模版都会自动创建特定于项目的package.json文件。

方法一：执行 npm init (选方法一。方法二手动添加后在安装时出现错误)命令（根据默认的grunt-init模板，引导你创建一个"基本"的package.json文件）：  
npm init

根据提示填写信息（都允为空）：
    
    
    
    name: (GruntT)　　　　　　// 模块名称：只能包含小写字母数字和中划线，如果为空则使用项目文件夹名称代替
    version: (0.0.0)　　　　　// 版本号
    description:　　　　　　　// 描述：会在npm搜索列表中显示
    entry point: (index.js)　// 模块入口文件
    test command:　　　　　　　// 测试脚本
    git repository:　　　　　　// git仓库地址
    keywords:　　　　　　　　　// 关键字：用于npm搜索，多个关键字用空格分开
    author:　　　　　　　　　　// 作者
    license: (BSD-2-Clause) 　// 开原协议
    

方法二：手动创建package.json文件，添加项目/模块的描述信息：
    
    
    
    {
    　"name": "my-project",
    　"version": "0.1.0"
    }
    

附：  
[package.json官方文档][2]  
[一个较完整的package.json文件][3]

**安装Grunt和Grunt插件：**  
方法一：手动添加，修改package.json文件：
    
    
    
    {
    　"name": "my-project",
    　"version": "0.1.0",
    　"devDependencies": {
    　　"grunt": "~0.4.1",
    　　"grunt-contrib-cssmin": "~0.7.0"
    　}
    }
    

注：devDependencies里面的参数，指定了项目依赖的Grunt和Grunt插件版本。其中"~0.7.0"代表安装该插件的某个特定版本，如果只需安装最新版本，可以改成"*"。

然后执行：  
npm install

这时你会发现项目文件夹中多了个node_modules文件夹，其里面就是对应的Grunt和Grunt插件。

方法二：自动安装：  
通过 npm install  \--save-dev 命令

安装最新版的Grunt：  
npm install grunt --save-dev

接着安装我们所需要的插件：  
npm install grunt-contrib-cssmin --save-dev

注：其中--save-dev，表示将它作为你的项目依赖添加到package.json文件中devDependencies内。如果你要安装指定版本的Grunt或者Grunt插件，只需要运行npm install grunt@VERSION --save-dev命令，其中VERSION就是你所需要的版本(指定版本号即可)。

附：[Grunt官方插件列表][4]，其中带星号的为官方维护的插件。

**创建Gruntfile.js文件：**
    
    
    
    module.exports = function(grunt) {
    
        // 配置任务参数
        grunt.initConfig({
            pkg: grunt.file.readJSON('package.json'),
            cssmin: {
                combine: {
                    files: {
                      'css/release/compress.css': ['css/*.css'] // 指定合并的CSS文件 ['css/base.css', 'css/global.css']
                    }
                },
               minify: {
                    options: {
                        keepSpecialComments: 0, /* 删除所有注释 */
                        banner: '/* minified css file */'
                    },
                    files: {
                        'css/release/master.min.css': ['css/master.css']
                    }
                }
            }
        });
    
        // 插件加载（加载 "cssmin" 模块）
        grunt.loadNpmTasks('grunt-contrib-cssmin');
    
        // 自定义任务：通过定义 default 任务，可以让Grunt默认执行一个或多个任务。
        grunt.registerTask('default', ['cssmin']);
    
    };
    

执行配置中所有的任务：  
grunt

执行某个特定的任务：  
grunt cssmin

**测试：**  
接着我们在项目文件夹中创建个子文件夹，命名为：CSS

并且在里面创建base.css和master.css，2个CSS文件，你可以随便写点内容在里面。

然后在命令行中执行grunt，看到如下提示说明执行成功：  
Running "cssmin:combine" (cssmin) task  
File css/release/compress.css created.

Running "cssmin:minify" (cssmin) task  
File css/release/master.min.css created.

Done, without errors.

参考文档：  
  


[1]: http://nodejs.org/
[2]: https://npmjs.org/doc/json.html
[3]: https://github.com/spmjs/spm/wiki/package.json
[4]: http://gruntjs.com/plugins

  
 NIGHT MODE
