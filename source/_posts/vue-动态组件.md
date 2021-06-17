---
title: vue-动态组件
categories: vue
date: 2018-12-28
tags: [编程, 功能]
---

让多个组件使用同一个挂载点，并动态切换，这就是动态组件。通过使用保留的 `<component>` 元素，动态地绑定到它的 `is`特性，我们让多个组件可以使用同一个挂载点，并动态切换。根据`v-bind:is="组件名"`中的组件名去自动匹配组件，如果匹配不到则不显示。
有一个功能需求，就是类似于官网上的动态组件的样子。点击会切换，在实现的过程中，有遇到问题，也请教了别人，记录下。

```javascript
// 两个子组件txt，file
Vue.component('txt',{
    template:``,
    methods:{
// 子组件暴露给外界的值
getValue: function () {
      return {
        type: 'text',
        content: this.inner
      }
    }
    },
    prop:["content"]
})
Vue.component('file',{
    template:``,
    methods:{
// 子组件暴露给外界的值
getValue: function () {
      return {
        type: 'text',
        content: this.filesName
      }
    }
    },
    prop:["content"]
})
// 父组件调用子组件
  <div class="despContainer" v-for="(desp,index) in desps">
    <component :is="comps[desp.type]" :content="desp" :ref="'comp' + index"/>
</div>


// desps的数据结构

// props：
// content:Object
// inner:'xxx'
// type:"text"


// 父组件去循环，返回子组件暴露的值
allDate() {
    let data = [];
    for(let comp in this.desps) {
        const comIns = this.$refs['comp'+comp][0]
        if(!comIns || typeof compIns.getValue !=='function'){
            return
        }
        if(compIns.getValue()) {
            data.push(compIns.getValue())
        }
    }
    return data
}

// 调用
let introduces = this.fetchAllData()

```

- 如果子组件要调用父组件的方法，可以使用\$emit()传值，v-on 监听

```javascript

<txt-component @check-title='checkTitle'/>

this.$emit('check-title', msg)

```
