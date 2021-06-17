---
title: css奇技淫巧
categories: css
tags: [编程,学习]
---

在项目中总会遇到许多css问题，因此，总结几个常遇到的问题
- placeholder移动:`text-indent:3`
- 去除input框:`outline-style:none`
- background渐变色 :` background: linear-gradient(to bottom, #000000 0%,#ffffff 100%);`
- 旋转180度带过渡:`-webkit-transform: rotate(180deg);transition: All 0.4s ease-in-out;`
- 关于垂直居中`vertical-align`可以设置像素值
- 在组件化的样式中，在类似于‘曰’这种布局的时候，如何动态的把线加载父元素上？
```css
.parent{
    dispaly:flex;
    flex-direction:row;
    justify-content:space-around;
    box-sizing:border-box;
    &:after{
        content:'';
        display:block;
        width:100%;
        height:0;
        box-sizing:border-box;
        border-bottom:1px solid #ddd;
        position:relative;
        top:-208px;
    }
}
```