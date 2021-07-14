---
title: js日常使用
date: 2021-07-14 10:39:40
categories: js
tags: [编程,学习]
---
<blockquote class="blockquote-center">
日常总结
</blockquote>
<!-- more -->

#### 简述 instanceof 原理
<details>
<summary>展开查看</summary>
如果 A 沿着原型链中可以找到B.prototype 则 A instanceof B 为 true

解释：遍历 A 的 原型链，如果可以找到 B，那么为 true 否则为 false
```javascript
const instance = (A,B) => {
    let p = A
    while (p) {
        if(p == B.prototype) {
            return true
        }
        p = p.__proto__
    }
    return false
}
```
</details>

#### 如何判断链表是否有环
<details>
<summary>展开查看</summary>
两个指针,一个快，一个慢，遍历链表，如果两个指针可以相逢，则有环

```javascript
function list(head) {
    let p = head
    let p2 = head
    while (p && p2 && p3.next) {
        if(p == p2) {
            return true
        }
        p = p.next
    }
    return false
}
```
[参考leetCode](https://leetcode-cn.com/problems/linked-list-cycle/submissions/)
</details>


#### 集合是什么
<details>
<summary>展开查看</summary>
一种<em style="color: #0d95e8">无序且唯一</em>的数据结构

ES6 中有集合，名为 Set

集合的常用操作：去重，判断某元素是否在集合中，求交集
```javascript
var a = [1,1,2,2,2]
var d = [...new Set(a)]
var e =  Array.from(new Set(a))
```
 判断元素是否在集合中
```javascript
const set = new Set(arr)
const has = set.has(1)
```

求交集
```javascript
const set2 = new Set([2,3])
const set3 = new Set([...set].filter(item=>set2.has(item)))
```

</details>

#### 字典是什么
<details>
<summary>展开查看</summary>
与集合类似，字典也是一种存储唯一值的数据结构，但它是以<em style="color: #0d95e8">键值对</em>的形式来存储

</details>