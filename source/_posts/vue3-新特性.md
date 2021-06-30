---
title: vue3-新特性
date: 2021-06-18 14:27:55
categories: 技术
tags: [编程,感悟]
keywords: vue3
created: 创建于
modified: 更新于
sticky: 置顶
posted: Posted on #发表于
visitors: Visitors #阅读次数
in: In #分类于
read_more: 阅读全文
untitled: 未命名
toc_empty: 此文章未包含目录
wordcount: 字数统计
min2read: 阅读时长
totalcount: Site words total count
copyright: true
---
#### 性能提升
- 打包大小减少41%
- 初次渲染快55%，133%
- 内存使用减少54%

#### Composition API
- ref和reactive
- computed和watch
- 新的生命周期函数
- 自定义函数-Hooks函数

#### 新增特性
- Teleport 瞬移组件的位置
- Suspense 异步加载组件的新福音
- 全局API的修改和优化
- 更多的试验性特性

#### 更好的ts支持



#### 为什么要有vue3（解决现有存在的棘手问题）
- 随着功能的增长，复杂组件的代码变得难以维护

- Mixin的缺点 
1. 明明冲突
2. 不清楚暴露出来的变量的作用
3. 重用到其他component经常遇到问题


- setup中无法访问this

- 新生命周期

![生命周期](./vue3-新特性/生命周期.png)