---
title: js五道题
date: 2018-01-01
categories: js
tags: [编程,感悟]
---
##### 第一题：
```javascript
if(!("a" in window)) {
    var a = 1
}
alert(1)
```
- 所有的全局变量都是window属性，`var a = 1 == window.a = 1  `
检测变量是否声明` 变量 in window`
- 所有的变量声明都在范围作用域的顶部,应该是变量提升吧
```javascript
 alert('a' in window)
 var a;

//等同于
var a;
alert('a' in window)
```
- 当变量声明和赋值在一起用的时候，js引擎会自动将他们分为两部分，声明和赋值，以便于将变量声明提前，不将赋值提前是因为他有可能影响代码执行处不可预期的结果。
```javascript
//所以等同于
var a;
if(!('a' in window)) {
    a = 1
}
alert(a)
```


##### 第二题：
```javascript
var a = 1,
    b = function a(x) {
        x && a(--x)
    }
alert(a)
```


- 变量声明在进入执行上下文就完成了
- 函数声明也是提前的，所有的函数声明都在执行代码之前都已经完成了声明，和变量声明一样。
```javascript
//函数声明
function name(arr1,arr2) {
    //...
}
//函数表达式，相当于变量赋值，不是函数,函数表达式不会提前，等于普通的变量赋值
var name = function(arr1,arr2) {
    //...
}
```

- 函数声明会覆盖变量声明，但不会覆盖变量赋值，函数声明的优先级高于变量声明的优先级
```javascript   
function value() {
    return 1
}
var value;
alert(typeof value) //function
//如果赋值了，变量赋值初始化就覆盖了函数声明
function value() {
    return 1
}
var value = 2;
alert(typeof value) //number
``` 

-  这个函数是一个有名函数表达式，函数表达式不像函数声明一样可以覆盖变量声明，变量b是包含了该函数表达式，而这个函数表达式的名字是a，浏览器允许在函数内部调用a(--x),因为这个时候a在函数外面依然是数字，返回undefined。

```javascript
var a = 1,
    b = function(x) {
        x && b(--x)
    }
alert(1) //1
```


##### 第三题：
```javascript
function a(x) {
    return x * 2
}
var a;
alert(a) //undefined  函数声明
```


##### 第四题：
```javascript
function b(x,y,a) {
    arguments[2] = 10;
    alert(a)
}
b(1,2,3) //10
```
- 活动对象是在进入函数上下文时被创建的，它通过函数的arguments属性初始化，arguments属性的值是arguments对象。
arguments 对象是活动对象的一个属性，包括：
1. callee指向当前函数的引用
2. length传递的参数的个数
3. properties-indexes（字符串类型的整数）属性的值就是函数的参数值，（左->右的顺序）
4. properties-indexes内部元素的个数等于arguments.length.properties-index的值和实际传递过来的参数之间是共享的。
这个<strong>共享</strong>不是真正的共享一个内存地址，而是两个不同的内存地址，使用js引擎来保证2个值是随时一样的，这个索引值要小于传入的参数的个数，如果只是传入了两个参数，使用arguments[2]赋值的话就会不一样。
```javascript
function b(x,y,a) {
    arguments[2] = 10;
    alert(a)
}
b(1,2) //undefined
//因为没有传递第三个参数a，所以赋值10以后，alert(a)的结果依然是undefined

function b(x,y,a) {
    arguments[2] = 10;
    alert(arguments[2])
}
b(1,2) //10

//与a没有关系
```






##### 第五题：
```javascript
function a() {
    alert(this)
}
a.call(null)
```

###### this的定义
```javascript
var obj = {
    method: function() {
        alert(this === obj)  //true 
    }
}
//当一个方法在对象上调用的时候，this就指向到了该对象上

function method() {
    alert(this === window) //true
}
//当一个function的定义不是属于一个对象属性的时候（单独定义的函数），函数内部的this等价于window

```

###### call
1. call方法最为一个function执行，代表该方法可以让另外一个对象作为调用者来调用。
2. call方法的第一个参数是对象调用者，随后的其他参数是要传递给调用method的参数（声明的话）
```javascript
function method() {
    alert(this === window)
}
method() //true
method.call(document) //false
```
###### 如果第一个参数穿肚的对象调用者是null或者undefined，call方法将把全局对象（window）作为this的值。
```javascript
//理解如下
function a() {
    alert(this)
}
a.call(window) //[object Window]
```