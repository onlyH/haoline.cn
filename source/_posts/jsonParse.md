---
title: 关于JSON报错
date: 2018-05-01
categories: json
tags: [编程,学习]
---

在项目代码中运用到了JSON.parse()和JSON.stringify()去转换保存对象
`this.editingFile = JSON.stringify(lesson)`
`this.$set(this.lessons, index, JSON.parse(this.editingFile))`


每次新建进入页面的时候，都会报错
`Uncaught SyntaxError: Unexpected token u in JSON at position 0`

Debugger后，发现每次新建的时候JSON.parse()里的参数是undefined

查考得知，当参数为undefined的时候，JSON.parse()会报错的。
解决办法是做判断。类似于：
```
   if (this.editingFile != undefined) {
          this.$set(this.lessons, index, JSON.parse(this.editingFile))
        }
```
以上是我的解决办法。

