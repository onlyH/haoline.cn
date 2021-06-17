---
title: html组件
categories: html
tags: [语言,理解]
---

今日读了掘金上面的一篇文章，讲的是html组件。
===
#### 四大 Web 组件标准
四大 Web 组件标准分别为：HTML Template、Shadow DOM、Custom Elements 和 HTML Imports。实际上其中一个已经被废弃了，所以变成“三大”了。

HTML Template,简单的讲也就是 HTML5 中的` <template>` 标签，正常情况下它无色无味，感知不到它的存在，甚至它下面的 img 都不会被下载，script 都不会被执行。<template> 就如它的名字一样，它只是一个模版，只有到你用到它时，它才会变得有意义。


Shadow DOM 则是原生组件封装的基本工具，它可以实现组件与组件之间的独立性。
Custom Elements 是用来包装原生组件的容器，通过它，你就只需要写一个标签，就能得到一个完整的组件。


HTML Imports 则是 HTML 中类似于 ES6 Module 的一个东西，你可以直接 import 另一个 html 文件，然后使用其中的 DOM 节点。但是，由于 HTML Imports 和 ES6 Module 实在是太像了，并且除了 Chrome 以外没有浏览器愿意实现它，所以它已经被废弃并不推荐使用了。未来会使用 ES6 Module 来取代它，但是现在貌似还没有取代的方案，在新版的 Chrome 中这个功能已经被删除了，并且在使用的时候会在 Console 中给出警告。警告中说使用 ES Modules 来取代，但是我测试在 Chrome 71 中 ES Module 会强制检测文件的 MIME 类型必须为 JavaScript 类型，应该是暂时还没有实现支持。


