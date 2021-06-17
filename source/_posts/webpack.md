---
title: webpack配置
categories: webpack
tags: [编程,感悟]
---


#### webpack属性配置
```javascript
 const path = require('path')
 module.exports = {
     entry:{
         //main默认入口，可以是多入口
         main:'./src/main.js'
     },
     //出口
     output:{
         filemane:'./build.js',
         //指定js文件
         path:path.join(__dirname,'..','dist',)
         //最好是绝对路径，代表当前目录的上一级的dist
     },
     module:{
            // 一样的功能rules:   webpack2.x之后新加的
            loaders:[       require('./a.css||./a.js')
                {test:/\.css$/,
                 loader:'style-loader!css-loader',
               //  顺序是反过来的2!1
                },
                {
                 test:/\.(jpg|svg)$/,
                 loader:'url-loader?limit=4096&name=[name].[ext]',
                // 顺序是反过来的2!1 
               //  [name].[ext]内置提供的，因为本身是先读这个文件
                 options:{
                    limit:4096,
                    name:'[name].[ext]'
                 }
                }
            ]
     },
     plugins:[
         //  插件的执行顺序是依次执行的
            new htmlWebpackPlugin({
                template:'./src/index.html',
                })
                //将src下的template属性描述的文件根据当前配置的output.path，将文件移动到该目录
     ]
 }
```
#### webpack-es6
* vue默认支持es6的模块导入导出
* babel-->babel-core

#### es6模块

```javascript
//default
import [,...xxx] [,..form] './xxx.ext'
export default obj;

//声明式
export var obj = xxx
export var obj2 = {}
export {stu}//单独导出
import {obj,obj2,stu} form './xxx.js'    //直接使用obj

```
* 默认导出和声明式导入在使用上的区别
    - 声明式导入的时候，必须{名称} 名称要一致（按需导入)
    - 默认导入，可以随意的使用变量名

 ```javascript
{
default:"我是默认导出的结果"    
        import xxx from './cal.js'会获取到整个对象的default属性
obj1:"我是声明式导出1"
obj2:"我是声明式导出2" 
obj3:"我是声明式导出3"     import {obj1,obj2}
obj4:"我是声明式导出4"
}
    import * as allObj from './cal.js';  获取的就是一整个对象
```
* import 和export一定写在顶级，不要包含在{}内



- build：打包配置所在的文件夹
- 打包的配置
- 开发项目的源码
- App.vue入口组件(.vue都是一个组件)
- main.js项目入口的文件
- static：静态资源
- webpack.base.conf.js 打包核心的配置与config->index.js可以合并成为一个

- build.js打生产包


- package.json
1. 项目描述
2. dependencies：依赖库
3. devDependencied：开发依赖库
4. engines： 引擎
5. browserslist：浏览器列表


