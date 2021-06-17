---
title: js日常使用
categories: js
tags: [编程,学习]
---
```javascript
var text = 'purple haze'
text['length']
text.length


//对象所扮演的主要角色实际上是一个属性的集合

var cat = {
    color:'gray',
    name:'spot',
    size:46
}
cat.size; //46
delete cat.size;
cat.size;//undefind

var empty = {}
empty.notReally = 1000
empty;//{notReally: 1000}

var thing = {
    'gabba':'hey',
    5:10
}
thing['5'] //10
thing[2+3] //10

//中括号会将表达式转化为字符串来判断是否有该属性的名称。
//也可以把变量当成属性名称
var propertyName = 'length'
var text = 'coco'
text[propertyName] //4

//操作符in可以用来判断一个对象是否有某个属性，产生的是布尔值
var chineseBox = {}
chineseBox.content = chineseBox;
'content' in chineseBox //true
debugger
'content' in chineseBox.content//true

//对象即集合
var set = {'spot':true}
set['white'] =true;

delete set['spot']
'aa' in set;//false

//相同对象的两个引用和包含相同属性的两个不同对象是有区别的。
var object1 = {value:10}
var object2 = object1
var object3 = {value:10}
debugger;
object1 == object2 //true
object1 == object3 //false

/* 
object1和object2是两个变量，抓取的是相同的值，这里只有一个实际对象，
因此修改了object1的值，同时也改变了object2的值，
object3指向的是另外一个对象，默认和object1有相同的属性，但各自单独运行

比较对象时，js中的==操作符只有在赋予的两个值都完全相同时才能返回true，
比较两个内容相同的不同对象将返回false
*/


//对象即集合

var arr = ['lele','tom','jack','shuan']
for(var i of arr) {
    console.log(`name:${i}`)
}

function range(item) {
    var arr = []
    for(var i = 0; i<=item; i++) {
        arr.push(i)
    }
    return arr
}
range(4) //01234

// split()将一个字符串分解成一个数组

var words = 'this is word'
words.split(' ') //空格！！！



//  如何测试一个段落是否以某个特定单词开头
// charAt()--->用于从某个字符串中获取指定的字符
var cat = 'born 15-11-2003'
cat.charAt(0) =='b' && cat.charAt(1) == 'o' && cat.charAt(2) =='r' && cat.charAt(3) == 'n'//true


cat.slice(0,4) =='born'//true

function startsWidth(str,comp) {
    return str.slice(0,comp.length) == comp
}

//如果指定的位置没有字符，charAt将返回空字符，而slice则只是将不存在的内容忽略掉。


//indexOf可以找出字符串第一次出现的位置或者截取字符串中的子串
//如果slice只是一个参数，他将返回从指定位置一直到字符串结束位置之间的字符串


function catName(paragraph) {
    var colon = paragraph.indexOf(":");
    return paragraph.slice(colon+2).split(", ")
}
```



