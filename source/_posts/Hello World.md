---
title: Hello World
date: '2016/08/17 22:16:37'
tag:
  - Hexo
abbrlink: 16107
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```
<!-- more -->
### Front-matter

Front-matter 是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，举例来说：

``` yaml
title: Hello World
date: 2013/7/13 20:46:25
---
```

以下是预先定义的参数，您可在模板中使用这些参数值并加以利用。

参数 | 描述 | 默认值
--- | --- | ---
`layout` | 布局 | 
`title` | 标题 |
`date` | 建立日期 | 文件建立日期
`updated` | 更新日期 | 文件更新日期
`comments` | 开启文章的评论功能 | true
`tags` | 标签（不适用于分页） |
`categories` | 分类（不适用于分页）|
`permalink` | 覆盖文章网址 |

#### 分类和标签

只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 `Foo, Bar` 不等于 `Bar, Foo`；而标签没有顺序和层次。

``` yaml
categories:
- Diary
tags:
- PS3
- Games
```


More info: [Deployment](https://hexo.io/docs/deployment.html)
