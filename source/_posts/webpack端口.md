---
title: webpack端口号
categories: js
tags: [编程,学习]
---

今日，修改webpack端口号，遇到了问题
本来是在webpack.config.js文件夹里添加了devServer属性
```
   devServer: {
        port: 2333,
        host: '0.0.0.0',
        overlay: {
            errors: true
        },
        hot: true
    },

     plugins: [
        new webpack.HotModuleReplacementPlugin(),]
```   
`webpack-dev-server` 带 `hot` 参数的时候，要去掉config里面的 HotModuleReplacementPlugin
不然会内存溢出。
解决办法：删除
```
plugins: [
        new webpack.HotModuleReplacementPlugin()
]
```
将
`"dev": "webpack-dev-server --mode development",`
改为
`"dev": "webpack-dev-server --hot --inline",`
启动服务并不能自动刷新，要自动刷新需要用到webpack-dev-server --hot --inline
当使用webpack-dev-server --hot --inline命令时，
在每次修改文件，是将文件打包
　　保存在内存中并没有写在磁盘里(默认是根据webpack.config.js打包文件，通过--config xxxx.js修改)，这种打包得到的文件
　　和项目根目录中的index.html位于同一级（你看不到，因为
　　它在内存中并没有在磁盘里）。使用webpack命令将打包后的文件保存在磁盘中
　　例如在index.html文件中引入通过webpack-dev-server --hot --inline打包的build.js
`<script src="build.js"></script>`
　　在index.html文件中引入通过webpack命令打包的build.js
`　<script src="./build/build.js"></script>`


