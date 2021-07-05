---
title: ES6系列-generator
date: 2021-03-17 15:33:03
categories: js
tags: [编程,学习]
---
1. generator基本定义,返回的结果类似于Iterator，其实就是返回的Iterator接口，当函数运行的时候，调用一次next(),会执行一个yield.一个遍历器生成函数，赋值给Symbol.iterator，从而使这个接口Iterator
2. 任何一个iterator接口都会指向Symbol.iterator属性上
```javascript
// 异步编程的解决方案
{
    let fn = function* () {
        yield 'a'
        yield 'b'
        return 'c'
    }
    let test = fn()
    console.log(test.next())
    console.log(test.next())
    console.log(test.next())
    console.log(test.next())

    // {value: "a", done: false}
    //{value: "b", done: false}
    //{value: "c", done: true}
    //{value: undefined, done: true}

}

//  改写Iterator，使用for..of循环对象

{
    let obj = {}
    obj[Symbol.iterator] = function* () {
        yield 1
        yield 2
        yield 3
    }
    for (let value of obj) {
        console.log(value)
    }

    /* 
        1
        2
        3
    */
}
// 什么时候优势最大 -- 状态机  a-b-c-a...
{
    let state = function* () {
        while (1) {
            yield 'a'
            yield 'b'
            yield 'c'
        }
    }
    let status = state()
    console.log(status.next())
    console.log(status.next())
    console.log(status.next())
    console.log(status.next())
    console.log(status.next())


    // {value: "a", done: false}
    // {value: "b", done: false}
    // {value: "c", done: false}
    // {value: "a", done: false}
    // {value: "b", done: false}
}
// 语法糖 async
{
    let state = async function () {
        while (1) {
            await 'a';
            await 'b';
            await 'c';
        }

    }
    let status = state()
    console.log(status.next())
    console.log(status.next())
    console.log(status.next())
    console.log(status.next())
    console.log(status.next())

}

// 抽奖逻辑
{
    let draw = count => {
        //具体逻辑。。
        console.log(`还剩下${count}次`)
    }

    let residus = function* (count) {
        while (count > 0) {
            count--;
            yield draw(count)
        }
    }
    let start = residus(5) //5为从后台取得值
    let btn = document.createElement('button')
    btn.innerHTML = '按钮'
    btn.id = 'startBtn'
    document.body.appendChild(btn)
    btn.addEventListener('click', function () {
        start.next()
    }, false)

}


{
    //长轮询
    let ajax = function* () {
        yield new Promise((resolve, reject) => {
            setTimeout(() => {
                reslove({ code: 1 })
            }, 200);
        })
    }
    let pull = () => {
        let generator = ajax()
        let step = generator.next()
        step.value.then(d => {
            if (d.value != 0) {
                setTimeout(() => {
                    console.log('wait')
                }, 1000);
            } else {
                console.log(d)
            }
        })
    }
}
```