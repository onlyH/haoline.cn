---
title: 上传文件报错
date: 2017-05-01
categories: hexo
tags: [编程,学习]
---

今日上传代码，遇到一个坑，
`hexo h`报错：
```
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 5, Column 20]
  expected variable end
    at Object._prettifyError (/Users/shuan/Desktop/blog/blog/node_modules/nunjucks/src/lib.js:36:11)
    at Template.render (/Users/shuan/Desktop/blog/blog/node_modules/nunjucks/src/environment.js:524:21)
    at Environment.renderString (/Users/shuan/Desktop/blog/blog/node_modules/nunjucks/src/environment.js:362:17)
    at Promise (/Users/shuan/Desktop/blog/blog/node_modules/hexo/lib/extend/tag.js:66:9)
    at Promise._execute (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/debuggability.js:313:9)
    at Promise._resolveFromExecutor (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/promise.js:483:18)
    at new Promise (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/promise.js:79:10)
    at Tag.render (/Users/shuan/Desktop/blog/blog/node_modules/hexo/lib/extend/tag.js:64:10)
    at Object.tagFilter [as onRenderEnd] (/Users/shuan/Desktop/blog/blog/node_modules/hexo/lib/hexo/post.js:230:16)
    at Promise.then.then.result (/Users/shuan/Desktop/blog/blog/node_modules/hexo/lib/hexo/render.js:65:19)
    at tryCatcher (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/promise.js:512:31)
    at Promise._settlePromise (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/promise.js:569:18)
    at Promise._settlePromise0 (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/promise.js:614:10)
    at Promise._settlePromises (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/promise.js:694:18)
    at _drainQueueStep (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/async.js:138:12)
    at _drainQueue (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/async.js:131:9)
    at Async._drainQueues (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/async.js:147:5)
    at Immediate.Async.drainQueues [as _onImmediate] (/Users/shuan/Desktop/blog/blog/node_modules/bluebird/js/release/async.js:17:14)
    at runCallback (timers.js:794:20)
    at tryOnImmediate (timers.js:752:5)
    at processImmediate [as _immediateCallback] (timers.js:729:5)
```


可能是标记异常，想到今天写的一篇关于vue的文档，赶紧回原文看了看。
原来是文章中使用了大括号 { } 这个特殊字符,且没有转义导致编译不通过
> Template render error 模板渲染错误
> 解决方案：将 { } 的大括号通过`&#123;`和` &#125;` 进行转换