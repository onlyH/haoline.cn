---
title: vue-slot
date: 2017-05-01
categories: vue
tags: [编程,学习]
---

```javascript
//goods.vue
<span slot="A">啦啦啦</span>
<span slot="B">嘿嘿嘿</span>

//list.vue
<slot name="A"></slot>
<slot name="B"></slot>
```