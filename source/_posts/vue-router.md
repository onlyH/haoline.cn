---
title: vue日常学习-router
categories: vue
date: 2018-03-02
tags: [编程,感悟]
copyright: true

---

今日搭建vue 3.0脚手架，写了个小项目，被一个问题卡了半天，就是在app.vue里面的router-view和router-link无法生效，页面不会跳转并且控制台会报错，请教后得知，自己在router.js里配置路由的时候代码写错了
```javascript
//错误代码
{
      path: '/shopping',
      name: 'shopping',
      component: () => {
        import("./components/shopping")
      }
    }
    
//正确的写法 1
{
      path: '/shopping',
      name: 'shopping',
      component: () => import("./components/shopping")
      
    }
//正确的写法 2
{
      path: '/shopping',
      name: 'shopping',
      component: () => {
        return import("./components/shopping")
      }
    }

//import是个异步函数, 单纯的componet方法没有返回值,如果加了{}，需要使用return返回出去
```