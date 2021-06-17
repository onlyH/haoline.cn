---
title: vue上传文件
date: 2018-2-28
categories: vue
tags: [编程,学习]
---

例如点击标签`<span @click = "addLesson"></span>`上传文件
```html
<input @change="fileChange(lesson,$event.target)" type="file">
```


```javascript  
addLesson:function() {
    this.lessons.push({
        name:'',
        files:[],
        editing:true //需要隐藏的地方/显示的地方
    })
} 

fileChange:function(course,input) {
    let fileObj = input.file[0]
    let file = {
        previewFlag:false, //是否可预览
        name:fileObj.name,
        process:0,
        size:fileObj.size
    };
    course.files.push(file)
}
```