---
title: 富文本-select改进
categories: vue
tags: [编程,功能]
---
之前在项目中，做了一个富文本编辑器，第一次用的是原生js实现，第二次用vue实现功能，在select里面定义可以选择字体的大小，当时采取的是@input去实现，无聊时读了下vue文档，发现其实。。是可以改进的，比如说v-model是一个语法糖，文档是这样说的：
- 自定义事件也可以用于创建支持 v-model 的自定义输入组件
`input v-nodel=searchText`
等价于：
```
<input v-bind:value='searchText' v-on:input='$event.target.value'>
```
而我当时的实现就有一些复杂
```javascript
 <select name="fontSize" class="fontSize" @change="showSize($event)">
    <option :value="ops.value"
            v-for="ops in fontSize"
            :selected="ops.value == 16 ? true: '' ">{{ops.value}}</option>
 </select>

showSize: function (ev) {
    var execFontSize = function (size, unit) {
    var spanString = $('<span/>', {
        'text': document.getSelection()
    }).css(
        {'font-size': size + unit, 'color': `#${this.designColor}`}).prop('outerHTML');
    document.execCommand('insertHTML', false, spanString);
    };
    const value = ev.target.value;
    execFontSize(value, 'px')

}
```
嗯嗯，改起来~~