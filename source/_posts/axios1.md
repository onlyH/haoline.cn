---
title: axios配置
categories: vue
date: 2018-02-28
tags: [编程,感悟]
copyright: true
---

```javascript
//引用
import Axios from 'axios'
//给vue原型挂载一个属性
Vue.prototype.$axios = Axios


// 初始化
created() {
    //get
    this.$axios.get('http://192.168.1.1')
    .then(res=>{
        console.log(res)
        this.data = this.data.message
    })
    .catch(err=>{
        console.log(err)
    })
 //post
 this.$axios.post('http://192.168.1.1',{content:'hello world'},
 { headers:{'content-type':'appliction/x-www-form-urlencoded'}})
 .then(res =>{
     this.data = res.data.message
 })
 .catch(err=>{

 })
}
```
