---
title: ES6系列-decorator
categories: js
tags: [编程,学习]
---
- decorator：修饰器是一个函数，修改行为，修改类的行为。
```javascript
{
    let readonly = (target, name, descriptor) => {
        descriptor.writable = false
        return descriptor
    }
    class Test {
        @readonly
        time() {
            return '2012-12-12'
        }
    }
    let test = new Test()
    test.time = () => {
        console.log('reset time')
    }
    console.log(test.time())
}


{
    let typename = (target, name, descriptor) => {
        target.myname = 'hello'
    }
    @typename
    class Test {

    }
    console.log('类的修饰', Test.myname)
}

{
    let log = (type) => {
        return function (target, name, descriptor) {
            let src_method = descriptor.value;
            descriptor.value = (...arg)=>{
                src_method.apply(target.arg);
                console.log(`log ${type}`)
            }
        }
    }
    class AD{
        @log('show')
        show() {
            console.log('ad is show')
        }
        @log('click')
        click() {
            console.log('ad is click')
        }
    }
    let ad = new AD()
    ad.show()
    ad.click()


    /* 
    ad is show
    log show
    ad is click
    log click
    */
}

```