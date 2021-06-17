---
title: ES6系列-Iterator与for of
categories: js
tags: [编程,学习]
---
```javascript
/* 
什么是Iterator接口
Iterator的基本用法
for...of 
*/

{
    let arr = ['hello', 'world']
    let map = arr[Symbol.iterator]()
    console.log(map.next())
    console.log(map.next())
    console.log(map.next())


// {value: "hello", done: false}
// {value: "world", done: false}
// {value: undefined, done: true}
}

// 使用for..of循环Iterator
{
    let obj = {
        start:[1,2,3],
        end:[4,5,6],
        [Symbol.iterator]() {
            let that = this
            let index = 0
            let arr = that.start.concat(that.end)
            let len = arr.length
            return {
                next() {
                 if(index <len) {
                     return {
                         value:arr[index++],
                         done:false
                     }
                 }else{
                     return {
                         value:arr[index++],
                         done:true
                     }
                 }
                }
            }
        }
        
    }
   for(let key of obj) {
       console.log(key)
   }
    // 1
    // 2
    // 3
    // 4
    // 5
    // 6
}


{
    let arr = ['hello','world']

    for(let value of arr) {
        console.log(value)
    }

   // hello
   // world
}
```