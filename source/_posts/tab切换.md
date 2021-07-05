---
title: tab切换
categories: js
date: 2018-04-02 15:33:03
tags: [编程,功能]
---
使用javascript和jquery编写tab切换,使用jquery编写列表切换
```javascript
// javascript
function $(id) {
    return typeof id == 'string' ? document.getElementById(id) : id
}
window.onload = function () {
    var titleName = $('tab-title').getelementsByTagName('li')
    var tabContent = $('tab-content').getelementsByTagName('div')
    if (titleName.length != tabContent.length) {
        return
    }
    for (var i = 0; i < titleName.length; i++) {
        titleName[i].id = i;
        titleName[i].onmouseover = function () {
            for (var j = 0; j < titleName.length; j++) {
                titleName[j].className = ''
                titleName[j].style.display = 'none'
            }
            this.className = 'select'
            tabContent[this.id].style.display = 'block'
        }
    }
}

```

```javascript   
//jquery方法添加个定时器
var timer;
$(function () {
    $('#tabfirst li').each(function (index) {
        var liNode = $(this)
        $(this).mouseover(function () {
            timer = setTimeout(() => {
                $('div.content').removeClass('content')
                $('#tabfirst li.tabin').removeClass('tabin')
                $('div').eq(index).addClass('content')
                liNode.addClass('tabin')
            }, 300);
        }).mouseout(function () {
            clearTimeout(timer)
        })
    })
})()
```

```javascript   
$(function () {
    $('.list-1').bind('click', function () {
        $('.list-1').css('backgroundPosition', '0px -26px')
        $('.list-2').css('backgroundPosition', '-30px -26px')
        $('changeList').children().removeClass('list-1-0').addClass('list-2-v')

    })
    $('.list-2').bind('click', function () {
        $('.list-1').css('backgroundPosition', '0px 0px')
        $('.list-2').css('backgroundPosition', '-30px 0px')
        $('changeList').children().removeClass('list-2-v').addClass('list-1-0')

    })
})()
```