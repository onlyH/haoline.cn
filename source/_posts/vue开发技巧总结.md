---
title: vue开发技巧总结 
date: 2021-07-09 11:34:11 
tags:
---
<blockquote class="blockquote-center">
日常开发总结
</blockquote>

<!-- more -->

### vue2

#### props 传值

```vue
<!-- 静态的prop -->
<blog-post title="My journey with Vue"/>
<!-- 动态的prop传递可以简写成 -->
<blog-post :title="data.title"/>
<!-- 需要传递多个prop的时候，可以一起写在v-bind上 -->
<blog-post v-bind="{ table, title: data.title}"/>

```

#### $attrs  包含了传入到父作用域中没有在 props 声明的其他 props，因此我们可以用 $attrs 去代替那些父组件中不需要的而子组件需要的 props， 通过 v-bind="$attrs" 统一传递给后代。这样就避免了一个个声明然后再一个个传递。
```vue
<blog-post v-bind="$attrs"/>
```
> 包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件——在创建更高层次的组件时非常有用。

$listeners 表示了父组件中的事件监听器集合，只要是触发父组件的事件，而不是自己的，就可以用一个 v-on="$listeners"表示。

```vue
<!-- 父组件（第一层组件） -->
<componentA @on-change="handleChange" v-bind="{ editable, title: post.title}" />

<!-- 中间层的组件 -->
<Child v-bind="$attrs" v-on="$listeners"/>

<!-- 数据传递的目标组件，事件触发的组件 -->
<div @click="handleClick">{{ title }} </div>
<script>
  export default {
    props: {
      title: String
    }
    handleClick() {
      this.$emit('on-change', 'New Title');
    }
  }
</script>

<!--中间层的组件内通过 v-bind="$attrs" 将其余的 Prop 传递给了 Child 组件，再通过 v-on="$listeners" 绑定父作用域中的事件监听器，一旦 emit 就会传给了父组件。-->
```

#### 实现双向数据绑定

传统做法：props + $emit (笨拙，不易维护)

优化方法：<strong>使用 .sync 实现 Prop 的“双向绑定”</strong>

解释：在 v-bind prop的时候添加 .sync 修饰符，赋新值的时候用 this.$emit('update:propName', newValue)

```vue
<!-- .sync是 v-on:update这种模式的一种缩写 -->
<Child v-on:update:title="title" />
<!-- 相当于 -->
<Child :title.sync="title" />
<!--数据更新-->
this.$emit('update:title', '新标题')
```


#### 使用 model 选项 （2.2.0+）
   
 一个组件上的 v-model 默认会利用名为 value  的 Prop  和名为 input 的事件， 而 model 选项可以规定 Prop 名称和事件名称来实现 v-model，好处是在实现 v-model 的同时也避免了 Prop 和事件名的冲突。

```vue
<!-- 父组件 -->
<Model v-model="checked"/>

<!-- Model组件 -->
<div @click="handleClick">
  <p>自定义组件的 v-model</p>
  checked {{checked}}
</div>
<script lang="ts">
export default {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  methods: {
    handleClick() {
      this.$emit('change', !this.checked);
    }
  }
<!-- TS -->
  @Model('change', { type: Boolean }) readonly checked!: boolean
handleClick() {
  this.$emit('change', !this.checked);
}
```

#### Mixins
   
   利用它去抽取成组件内的公共代码加强代码复用，不要在全局内套来套去，最好在组件内或者页面中使用。
   
   利用它去分离功能点，有时候会遇到一种情况，就是业务功能很多导致写起来的 Vue 文件行数很多，导致代码很难以维护，功能点代码不好追踪。可以通过抽取功能代码的方式，让这个庞大的 Vue 文件更好维护。

```vue
export default class CommonMixins extends Vue{
    public paginations = {
        pageSize: 20,
        total: 0,
        currentPage: 1,
    }
    handleChangePageSize (pageSize: number, cb: Function) {
        this.paginations.pageSize = pageSize;
        cb();
    }
    handleChangePageNum (currentPage: number, cb: Function) {
        this.paginations.currentPage = currentPage;
        cb();
    }
}

```

vue-property-decorator 提供了 Mixins 的装饰器，在业务页面中引入 Mixin 只需要往里 Mixins 传入 ， 可以传多个，表示混入多个 Mixin。

```vue
<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator';
import CommonMixins from "./common-mixin";
import PermissionMixins from "./permission-mixin";
@Component({})
export default class Parent extends Mixins(CommonMixins, PermissionMixins) {
}
</script>

<!-- 一个可以直接继承 -->
<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator';
import CommonMixins from "./common-mixin";
@Component({})
export default class Parent extends CommonMixins {
}
</script>

```

#### 使用动态组件去懒加载组件

> 组件在加载都是同步的，但当页面内容很多，有些组件并不需要一开始就加载出来的比如弹窗类的组件，这些就可以用动态组件，当用户执行了某些操作后再加载出来，这样可以提高主模块加载的性能， 在 Vue 中可以使用 component 动态组件， 依 is 的值，来决定哪个组件被渲染。

#### 在组件作用域内的 CSS 中使用 ::v-deep 修改组件样式

::v-deep 和 /deep/ 作用是一样的，但不推荐使用 /deep/， 在 Vue3.0 中将不支持 /deep/ 这种写法


```css
<style lang="scss" scoped>
/deep/ .ivu-tabs-tabpane {
        background: #f1f1f1;
    }
</style>
<style lang="scss" scoped>
::v-deep .ivu-tabs-tabpane {
        background: #f1f1f1;
    }
</style>

```

