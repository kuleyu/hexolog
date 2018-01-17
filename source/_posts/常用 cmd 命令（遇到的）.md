---
title: 常用 cmd 命令（遇到的）
date: '2017/07/27 22:46:19'
tags:
  - cmd
  - sublime text
abbrlink: 20990
---

## 常用 cmd 命令
1. 进入 cmd 命令窗口：快捷键 <kbd>Win</kbd>+<kbd>R</kbd> ,输入 “cmd” ，点击“运行”即可。
2. 返回上一级文件夹：输入 “cd..” ，再按回车（enter）即可。
3. 进入下一级文件夹：输入 “cd 文件夹名”，再按回车（enter）即可。
4. 进入另一个磁盘（如：D盘）根目录：输入“d:”,再按回车（enter）即可。
5. 进入任一文件夹：输入“cd 文件夹路径”，再按回车（enter）即可。
6. 查看文件夹内容：输入“dir”,再按回车（enter）即可。
7. 创建文件夹：输入“mkdir 文件夹名”，再按回车（enter）即可。
8. *如何在cmd中打开D:\Github\Hugo\bin\hugo.exe*：输入“d:”进入d盘，再输入“start D:\Github\Hugo\bin\hugo.exe”，按回车即可。
9. cmd清屏：输入“cls/clear”,再按回车（enter）即可。

## 在 sublime 编辑器中运行 cmd

这里需要用到[SublimeREPL插件](https://packagecontrol.io/packages/SublimeREPL).

1. 给sublime text2/3安装SublimeREPL插件；
2. 重启sublime text2/3；
3. 按组合键 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>， 然后输入sublimerepl:shell，选中它单击即可调出cmd命令。


