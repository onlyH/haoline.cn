---
title: HTML-autocomplete
date: 2017-05-01
categories: html
tags: [编程]
---
项目中，手撸搜索框，从后台拿数据就行展示，选择操作等。
偶然发现，搜索后，浏览器会有缓存。就是我的搜索会展示在页面上。
于是。。查资料。。关于input的。。有这么一条属性`autocomplete = off | no`
~~~
定义和用法
autocomplete 属性规定表单是否应该启用自动完成功能。

自动完成允许浏览器预测对字段的输入。当用户在字段开始键入时，浏览器基于之前键入过的值，应该显示出在字段中填写的选项。

注释：autocomplete 属性适用于 <form>，以及下面的 <input> 类型：text, search, url, telephone, email, password, datepickers, range 以及 color。

提示：在某些浏览器中，您可能需要手动启用自动完成功能。
~~~
所以，我之所以有搜索记录存在，是因为浏览器会对同一个name标记的输入进行缓存，设置`off`瞬间解决掉问题。