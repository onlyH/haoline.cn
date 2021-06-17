---
title: gulp了解
categories: gulp
tags: [语言,感悟]
---

* 安装 gulp `$ npm install --global gulp`
* 任务管理文件 `gulpfile.js`
```javascript
//引入
const gulp = require('gulp')
const shelljs = require('shelljs')
//定义任务，默认default
gulp.task('default',()=>{
   // console.log('this is deafult')
   //执行
})
```
* 运行 `$ gulp`

