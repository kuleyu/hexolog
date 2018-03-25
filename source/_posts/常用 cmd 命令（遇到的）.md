---
title: "常用 cmd 命令（遇到的）"
date: 2017-07-27 22:46:19
tags: ["cmd","sublime text"]
abbrlink: 20990
categories: ["学习笔记"]
---

## 常用 cmd 命令

```bash
1. 进入 cmd 命令窗口：快捷键 Win+R,输入 “cmd” ，点击“运行”即可。
2. cd..                   				   # 返回上一级文件夹
3. cd [文件夹名]          				   # 进入下一级文件夹
4. d:                     				   # 进入另一个磁盘（如：D盘）根目录
5. cd [文件夹路径]        				   # 进入任一文件夹
6. dir                   				   # 查看当前路径文件夹内容
7. mkdir [文件夹名]       				   # 创建文件夹
8. start D:\Github\Hugo\bin\hugo.exe       # 如何在cmd中打开D:\Github\Hugo\bin\hugo.exe
9. cls 或者 clear         				   # cmd清屏
```

## 在 sublime 编辑器中运行 cmd

这里需要用到[SublimeREPL插件](https://packagecontrol.io/packages/SublimeREPL).

1. 给sublime text2/3安装SublimeREPL插件；
2. 重启sublime text2/3；
3. 按组合键 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>， 然后输入sublimerepl:shell，选中它单击即可调出cmd命令。


