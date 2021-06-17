---
title: ES6 理解api
date: 2017-05-01
categories: js
tags: [编程,学习]
---


```javascript
function sum(x,y,z) {
    let total = 0;
    if(x) total += x;
    if(y) total += y;
    if(z) total += z;
    console.log(`total:${total}`)
}
sum(1,'',2)

function sum2(...m) {    //...rest参数，动态的，不确定的
    let total = 0;
    for(var i of m) {
        total += i
    }
    console.log(`total:${total}`)
}
sum2(1,2,3) //不能传入字符串，否则会解析为字符串


let sum3 =(...m) =>{
    let total = 0;
    for(var i of m) {
        total += i;
    }
    console.log(`total:${total}`)
}
sum3(1,2,3)



var [x,y] = [4,8] //解构
console.log(...[4,8]) //

let arr1 = [1,2]
let arr2 = [3,4]
console.log(...arr1,...arr2)
console.log([...arr1,...arr2]) //concat

var [x,...y] = [4,8,12,13] // y : 8,12,13


let [a,b,c] = 'es6' //a:e,b:s,c:6
let xy = [...'es6'] //拆解
```

##### ...放函数里是rest参数，放数组里会进行运算，拆解
```javascript
let checkLogin = function() {
    return new Promise(function(resolve,reject) {
        let flag = document.cookie.indexOf('userId') > -1 ? true : false;

        if(flag) {
            resolve({
                status:0,
                result:true
            })
        }else{
            reject('error')  //报错
        }
    })
};
let getUserInfo = () => {
    return new Promise((resolve,reject)=>{
        let userInfo = {'101'
        }
        resolve(userInfo);
    })
}
checkLogin().then(res=>{
    if(res.status == 0) {
        console.log('login');
        return getUserInfo()
    }
}).catch(error=>{
    console.log('error')
}).then((res2) =>{
    console.log(`userId:${userId}`)
})

Promise.all([checkLogin(),getUserInfo()]).then(([res1,res2])=>{
    console.log(`result:`${res1.result})
})
```

#### import,export
```javascript
//utils.js
export let sum = (x,y)=>{
    return x + y;
}
export let sale = (m,n) =>{
    return m - n;
}

//router.js
import {sum,sale} from './utils'
console.log(`sum:${sum(1,2)}`)


import *as util from './utils'
console.log(`sum:`${util.sum(1,2)})

// import可以异步加载 
<span @click="add"></span>
add(){
    import('./utils')
}
```
##### AMD,CMD,CommonJs,ES6
- 模块化，规范，标准
- AMD是requireJs在推广中对模块定义的规范化产出-同步模块定义 -- 依赖前置
```javascript
define(['package/lib'],function(lib) {
    function foo() {
        lib.log('hello world')
    }
    return {
        foo:foo
    }
})
```
- CMD是SeaJs在推广过程中对模块定义的规范化产出-同步模块定义--依赖就近
```javascript
//所有模块通过define来定义
define(function(require,exports,module) {
    //通过require引入依赖
    var $ = require('jquery')
    var Spinning = require('./spinning')
})
```
- commonJs---module.exports
* 在浏览器不支持 ==>在node中使用
```javascript
exports.area = function() {
    return Math.PI * r * r;
}
exports.circumference = function() {
    return 2 * Math.PI * r
}
```



- 解构赋值
```javascript

{
    let [a,b]  = [1,2]
}

{
    let [a,b,c = 3] = [1,2,4]
    console.log(a,b,c)
}

{
    function sum() {
        return [1,2]
    }
    let [a,b] = sum()
}

{
    let {a,b} = {a:1,b:2}
}

{
    var list = {
        title:'hello',
        core:[{
            title:'tom',
            age:'34'
        },{
            title:'lele',
            age:'22'
        }]
    }

    var {title:newTitle,core:[{title:contentTitle,age:contentAge}]} = list
    console.log(newTitle,contentTitle,contentAge) //hello tom 34

}

{
      var list = [{
            title:'tom',
            age:'34'
        },{
            title:'lele',
            age:'22'
        }]
    

    for(var {title:cTitle,age:cAge} of list) {
            console.log(cTitle,cAge)//tom 34 lele 22
    }
}


{
    let [a,,b] = [1,2,3]  //1,3
}


{
    let [a,...b] = [1,2,3,4]//1,[2,3,4]
}


{
    let [a,b] = [2,3]
    console.log(b);//3
    [b,a] = [a,b];
    console.log(b)//2
   
}

{
    let o = {
        p:42,
        q:true
    }
    let {p,q} = o
    console.log(p,q)
}

```

