---
title: 运行babel，webpack，rollop.js用法
categories: babel
tags: [编程,学习]
---

- 运行 `npm init -y`
- `npm i --save-dev babel-core babel-preset-es2015 babel-preset-latest`
- 创建.babelrc
- `npm install --save-dev babel-cli`
本地就不能用 babel 命令了，在 package.json 文件中添加：
```
    {
        "script": {
            "build": "babel src -d lib"
        }
    }
```
- 创建 `./src/index.js`
~~~
把babel-preset-es2015换成babel-preset-env

babel-preset-env包括了之前的：

babel-preset-es2015, babel-preset-es2016, babel-preset-es2017

babel-preset-latest

其他社区的es20xx

babel-preset-node5, babel-preset-es2015-node, 等等

// "build": "babel src -d lib",
~~~


- 开发环境 webpack
`npm i webpack babel-loader --save-dev`
- 配置webpack.config.js
- 配置package.json中的script
- 运行 npm start


运行项目需要安装webpack-dev-server，webpack4x后要区分development和production
不支持loader-core@8，需要卸载
`npm un loader-core`
安装
`npm i --save-dev babel-loader@7 `



#### rollup.js   <----react,vue都是这样打包的
- Rollup 是一个 JavaScript 模块打包器，可以将小块代码编译成大块复杂的代码，例如 library 或应用程序。Rollup 对代码模块使用新的标准化格式

- `npm init`
- `npm i rollup-plugin-node-resolve rollup-plugin-babel babel-plugin-external-helpers babel-preset-latest`
- 配置 .babelrc
- 配置 rollup.config.js

#### rollup与webpack
rollup功能单一，webpack功能强大。
工具要尽量功能单一，可集成，可扩展。
