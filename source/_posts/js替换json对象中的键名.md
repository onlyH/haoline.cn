---
title: js替换json对象中的键名
categories: js
date: 2021-03-17 15:33:03
tags: [编程,功能]
---
使用map()是目前想到的最简单的办法。。
> Map 对象保存键值对。任何值(对象或者原始值) 都可以作为一个键或一个值。
[Objects 和 maps 的比较](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map#Objects_%E5%92%8C_maps_%E7%9A%84%E6%AF%94%E8%BE%83)
```javascript   
var data = [
    {count:123,goods:'小米'},
    {count:456,goods:'华为'},
    {count:789,goods:'苹果'}
].map(item=>{
    return{
        name:item.count,
        value:item.goods
    }
})
```