- 字符串扩展
```javascript
{
    let str = 'string'
    console.log(str.includes('g'),str.startsWith('st'),str.endsWith('ng'))//true true true
}


{
    let str = 'string'
    console.log(str.repeat(2)) //stringstring
}

{
    // 补白
    console.log('1'.padStart(2,'0'))
    console.log('1'.padEnd(2,'0'))
}

{
    // 标签模板 适用于多语言和xss攻击
     let user = {
         name:'Tom',
         age:'23'
     }
     function person(s,v1,v2) {
         return s + v1 + v2
     }
     person`i am ${user.name},my age is ${user.age}`

}
```
- 数值扩展
```javascript
{
    Number.isFinite(12)
    Number.isNaN(NaN)
    Number.isInteger(2)
    Number.isSafeInteger(2) //是否在有效范围之内
    Math.trunc(3.2)//返回小数的整数部分,3
    Math.sign(-5)//-1,0,1 ==>判断是正数负数还是0
    Math.cbrt(-2)
}
```
- 数组扩展
`Array.form,Array.of,copyWithin,find/findIndex,fill,entries/keys/values,inludes`
```javascript
let arr = Array.of(1,2,3,4,5) //把一组数据变量转化为数组类型
// 把一些集合转化为数组
Array.from([1,2,3,4,5],item=>item*2)   //2,4,6,8,10

[1,2,3].fill(7)  //[7,7,7]
[1,'a',undefined,3].fill(9) //(4) [9, 9, 9, 9]
[1,2,3,4,5,6,7].fill(8,1,3) 
[1,2,3,4,5,6,7].fill(8,1,3) //(7) [1, 8, 8, 4, 5, 6, 7]

for(let index of [1,2,3,4,5].keys()) {
    console.log(index)  //0,1,2,3,4
}

for(var val of ['a',1,'b'].values()) {
    console.log(val) //a,1,b
}


for(let [index,val] of [1,'a','cc',9].entries()) {
    console.log(index,val)
}
//  0 1
//  1 "a"
//  2 "cc"
//  3 9
[1,2,3,4,5].copyWithin(0,2,4) //(5) [3, 4, 3, 4, 5]


//查找一个元素是否在一个数组中
[1,2,3,4,5].find(item => item > 3 ) //4 只找一个
[1,2,3,4,5].findIndex(item =>item > 3) //返回当前符合元素的下标
[1,2,3].includes(2)

[NaN].includes(NaN) //true

```
- 函数扩展
```javascript 
 function test(x,y = 'world') {
     console.log(x,y)
 }
 test() //undefined,world


let x = 'one'
function test(x,y = x) {
    console.log(x,y)
}
test('two') //two,two


let x = 'one'
function test(c,y = x) {
    console.log(x,y)
}
test('two') //two,one


function test(...args) {
    for(let val of args) {
        console.log(val)
    }
}
test(1,2,3,4,5)   //1,2,3,4,5
// 把数组转化为离散的值
console.log(...[1,2,3])
//合并
console.log(...[1,3,4],...[9,8])

let arrow = item =>item * 2
arrow(3) //6

//尾调用
function tail(x) {
    console.log(x)
}
function tx(x) {
    return tail(x)
}
tx(4) //4
```
- 对象扩展
```javascript
//简介表达式
let name = 'lele'
let age = 12

let dog = {
    name,
    age,
    }
//属性表达式
let a = 'b'
let es5 = {
    a:'c',
    b:'c'
}
let es6 = {
    [a]:'c'
}
console.log(es5,es6)

//{a: "c", b: "c"}, {b: "c"}

// 新增api
Object.is('abc','abc')
Object.assign({a:1},{b:'b'}) //{a: 1, b: "b"}


let test = {k:123,v:456}
for(let [key,val] of Object.entries(test)) {
    console.log(key,val)
}
// k 123
// v 456
```
- Symbol
```javascript   
let a1 = Symbol()
let a2 = Symbol()
a1 == a2 //false

let a3 = Symbol.for('a3')
let a4 = Symbol.for('a3')
a3 == a4 //true

let aa = Symbol.for('abc')
let obj = {
    [aa]:123,
    'abc':345,
    'c':456
}
console.log(obj)

/* 
{abc: 345, c: 456, Symbol(abc): 123}
abc: 345
c: 456
Symbol(abc): 123 
*/

for(let [attr,val] of Object.entries(obj)) {
    console.log(attr,val)
}

/* 
abc 345
c 456 
不会循环到Symbol*/

Object.getOwnPropertySymbols(obj).forEach(item=>console.log(item,obj[item]))
//Symbol(abc),123

Reflect.ownKeys(obj).forEach(item=>console.log(item))
/*
abc
c
Symbol(abc) */
```
- set-map结构
```javascript
let a1 = new Set()
a1.add(2)
a1.add(3)
add.size; //2


{
   let arr = [1,2,3,4]
   let list = new Set(arr)
   lise.size;//4 
}
//可用于数组去重
{
    let arr = [1,2,1,2,1]
    let arr1 = Array.from(new Set(arr))   //[1, 2]
    arr instanceof Array  //true
}

{
    let arr = ['add','delete','clear','has']
    let list = new Set(arr)
    list.has('add')  //true
    list.delete('add') //list ==> {"delete", "clear", "has"}
    list.clear() //list ==> {}
}

//遍历
for(let val of arr.values()) {
    console.log(val)
}

/* 
add
delete
clear
has 
*/

for(let attr of arr.keys()) {
    console.log(attr)
}
/* 
0
1
2
3 
*/

for(let [attr,val] of arr.entries()) {
    console.log(attr,val)
}
/*
0 "add"
1 "delete"
2 "clear"
3 "has" 
*/

list.forEach(item =>console.log(item))
/* 
add
delete
clear
has
 */


{
    //weekSet()必须是对象，不会检测，不用担心内存泄漏，如果别的对象不引用该对象， 这个对象会被垃圾回收机制自动回收；
    let weakList = new WeakSet()
    let arg = {}
    weakList.add(arg)
    weakList.add(2) //报错
    // 没有clear方法，没有set属性，不能遍历
    console.log(weakList)
}


{
    let list = new Map()
    let arr = ['123']
    list.set(arr,456)
    console.log(list,list.get(arr)) //Map(1) {Array(1) => 456} 456
}

{
    let map = new Map([['a',123],['b',456]])
    console.log(map) //Map(2) {"a" => 123, "b" => 456}
    map.size; //2
    map.delete('a')
    map.clear()
}

{
    let weakMap = new WeakMap()   //===>和WeakSet()相同
}

// set-map与数组和对象的对比
{
    let map = new Map()
    let array = []
    // 增
    map.set('t',1)
    array.push({t:1})
    console.info(map,array)
    // Map(1) {"t" => 1} [{t: 1}]

    // 删
    map.delete('t')
    let index = array.findIndex(item =>item.t)
    array.splice(index,1)
    console.info(map,array)
    // Map(0) {} []

    //改
    map.set('t',2)
    // array['t'] = 2
    array.forEach(item=>item.t? item.t = 2:'')
    console.info(map,array)
    // Map(1) {"t" => 2} [t: 2]

    // 查
    map.has('t')
    array.find(item=>item.t)
     // Map(1) {"t" => 2} [t: 2]
}

{
    let set = new Set()
    let array = []
    // 增
    set.add('t',1)
    array.push({t:1})
    console.log(set,array)
    // {"t" => 1} [{t: 1}]

    // 删
    set.delete('t')
    let index = array.forEach(item=>item.t)
    array.splice(index,1)
    console.log(set,array)

    // 改 ==>有问题
    set.forEach(item=>item.t?item.t = 2:'')
    array.forEach(item=>item.t?item.t = 2:'')
     console.log(set,array)

    // 查  ==》有问题
    set.has('t')
    console.log(set)
}

{
    let set = new Set()
    let map = new Map()
    let obj = {}
    // 增
    set.add('t',1)
    map.set('t',1)
    obj['t'] = 1
    console.log(set,map,obj)
    // Set(1) {"t"} Map(1) {"t" => 1} {k: 1}

    // 删
    set.delete('t')
    map.delete('t')
    delete obj['t']
    console.log(set,map,obj) 
    //{}{}{}
    // 改
    map.set('t',2)
    obj['t'] = 2
    // 查
    console.log({
        a: set.has('t'),
        b:map.has('t'),
        c:'t' in obj
    })
    //true true true
}
```