#### 使用装饰器优化代码

装饰器增加了代码的可读性，清晰地表达了意图，而且提供一种方便的手段，增加或修改类的功能，比如给类其中的方法提供防抖的功能。

```vue
import debounce from 'lodash.debounce';
export function Debounce(delay: number, config: object = {}) {
  return (target: any, prop: string) => {
    return {
      value: debounce(target[prop], delay, config),
    };
  };
}

```
```vue
@Debounce(300)
onIdChange(val: string) {
  this.$emit('idchange', val);
}
```

#### 利用 require.context 去获取项目目录信息

可以给这个函数传入三个参数：一个要搜索的目录，一个标记表示是否还搜索其子目录， 以及一个匹配文件的正则表达式。

webpack 会在构建中解析代码中的 require.context() 。如果想引入一个文件夹下面的所有文件，或者引入能匹配一个正则表达式的所有文件，这个功能就会很有帮助


我们可以引用到一个文件夹下面的所有文件，由此可以利用获取的文件信息去做一些操作，比如在注册组件的时候，原本我们注册组件的时候需要一个个引入并且一个个注册，而且后面想加新的，又要再写上，现在可以通过 require.context 去优化这一段代码。

```vue
import Table from './table/index.vue';
import CustomHooks from './custom-hooks/custom-hooks-actions/index';
import SFilter from './s-filter/filter-form';
import WButton from './button/index';
import CreateForm from './createForm/create-form/CreateForm.vue';
import Action from './table/action-table-column.vue';
import DetailItem from './detail-item.vue';


Vue.component('w-filter', SFilter);
Vue.component('w-button', WButton);
Vue.component('custom-hooks', CustomHooks);
Vue.component('create-form', CreateForm);
Vue.component('w-table', Table);
Vue.component('w-table-action', Action);
Vue.component('zonetime-date-picker', ZonetimeDatePicker);
Vue.component('detail', DetailItem);

```

注册全局组件的时候，不需要一个一个 import，和一个个去注册，使用 require.context 可以自动导入模块，这样的好处在于，当我们新建一个组件，不用自己再去手写注册，而在一开始就帮我们自动完成。

```vue
const contexts = require.context('./', true, /\.(vue|ts)$/);
export default {
  install (vm) {
    contexts.keys().forEach(component => {
      const componentEntity = contexts(component).default;
      if (componentEntity.name) {
        vm.component(componentEntity.name, componentEntity);
      }
    });
  }
};


```

#### 函数组件

函数组件 是无状态的，没有生命周期或方法，因此无法实例化

创建一个函数组件非常容易，你需要做的就是在SFC中添加一个 functional: true 属性，或者在模板中添加 functional。由于它像函数一样轻巧，没有实例引用，所以渲染性能提高了不少。

函数组件依赖于上下文，并随着其中给定的数据而突变。

```vue
<template functional>
  <div class="book">
    {{props.book.name}} {{props.book.price}}
  </div>
</template>

<script>
Vue.component('book', {
  functional: true,
  props: {
    book: {
      type: () => ({}),
      required: true
    }
  },
  render: function (createElement, context) {
    return createElement(
      'div',
      {
        attrs: {
          class: 'book'
        }
      },
      [context.props.book]
    )
  }
})
</script>

```
#### 订阅多个变量突变

watcher 不能监听多个变量，但我们可以将目标组合在一起作为一个新的 computed，并监视这个新的 "变量"。

```vue
computed: {
  multipleValues () {
    return {
      value1: this.value1,
      value2: this.value2,
    }
  }
},
watch: {
  multipleValues (val, oldVal) {
    console.log(val)
  }
}

```
#### 路由器参数解耦
```vue
<!--bad-->
<!--在组件内部使用 $route 会对某个URL产生强耦合，这限制了组件的灵活性。-->
export default {
  methods: {
    getRouteParamsId() {
      return this.$route.params.id
    }
  }
}
```

```vue
<!--正确的解决方案是向路由器添加props。-->
const router = new VueRouter({
  routes: [{
    path: '/:id',
    component: Component,
    props: true
  }]
})

export default {
props: ['id'],
methods: {
getParamsId() {
return this.id
}
}
}
```
```vue
<!--可以传入函数以返回自定义 props-->

const router = new VueRouter({
routes: [{
path: '/:id',
component: Component,
props: router => ({ id: route.query.id })
}]
})

```

#### 组件生命周期 Hook
```vue
<!--监听子组件的生命周期（例如 mounted）-->
<!-- Child -->
<script>
export default {
  mounted () {
    this.$emit('onMounted')
  }
}
</script>

<!-- Parent -->
<template>
  <Child @onMounted="handleOnMounted" />
</template>

<!--可以改用 @hook:mount 在Vue内部系统中使用-->
<!-- Parent -->
<template>
  <Child @hook:mounted="handleOnMounted" />
</template>

```
#### 事件监听APIs

```vue
<!--在页面挂载时增加一个定时器，但销毁时需要清除定时器-->
export default {
data () {
return {
timer: null
}
},
mounted () {
this.timer = setInterval(() => {
console.log(Date.now())
}, 1000)
},
beforeDestroy () {
clearInterval(this.timer)
}
}
<!--变量越少，性能越好-->
```

使其只能在生命周期钩子内访问。使用 $once 来放弃不必要的东西


```vue
export default {
  mounted () {
    let timer = null
    timer = setInterval(() => {
      console.log(Date.now())
    }, 1000)
    this.$once('hook:beforeDestroy', () => {
      clearInterval(timer)
    })
  }
}

```