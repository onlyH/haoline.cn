---
title: vue简易下拉框
categories: vue
date: 2018-12-14
tags: [编程,学习]
---
使用vue模拟了一个简易的下拉框，主要是根据传入当前索引实现数据切换的。具体实现代码如下：
```html
  <div class="dropdown show">
    <div class="dropdown-toggle" @click="toggleDrop"  ref="listParents">
    {{content.languages[nowIndex].name}}
    </div>

    <div class="drop">
    <a class="dropdown-item"
        v-show="isDrop" 
        v-for="(ops,index) in content.languages" 
        :key="ops.id" 
        @click="chooseSelection(index)">{{ops.name}}</a>
    </div>
</div>
```
```javascript
data(){
    return {
        isDrop:false,
        nowIndex:0,
        content{
            languages:{
                [
                    name:'lele',
                    id:1
                ],
                [
                    name:'xiaohong',
                    id:2
                ],
            }
        }
    }
},
methods:{
    toggleDrop() {
        this.isDrop = !this.isDrop
    },
    chooseSelection(index) {
        this.nowIndex = index;
        this.isDrop = false
    }
}
```
关于点击空白处隐藏，具体实现的思路是：
1. 给document添加一个事件监听
2. 当发生点击事件的时候判断点击的是否是当前对象（vue使用ref）
3. 此段js写在mounted函数里面
```javascript
mounted() {
    document.addEventListener('click',e=>{
        if(!this.$refs.listParents.contains(e.target)) { //这句是说如果我们点击到了listParents以外的区域
            this.isDrop = false
        }
    })
}
```
> 原生JS中是有contains方法的,但它并不是字符串方法，，仅用于判断DOM元素的包含关系，参数是Element类型 
> JS中通用的contains方法判断两个节点的关系