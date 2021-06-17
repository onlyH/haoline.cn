---
title: js条件语句
categories: js
tags: [编程,感悟]
---

#### 使用Array.includes来处理多重条件
[Array.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)

```javascript
//bad
function test(fn) {
    if(fn === 'apply' || fn == 'pear') {
        console.log('yes')
    }
}
//如果筛选条件多
function test(param) {
    const fruits = ['apply','pear','banana'] //条件提取到数组
    if(fruits.includes(param)) {
        console.log('yes')
    }
  }
```
* 少写嵌套，尽早返回
    + 如果没有水果，抛出错误
    + 如果该水果的数量大于10，将其打印出来
```javascript
//bad
function test(param, num) {
    const fruit = ['apple', 'pear', 'cherry']
      // 条件 1：fruit 必须有值
    if (param) {
            // 条件 2：必须存在
        if (fruit.includes(param)) {
            console.log('red')
        //数量大于 10
            if (num > 10) {
                console.log('more')
            }
        }
    } else {
        throw new Error('no frulte')
    }
}
// 测试结果
test(null); // 报错：No fruits
test('apple'); //red
test('apple', 20) //red


function test(p,num) {
    const fruits = ['apply','pear']
    if(!p) throw new Error('no')
    if(fruits.includes(p)) {
        console.log('yes')
        if(num>10) {
            console.log('good')
        }
    }
}

function test(p,num) {
    const fruits = ['apply','pear','chreey']
    if(!p) throw new Error('no')
    if(!fruits.includes(p)) return  //不是直接返回
    console.log('red')
    if(num>10) console.log('good')
}
```
#### 使用函数默认参数和解构
```javascript 
function test(fruit,num) {
    if(!fruit) return
    let q = num || 1
    console.log(`we have ${q} ${fruit}`)
}
//测试结果
test('banana'); // We have 1 banana!
test('apple', 2); // We have 2 apple!

function test(fruit,num = 1) { 
    if(!fruit) return
    console.log(`we have ${fruit} ${num}`)
 }  
//如果fruit是一个对象
function test(fruit) {
     if(fruit && fruit.name) {
         console.log(fruit.name)
     }else{
         console.log('unknow')
     }
 }

//测试结果
test(undefined); // unknown
test({ }); // unknown
test({ name: 'apple', color: 'red' }); // apple

//可以通过默认参数和解构赋值的方法来避免写出 fruit && fruit.name 这种条件。
function test({name} ={}) {
    console.log(name || 'unknow')
}
//解构只适用于对象（Object）
```
#### 相较于 switch，Map / Object 也许是更好的选择
```javascript  
function test(color) {
    switch(color) {
        case 'red':
        return ['apple', 'strawberry'];
        case 'yellow':
        return ['banana', 'pineapple'];
        case 'purple':
        return ['grape', 'plum'];
        default:
        return [];
    }
}
//测试结果
test(null); // []
test('yellow'); // ['banana', 'pineapple']

const fruit = {
    red: ['apple', 'strawberry'],
    yellow: ['banana', 'pineapple'],
    purple: ['grape', 'plum']
}
function test(color) {
    return fruit[color] || []
  }

//Map
const fruit = new Map()
.set('red', ['apple', 'strawberry'])
.set('yellow', ['banana', 'pineapple'])
.set('purple', ['grape', 'plum']);

function test(color) {
    return fruit[color] || []
  }

//Array.filter
const fruit = [
    { name: 'apple', color: 'red' }, 
    { name: 'strawberry', color: 'red' }, 
    { name: 'banana', color: 'yellow' }, 
    { name: 'pineapple', color: 'yellow' }, 
    { name: 'grape', color: 'purple' }, 
    { name: 'plum', color: 'purple' }
]
function test(color) {
    return fruit.filter(f =>f.color = color)
  }


```

#### 使用 Array.every 和 Array.some 来处理全部/部分满足条件
```javascript
const fruits =[
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
  ]
function test() {  
    let isAll = true
    //所有水果都必须是红色
    for(let f of fruits) {
        if(!isAll) break
        isAll = (f.color =='red')
    }
    console.log(isAll) //false
}


//Array.every
const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
]
function test() {
    //所有水果必须都是红色
    const isAll = fruits.every(f =>f.color =='red')
    console.log(isAll) //false
  }

  //Array.some
  const fruits = [
    { name: 'apple', color: 'red' },
    { name: 'banana', color: 'yellow' },
    { name: 'grape', color: 'purple' }
]
function test() {
        //至少一个水果是红色
    const isAll = fruits.some(f =>f.color=='red')
    console.log(isAll) //true
}

```




