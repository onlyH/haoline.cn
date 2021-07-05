---
title: 在项目中使用二次元（live2d）
date: 2021-07-01 15:02:41
tags: []
---
<blockquote class="blockquote-center">
<image src="https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlLzk4YjQzNTJhZ3kxZzI5NGVocG96aWoyMDVhMDVuZ2xrLmpwZw?x-oss-process=image/format,png"></image>
了解live2d使用原理
</blockquote>

<!-- more -->



```shell
npm install --save hexo-helper-live2d
npm install live2d-widget-model-hijiki #这里可以改成你想要的动画资源

```
![](https://imgconvert.csdnimg.cn/aHR0cDovL3d3MS5zaW5haW1nLmNuL2xhcmdlLzk4YjQzNTJhZ3kxZzI5NThmMGd5cGoyMGloMGhpYWFuLmpwZw?x-oss-process=image/format,png)

找到并拷贝`node_modules\live2d-widget-model-hijiki`文件夹

在博客的根目录下新建一个文件夹（live2d_modules），将之前拷贝的内容放进去

找到根目录的配置文件`_config.xml`
```
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-hijiki
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
```

运行 `hexo clean && hexo g && hexo d`


 编译完后 public 目录会多出来一个live2dw,index.html

 打开HTML文件，拉到最下面，复制js代码段

```javascript
<script src="/live2dw/lib/L2Dwidget.min.js?094cbace49a39548bed64abff5988b05"></script>
<script>L2Dwidget.init({"pluginRootPath":"live2dw/","pluginJsPath":"lib/","pluginModelPath":"assets/","tagMode":false,"debug":false,"model":{"jsonPath":"/live2dw/assets/live2d-widget-model-hijiki.model.json"},"display":{"position":"right","width":150,"height":300},"mobile":{"show":true},"log":false});
</script>
```

把复制出来的 2 段script标签和上图中 live2dw文件夹，拷到自己的项目中

#### 要注意资源路径的存放方式，避免因为资源文件找不到一直报错

只要把资源文件放好，JS引入正确，方法调用没问题。那就是已经成功了

>其实通过 hexo 编译只是为了拿到 /live2dw/lib/L2Dwidget.min.js 这段核心JS文件,在 init 方法中的一些配置，其实都是对应的路径。比如 lib 的路径 assets(动画资源的路径)。只要细心一点，观察一下目录的路径不同。就可以在项目中通过修改配置使用全部的动画了！

### 在vue中的使用

将live2dw放入public文件夹中

在index.html文件中引用`<script src="/live2dw/lib/L2Dwidget.min.js"></script>`

使用：
```js
 created () {
    setTimeout(() => {
      window.L2Dwidget.init({
        pluginRootPath: 'live2dw/',
        pluginJsPath: 'lib/',
        pluginModelPath: 'live2d-widget-model-hijiki/assets/',
        tagMode: false,
        debug: false,
        model: { jsonPath: '/live2dw/live2d-widget-model-hijiki/assets/hijiki.model.json' },
        display: { position: 'right', width: 150, height: 300 },
        mobile: { show: true },
        log: false
      })
    }, 3000)
  }
```



