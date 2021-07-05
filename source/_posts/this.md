---
title: this
date: 2017-12-02 15:33:03
categories: js
tags: [编程,学习]
---
##### this
```javascript   
    function identify() {
        return this.name.toUpperCase(this)
    }
    function speak() {
        var greeting = 'hello' + identify.call(this)
        console.log(greeting)
    }
    var me = {
        name:'lele'
    }
    var you = {
        name:'tom'
    }
    identify.call(me)
    identify.call(you)

    speak.call(me)
    speak.call(you)
    //这段代码可以在不同的上下文对象(me 和 you)中重复使用函数 identify() 和 speak()， 不用针对每个对象编写不同版本的函数。



    //如果不使用 this，那就需要给 identify() 和 speak() 显式传入一个上下文对象。

    function identify(context) {
        return context.name.toUpperCase()
    }
    function speak(context) {
        var green = 'hello' + identify(context)
        console.log(green)
    }
    identify(me)
    identify(you)





    function nums(i) {
        console.log('nums' + i );
        this.count++
    }
    foo.count = 0;
    for(var i = 0; i<10;i++) {
        if(i > 5) {
            nums(i)
        }
    }
    console.log(nums.count)


    function nums(i) {
        console.log('nums' + i)
        data.count++
    }
    var data = {
        count : 0
    }
    for(var i = 0; i< 10;i++) {
        if(i<5) {
            nums(i)
        }
    }
    console.log(data.count)


    //如果要从函数对象内部引用它自身，那只使用 this 是不够的。一般来说你需要通过一个指 向函数对象的词法标识符(变量)来引用它。
    function foo() {
        foo.count = 4; //foo指向它自身
    }
    setTimeout(function() {
        // 匿名(没有名字的)函数无法指向自身
    },10)
//第一个函数被称为具名函数，在它内部可以使用 foo 来引用自身。
//但是在第二个例子中，传入 setTimeout(..) 的回调函数没有名称标识符(这种函数被称为 匿名函数)，因此无法从函数内部引用自身。


//使用 foo 标识符替代 this 来引用函数 对象:
function foo(num) {
    console.log('foo' + num)
    foo.count++;
}
foo.count = 0;
for(var i = 0; i < 10; i++) {
    if(i > 5) {
        foo(i)
    }
}
```
- 强制this指向foo函数对象
```javascript
function foo(num) {
    console.log('num' + num)
    this.count++
}
foo.count = 0;
for(var i = 0; i < 10; i++) {
    if(i > 5) {
        foo.call(foo,i)
    }
}
```