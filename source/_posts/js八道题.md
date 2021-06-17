---
title: js八道题
date: 2018-04-01
categories: js
tags: [编程,感悟]
---

##### 1，写出判断一个变量变量名为arr是数组的方法。
```bash
arr instanceof Array
if(typeof arr.isArray == undefined) {
    return Array.prototype.toString.call = ['object Array']
}
//用一些数组方法去检测，比如slice()
```

##### 2,一下代码输入的结果是？
```bash
var object = {
    foo: 'bar',
    func:function() {
        var self = this;
        console.log(this.foo);  //bar
        console.log(self.foo);  //bar
        (function() {
            console.log(this.foo) //undefined
            console.log(self.foo) //bar
        })()
    }
}
object.func()

//前两个的输出是因为在同一个作用域，this指向当前函数对象，第二次输出，因为是立即执行函数，所以this指向全局window，但是self指向的是当前作用域中的this
//self这个变量会在整个func作用域中生效，同样，立即执行函数IIFE也在func作用域中，因此可以访问self，但IIFE由于缺乏对象this指向了window，但self提前保留了func的作用域this
```

##### 3，请实现以下findList方法
```bash
var docs = [
    { id: 1, words: ['hello', 'world'] },
    { id: 2, words: ['hello', 'China'] },
    { id: 3, words: ['zzz', 'hello'] },
    { id: 4, words: ['world', 'China'] }
];
findList(docs, ['hello']) //1,2,3
findList(docs, ['hello', 'world']) //1

function findList(docs, arr) {
    for (var i = 0; i < docs.length; i++) {
        var bin = false
        for (var j = 0; j < arr.length; j++) {
            var str = arr[j]
            if (docs[i].words.indexOf(str) == -1) {
                bin = true
            }
        }
        if(!bin) {
            console.log(docs[i].id)
        }
    }
}
```

##### 4，移动端如何适配不同手机屏幕，有什么解决方案
- 目前知道的有百分比，rem，vh，flexible。。。

##### 5，编写javascript深度克隆deepClone
```bash
//浅拷贝
var json1 = {
    name: 'a',
    age:12,
    data:{
        a:1,
        b:2
    }
}
function copy(parent,child) {
    var child = child || {}
    for(var i in parent) {
        child[i] = parent[i]
    }
    return child
}
var json2 = copy(json1)
json2.data.a = 3
console.log(json1.data.a)//3
console.log(json2.data.a)//3

//深拷贝
var json1 = {
    name:'aa',
    age:12,
    data:{
        a:1,
        b:2
    }
}

function deepCopy(parent,child) {
    var child = child || {}
    for(var i in parent) {
        if(typeof parent[i] == 'object') {
            child[i] = (parent[i].constructor === Array) ? [] : {}  //child.data = {}
            deepCopy(parent[i],child[i]);//{a:1,b:2} 传过去的是child.data的空json
        }else{
            child[i] = parent[i]; //child.data.a ..
        }
    }
    return child
}

var json2 = deepCopy(json1)
json2.data.a = 3;
console.log(json1.data.a) //1
console.log(json2.data.a) //3
```

##### 6，有一个数组a = [8, 10, 30, 55, 78, 90, 1]，新建一个数组b，b从a中一次随机选取一个元素，取完为止。
```bash
var a = [8,10,30,55,78,90,1]
var b = []
for(var i = 0; i<a.length;i++) {
    var randomNum = Math.floor(Math.random() * a.length) //7个随机数
    var newStr = a.splice(randomNum,1).toString() //随机删除一个，并且把它转换为字符串
    console.log(newStr)
    i--;
    b.push(newStr)
}
console.log(b)
```
##### 7,假设发现有一篇文章，var content = "大量文字..."，过滤其中的敏感词汇，如何发现敏感词汇并将其背景标记为红色.
```bash 
//正则
function filter(content) {
    var result = ''
    var errWorld = ['坏','蠢']
    for( var i  = 0; i<content.length;i++) {
        var reg = new RegExp(errWorld[i],'ig')
        result = content.replace(reg,'')
    }
    return result
}
```
##### 8，编写sum()函数求和，非number类型参数需要进行过滤
```bash
function sum() {
    var result = 0;
    for(var i = 0; i<arguments.length;i++) {
        if(isNaN(arguments[i])) {
            continue
        }else if(typeof arguments[i] === 'number'){
result +=arguments[i]
        }
    }
    return result
}
sum(1,'aaaa',2,'ccc','2','33333',3)
```

