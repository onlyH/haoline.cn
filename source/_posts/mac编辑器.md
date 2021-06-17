---
title: Mac 终端中用指定文本编辑器打开选中文件
date: 2017-05-01
categories: tools
tags: [编程,学习]
---

之前每次用`terminal`打开文件都是`open .` 打开父文件夹然后用编辑器打开要打开的文件，或者`cat`查看当前文件内容。很不舒服，于是，就开始研究命令，果不其然，真的有办法
```
open -t index.html
open -e inex.html
open -a WebStorm index.html
```
- `-t`使用默认编辑器打开
- `-e`文本编辑器打开
- `-a`指定应用