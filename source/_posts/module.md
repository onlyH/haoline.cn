---
title: ES6系列-模块化
categories: js
tags: [编程,学习]
---

```javascript
export let A = 123; //导出一个变量
export function test() {
    console.log('test')
}
export class Hello{
    test() {
        console.log('class')
    }
}

import {A,test,Hello} from './index'
import * as lesson from './index'
console.log(lesson.A)

{
    let A = 123;
    export function test() {
        console.log('test')
    }
    class Hello{
        test() {
            console.log('hello')
        }
    }

    export default{
        A,
        test,
        Hello
    }



    import 任意变量名 from './index'
}
```