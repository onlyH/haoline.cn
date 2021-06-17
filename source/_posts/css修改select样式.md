---
title: css修改select样式
categories: css
tags: [编程,学习]
---






select是有默认样式的，select的一些默认样式我们很难修改，比如图标的替换。
参考了一些别人的例子，整理下，如果要切换图片。
```html
 <select>
        <option value="1">1</option>
        <option value="2">2</option>
    </select>
```

```css
 select {
  border: none;
  /*将默认的select选择框样式清除*/
  appearance: none;
  -moz-appearance: none;
  -webkit-appearance: none;
  /*在选择框的最右侧中间显示小箭头图片*/
  background: url("http://ourjs.github.io/static/2015/arrow.png") no-repeat scroll right center transparent !important;
  /*为下拉小箭头留出一点位置，避免被文字覆盖*/
  padding-right: 14px;
  outline: none;
}
```
