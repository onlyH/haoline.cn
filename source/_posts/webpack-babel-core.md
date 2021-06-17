---
title: webpack.config.js配置错误问题
categories: webpack
tags: [编程,感悟]
copyright: true
---
今日搭建webpack的时候，一直在报错，其中一个问题是`Cannot find module '@babel/core'问题`
最初以为是babel-core没有安装上。重装了好几遍babel-core还是不行。对照以前的项目,发现babel-loader的版本不一样,之前的是@7.1.5版本,而现在是@8.0.0版本。
- 解决办法：降版本。。。
`npm uninstall babel-loader npm install babel-loader@7.1.5`

#### 官方文档说：
##### 官方默认babel-loader | babel 对应的版本需要一致: 即babel-loader需要搭配最新版本babel