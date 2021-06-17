---
title: js高阶函数
date: 2017-12-02
categories: js
tags: [编程,学习]
---

```

function greentng() {
    console.log('hello world')
}
greentng() //hello world


greentng.lang = 'English'
console.log(greentng.lang) //English


//可以在 JavaScript 中将函数赋值给变量
const square = function (x) {
    return x * 2
}
const total = square;
total(4)  //8
//将函数作为参数传递
function format(type, one, two) {
    if (type == 'oneCase') {
        one()
    } else if (type == 'twoCase') {
        two()
    }
}
function one() {
    console.log('this is one')
}
function two() {
    console.log('this is two')
}
format('oneCase', one, two)

/* 
高阶函数是对其他函数进行操作的函数，操作可以是将它们作为参数，或者是返回它们。
 简单来说，高阶函数是一个接收函数作为参数或将函数作为输出返回的函数。
 例如，Array.prototype.map，Array.prototype.filter 和 Array.prototype.reduce 是语言中内置的一些高阶函数。
*/



/* 
Array.prototype.map
map() 方法通过调用对输入数组中每个元素调用回调函数来创建一个新数组。
map() 方法将获取回调函数中的每个返回值，并使用这些值创建一个新数组。
传递给 map() 方法的回调函数接受 3 个参数：element，index 和 array。
*/

var num = [1, 2, 3, 4, 5]
var arr = []
for (var i = 0; i < num.length; i++) {
    arr.push(num[i] * 2)
}
console.log(arr)


// map

var num = [1, 2, 3, 4]
var arr = num.map(item => item * 2)
console.log(arr)

//假设我们有一个包含不同人的出生年份的数组，我们想要创建一个包含其年龄的数组。 例如：
const year = [1993, 1923, 1983, 1990]
const age = []

for (var i = 0; i < year.length; i++) {
    let change = 2018 - year[i]
    age.push(change)
}
console.log(age)


// map
const year = [1993, 1923, 1983, 1990]
const age = year.map(item => 2018-item)
console.log(age)


/* 
Array.prototype.filter
filter() 方法会创建一个新数组，其中包含所有通过回调函数测试的元素。
 传递给 filter() 方法的回调函数接受3个参数：element，index 和 array。
*/


//假设我们有一个包含名称和年龄属性的对象数组。 我们想要创建一个只包含成年人（年龄大于或等于18）的数组。

var persons = [
    {name:'peter',age:16},
    {name:'lele',age:26},
    {name:'shaun',age:18},
    {name:'tony',age:46},
    {name:'jack',age:54},

]
var adult = []

for(var i = 0 ;i <persons.length;i++) {
    if(persons[i].age >=18) {
        adult.push(persons[i])
    }
}
console.log(adult)
//filter

var persons = [
    {name:'peter',age:16},
    {name:'lele',age:26},
    {name:'shaun',age:18},
    {name:'tony',age:46},
    {name:'jack',age:54},

]
var adult = persons.filter(item =>item.age >=18)
console.log(adult)

/* 
Array.prototype.reducereduce 方法对调用数组的每个元素执行回调函数，最后生成一个单一的值并返回。
 reduce 方法接受两个参数：1）reducer 函数（回调），2）一个可选的 initialValue。
 reducer 函数（回调）接受四个参数：accumulator，currentValue，currentIndex，sourceArray。
 如果提供了 initialValue，则累加器将等于 initialValue，currentValue 将等于数组中的第一个元素。
 如果没有提供 initialValue，则累加器将等于数组中的第一个元素，currentValue 将等于数组中的第二个元素。

*/
//假设我们要对一个数字数组的求和：

var sum = [1,2,3,4,5]
var total = sum.reduce((a,b) => a+b)
console.log(total)


/* 
每次对数组中的某个值调用 reducer 函数，
累加器都会保留上一次 reducer 函数操作返回的结果，
并将 currentValue 设置为数组的当前值。 
最后把结果存储在 sum 变量中。
我们还可以为它提供初始值：
*/

var num = [1,2,3,4]
var total = 0;
for(var i = 0 ;i <num.length;i++) {
    total +=num[i]
}
console.log(total)

//reduce
var num = [1,2,3,4,5,6]
var total = num.reduce((a,b) =>{return a+b},10)
console.log(total)



/* 
我们假设 JavaScript 没有原生的 map 方法。
我们可以自己构建它，从而创建我们自己的高阶函数。 
假设我们有一个字符串数组，我们希望把它转换为整数数组，其中每个元素代表原始数组中字符串的长度。
*/

var strArray = ['javascript','python','php','java','c']
function mapEach(arr,fn) {
    var num = []
    for(var i = 0; i<arr.length;i++) {
        num.push(fn(arr[i]))
    }
    return num;

}
var strLength = strArray.mapEach((strArray,()=>))

```