---
title: ES6系列-proxy,Reflect
date: 2017-05-01
categories: js
tags: [编程,学习]
---

```javascript

// 代理 proxy 连接用户和最真实的层

// reflect反射 ==> object

{
    let obj = {
        name: 'lele',
        time: '2012-12-12',
        _r: 123
    }
    let monitor = new Proxy(obj, {
        // 拦截对象属性的读取
        get(target, key) {
            return target[key].replace('2012', '2018')
        },
        // 拦截对象设置属性
        set(target, key, value) {
            if (key == 'name') {
                return target[key] = value
            } else {
                return target[key]
            }
        },
        // 拦截key in object操作
        has(target, key) {
            if (key == 'name') {
                return target[key]
            } else {
                return false
            }
        },
        // 拦截delete
        deleteProperty(target, key) {
            if (key.indexOf('_') > -1) {
                delete target[key]
                return true
            } else {
                return target[key]
            }
        },
        // 拦截Object.keys,Object.getOwnProperty,Object,getOwnPropertyNames
        ownKeys(target) {
            return Object.keys(target).filter(item => item != 'time')
        }
    })
    console.log('get', monitor.time) //2018-12-12
    monitor.name = 'dd'
    console.log('set', monitor) //Proxy {name: "dd", time: "2012-12-12", _r: 123}
    console.log('has', 'name' in monitor, 'time' in monitor) //has true false
    delete monitor.name;
    delete monitor._r
    console.log('delete', monitor) //delete Proxy {name: "dd", time: "2012-12-12"}
    console.log('ownkey', Object.keys(monitor)) //ownkey (2) ["name", "_r"]
}

{
    let obj = {
        name: 'lele',
        time: '2012-12-12',
        _r: 'dog'
    }
    let monitor = Reflect.get(obj, 'time')
    console.log(monitor) //2012-12-12
    let setName = Reflect.set(obj, 'name', 'yoyo')
    console.log("setName", setName)  //true
    Reflect.set(obj, 'name', 'pp')
    console.log(obj) //{name: "pp", time: "2012-12-12", _r: "dog"}

    console.log('has', Reflect.has(obj, 'name')) //true

}

//对数据进行校验 ====>使用场景
// 建立函数，提供代理
{
    function validator(target, validator) {
        return new Proxy(target, {
            _validator : validator,
            set(target, key, value, proxy) {
                if (target.hasOwnProperty(key)) {
                    //判断是否满足条件
                    let va = this._validator[key];
                    if (!!va(value)) {
                        return Reflect.set(target, key, value, proxy)
                    } else {
                        throw Error(`不可以设置`)
                    }
                } else {
                    throw Error(`${key} 报错了`)
                }
            }
        })
    }
    // 校验条件
    const personValidator = {
        name(val) {
            return typeof val === 'string'
        },
        age(val) {
            return typeof val === 'number' && val > 18
        }
    }
    class Person {
        constructor(name, age) {
            this.name = name
            this.age = age
            return validator(this, personValidator)
        }
    }
    const people = new Person('lili', 28)
    console.log(people) //Proxy {name: "lili", age: 28}
   /*  people.name = 'rr'
    console.log(people)
    people.age = 12
    console.log(people) */
    const people1 = new Person('yoy',12)
    console.log(people1)
}
```