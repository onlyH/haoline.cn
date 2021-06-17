---
title: vue日常学习
categories: vue
date: 2018-03-01
tags: [编程,感悟]
copyright: true

---

* 路由操作的基本步骤
```javascript
//引入对象
import VueRouter from ’vue-router‘
//安装插件
Vue.use(VueRouter); //挂载属性的行为
//创建理由对象
let router = new VueRouter({
    routers:[{
        name:'xxx',path:'/xxx',组件
    }]
})
//将路由对象放入到options中的 new Vue()
new vue({
    router
})
```
* 设置
- 1: <router-link :to={name:'xxx'}></router-link>
- 2: 配置路由规则`{name:'xxx',path:'xxx',组件名}`
- 3: 作了什么
        + 在created事件函数中，获取路由参数
        + 发起请求，把数据挂在上去
* 参数
    - 查询字符串(#/beijing?id=1&age=2)
+ 1: <router-link :to={name:   'xxx',query:{id=1,age=2}}></router-link>
+ 2: 配置路由规则`{name:'xxx',path:'beijing',组件名}`
+ 3: 作了什么
        + `this.$route.query.id || age`

    - path(#/beijing/1/2 )
+ 1: <router-link :to={name:'xxx',params:{id:1,age:2}}></router-link>
+ 2: 配置路由规则`{name:'xxx',path:'/beijing/:id/:age',组件名}`
+ 3: 作了什么
        + `this.$route.params.id || age`

* 编程导航
    - 一个获取信息的只读对象($route)
    - 一个具备功能函数的对象($router)
    - 根据浏览器历史记录前进和后退`this.$router.go(-1 || 1)`
    - 跳转到指定路由`this.$router.push({name:'bj})`

* 嵌套路由
    - 让变化的视图(router-view)产生包含关系(router-view)
    - 让路由与router-view关联，并且产生父子关系

#### axios
* 合并请求
* axios.all{[请求1，请求2]}
* 分发相应 axios.pread(fn)
* fn:对应参数和请求的顺序一致。
* 必须两次请求都成功，只要有一次失败就算失败，否则成功。
```javascript
//main.js

import Vue from 'vue'
import App from 'app'
//引入
import Axios from 'axios'

Axios.defaults.baseURL = 'http://123.34.543/api/';

//给Vue原型上挂载属性
Vue.prototype.$axios = Axios

//启动
new Vue({
    el:'#app',
    render:c => c(App)
})

//app.vue
created() { 
    function getMsg(res1,res2) {
        console.log(res1)
        console.log(res2)
    }
    this.$axios.all([
        this.$axios.post('postcomment/300','content=123'),
        this.$axios.get('postcomment/300','content=123')
    ])
    //分发相应
    .then(this.$axios.spread(getMsg))
    .catch(err =>{
        console.log(err)
    })
}
```

#### 拦截器
* 过滤，再一次请求中，做操作,拦截器对每一次请求都有效 
* axios.interceptors.request.use(fn) 在请求之前
* function(config) {} config相当于options对象
* 范围广
```javascript
//main.js
Axios.defaults.headers = {
    accept:'defaults'
}
//拦截器
Axios.interceptors.request.use(function(config) {
    console.log(config)
    //个性化修改
    //config.headers.accept = 'interceptors'
     config.headers = {
         accept:'interceptors'
     }
    return config
    //返回没有修改的设置
})
//拦截器覆盖默认设置
```
```javascript
this.$axios.get('getcomments/300?pa=1',{
    headers:{
        accept:'get'
    }
})
.then(res =>{

})
.catch(err =>{

})
```

#### token
* cookie和session的机制，coolie自带一个字符串
* cookie只在浏览器
* 移动端原生应用，也可以使用http协议，可以加自定义的头，原生应用没有cookie
* 对于三端，token可以作为类似cookie的使用，并且可以通用
* 拦截器可以用在添加token上

#### 拦截器操作 loading
* 在请求发起前open，在相应回来后close
```javascript
Axios.interceptors.request.use(function(config) {
    //请求发起之前，显示loading
    return config
})
Axios.interceptors.response.use(function(config) {
    //在相应回来之后，隐藏loading
    return config
})
```  

#### 监听
* watch可以对（单个）变量进行监视，也可以深度监视
* 如果需求是对于10个变量进行监视？computed，可以监视多个，并且指定返回数据，并且可以显示在页面
* 都是options中的根属性
    - watch监视单个
    - computed可以监视多个this相关属性值的改变，如果和原值一样，不会触发函数的调用，并且可以返回对象
```javascript
 <input type="text" v-model="text">
<button @click="changeValue"></button>

 data() {
      return{
        text:[],
        person:[{
          name:'nick'
        },{
          name:'lee'
        }]
      }
    },
    methods:{
      changeValue() {
        this.text = 'abc'
        this.person[0].name = 'tom'
      }
    },
    watch:{
      text:function (old,newV) {
        console.log('change it')
      },
      person:{
        handler:function (val,old) {
          console.log('change it')
        },
        deep:true
      }
    }
    
    
```


```javascript
 单价：<input type="text" v-model="price">*
 件数：<input type="text" v-model="num">*
 折扣：<input type="text" v-model="rate">=
    {{sum.name}} {{sum.price}}

  data() {
      return {
        price: 0,
        num: 0,
        rate: 0
      }
    },
    computed: {
      sum() {
        //如果当函数内涉及到的this.相关属性发生变化后触发，并返回一个值（可以是对象）
        return {
          name:'music',
          price:this.price * this.num * (this.rate / 100)
        }
      }
    }
```