![](https://user-gold-cdn.xitu.io/2018/10/18/16684f2ad0409535?imageslim)

##### Shadow DOM
DOM，在 HTML 中作为一个最基础的骨架而存在，它是一个树结构，树上的每一个节点都是 HTML 中的一部分。DOM 作为一棵树，它拥有着上下级的层级关系，我们通常使用“父节点”、“子节点”、“兄弟节点”等来进行描述（当然有人觉得这些称谓强调性别，所以也创造了一些性别无关的称谓）。子节点在一定程度上会继承父节点的一些东西，也会因兄弟节点而产生一定的影响，比较明显的是在应用 CSS Style 的时候，子节点会从父节点那里继承一些样式。


而 Shadow DOM，也是 DOM 的一种，所以它也是一颗树，只不过它是长在 DOM 树上的一棵特殊的子树。


Shadow DOM 的特别之处就在于它致力于创建一个相对独立的一个空间，虽然也是长在 DOM 树上的，但是它的环境却是与外界隔离的，当然这个隔离是相对的，在这个隔离空间中，你可以选择性地从 DOM 树上的父节点继承一些属性，甚至是继承一棵 DOM 树进来。
利用 Shadow DOM 的隔离性，我们就可以创造原生的 HTML 组件了。
实际上，浏览器已经通过 Shadow DOM 实现了一些组件了，只是我们使用过却没有察觉而已，这也是 Shadow DOM 封装的组件的魅力所在：你只管写一个 HTML 标签，其他的交给我。（是不是有点像 React 的 JSX 啊？）
```html
<video controls src="./video.mp4" width="400" height="300"></video>
```
![](https://user-gold-cdn.xitu.io/2018/10/18/16684f31a0ba2ada?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
![](https://user-gold-cdn.xitu.io/2018/10/18/16684f3402275323?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
> 注：浏览器默认隐藏自身的 Shadow DOM 实现，但如果是用户通过脚本创造的 Shadow DOM，是不会被隐藏的。
![](https://user-gold-cdn.xitu.io/2018/10/18/16684f3694275a9b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)
shadow DOM 中的节点大多都有 pseudo 属性，根据这个属性，你就可以在外面编写 CSS 样式来控制对应的节点样式了。比如，将上面这个 
pseudo="-webkit-media-controls-overlay-play-button" 的 input 按钮的背景色改为橙色：
```css
video::-webkit-media-controls-overlay-play-button {
  background-color: orange;
}
```
![](https://user-gold-cdn.xitu.io/2018/10/18/16684f3b4eabacd8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)


由于 Shadow DOM 实际上也是 DOM 的一种，所以在 Shadow DOM 中还可以继续嵌套 Shadow DOM，就像上面那样。


浏览器中还有很多 Element 都使用了 Shadow DOM 的形式进行封装，比如 `<input>`、`<select>`、`<audio>` 等，这里就不一一展示了。
由于 Shadow DOM 的隔离性，所以即便是你在外面写了个样式：`div { background-color: red !important; }`，Shadow DOM 内部的 div 也不会受到任何影响。


也就是说，写样式的时候，该用 id 的时候就用 id，该用 class 的时候就用 class，一个按钮的 class 应该写成 .button 就写成 .button。完全不用考虑当前组件中的 id、class 可能会与其他组件冲突，你只要确保一个组件内部不冲突就好——这很容易做到。


这解决了现在绝大多数的组件化框架都面临的问题：Element 的 class(className) 到底怎么写？用前缀命名空间的形式会导致 class 名太长，像这样：`.header-nav-list-sublist-button-icon`；而使用一些 CSS-in-JS 工具，可以创造一些唯一的 class 名称，像这样：`.Nav__welcomeWrapper___lKXTg`，这样的名称仍旧有点长，还带了冗余信息。


##### ShadowRoot
ShadowRoot 是 Shadow DOM 下面的根，你可以把它当做 DOM 中的 <body> 一样看待，但是它不是 <body>，所以你不能使用 <body> 上的一些属性，甚至它不是一个节点。


你可以通过 ShadowRoot 下面的 appendChild、querySelectorAll 之类的属性或方法去操作整个 Shadow DOM 树。


对于一个普通的 Element，比如 `<div>`，你可以通过调用它上面的 attachShadow 方法来创建一个 ShadowRoot（还有一个 createShadowRoot 方法，已经过时不推荐使用），attachShadow 接受一个对象进行初始化：`{ mode: 'open' }`，这个对象有一个 mode 属性，它有两个取值：'open' 和 'closed'，这个属性是在创造 ShadowRoot 的时候需要初始化提供的，并在创建 ShadowRoot 之后成为一个只读属性。
mode: 'open' 和 mode: 'closed' 有什么区别呢？在调用 attachShadow 创建 ShadowRoot 之后，attachShdow 方法会返回 ShadowRoot 对象实例，你可以通过这个返回值去构造整个 Shadow DOM。当 mode 为 'open' 时，在用于创建 ShadowRoot 的外部普通节点（比如` <div>`）上，会有一个 shadowRoot 属性，这个属性也就是创造出来的那个 ShadowRoot，也就是说，在创建 ShadowRoot 之后，还是可以在任何地方通过这个属性再得到 ShadowRoot，继续对其进行改造；而当 mode 为 'closed' 时，你将不能再得到这个属性，这个属性会被设置为 null，也就是说，你只能在 attachShadow 之后得到 ShadowRoot 对象，用于构造整个 Shadow DOM，一旦你失去对这个对象的引用，你就无法再对 Shadow DOM 进行改造了。


可以从上面 Shadow DOM 的截图中看到 #shadow-root (user-agent) 的字样，这就是 ShadowRoot 对象了，而括号中的 user-agent 表示这是浏览器内部实现的 Shadow DOM，如果使用通过脚本自己创建的 ShadowRoot，括号中会显示为 open 或 closed 表示 Shadow DOM 的 mode。

![](https://user-gold-cdn.xitu.io/2018/10/18/16684f3f3702ee88?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

> 浏览器内部实现的 user-agent 的 mode 为 closed，所以你不能通过节点的 ShadowRoot 属性去获得其 ShadowRoot 对象，也就意味着你不能通过脚本对这些浏览器内部实现的 Shadow DOM 进行改造。
















[链接](https://juejin.im/post/5bc7ead7f265da0afc2c2c6b)
