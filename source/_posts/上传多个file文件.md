---
title: 清空上传内容
date: 2018-4-28
categories: js
tags: [编程,学习]
---

如果要上传同一个文件。例如
`<input @change="fileChange(lesson, $event.target,index)" type="file" accept=".jpg,.png,.JPG,.PNG,.gif,.GIF,.xls,.xlsx,.ppt,.pptx,.doc,.docx,.txt,.pdf,.jpeg,.bmp,.XLS,.XLSX,.PPT,.PPTX,.DOC,.DOCX,.TXT,.PDF,.JPEG,.BMP">
`
```javascript
 fileChange: function (course, input, index) {
        let fileObj = input.files[0];
        var file = {
          prepare: false,//是否可预览
          name: fileObj.name,
          size: fileObj.size,
          previewFlag: false,
          process: true
        };
        course.files.push(file);
                  input.value = ""

        }
```
那么在js里只需要一行代码
`input.value = ""`
