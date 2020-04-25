[TOC]

# VUE入门

 ## 一、初识VUE

### VUE.js 是什么？

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**[渐进式](#渐进式)框架、[MVVM](#MVVM)框架**。与其它大型框架不同的是，Vue 被设计为可以**自底向上逐层应用**。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。

> 学习本教程HTML、CSS 和 JavaScript 知识
>

### 葵花宝典

* 官方中文文档：<https://cn.vuejs.org/v2/guide/>
* 类库：<https://github.com/vuejs/awesome-vue#libraries--plugins>
* Github：<https://github.com/vuejs/vue>



### 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

![components](.\components.png)

### 兼容性

Vue **不支持** IE8 及以下版本，因为 Vue 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有[兼容 ECMAScript 5 的浏览器](https://caniuse.com/#feat=es5)。



## 二、安装

### Vue Devtools

在使用 Vue 时，我们推荐在你的浏览器上安装 [Vue Devtools](https://github.com/vuejs/vue-devtools#vue-devtools)。它允许你在一个更友好的界面中审查和调试 Vue 应用。



### 使用 VUE 的几种方式

#### 1、直接使用`<script>`引入

直接下载并用 `<script>` 标签引入，`Vue` 会被注册为一个全局变量。

> 有两个版本
>
> * 开发版：包含完整的警告和调试模式
> * 生产版：删除了警告，33.30KB min+gzip

* 使用CDN链接

```js
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

对于生产环境，我们推荐链接到一个明确的版本号和构建文件，以避免新版本造成的不可预期的破坏：

```js
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>
```

如果你使用原生 ES Modules，这里也有一个兼容 ES Module 的构建文件：

```js
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.11/dist/vue.esm.browser.js'
</script>
```



#### 2、NPM

在用 Vue 构建大型应用时推荐使用 NPM 安装。NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。同时 Vue 也提供配套工具来开发[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)。

```shell
# 最新稳定版
npm install vue
```



#### 3、命令行工具（CLI）

Vue 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为单页面应用 (SPA) 快速搭建繁杂的脚手架。

> 命令行工具依赖Node.js



## 三、VUE 基础

### 第一个`VUE`实例

每个 Vue 应用都是通过用 `Vue` 函数创建一个新的 **Vue 实例**开始的：

```js
const vm = new Vue({
  // 选项
})
```

选项列表：<https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE>

下面是一个简单的例子：

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
    <!-- 把只绑定到innerHTML -->
    <div>{{ message }}</div>
    <!-- 把值绑定到属性 -->
    <div v-bind:title="title">
        鼠标悬停几秒钟查看此处动态绑定的提示信息！
    </div>
    <div :title="title">
        鼠标悬停几秒钟查看此处动态绑定的提示信息！（我是语法糖的简写）
    </div>

    <div>{{ sex }}</div>
    <button v-on:click="sex = '女孩子'">Change 写法一</button>
    <button @click="sex = 'Gay'">Change 写法二</button>
</div>
</body>
<script src="vue.js"></script>
<script>
    const app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!',
            title: '页面加载于 ' + new Date().toLocaleString(),
            sex: '男孩子'
        }
    })
</script>
</html>
```

除了数据属性，Vue 实例还暴露了一些有用的实例属性与方法。它们都有前缀 `$`，以便与用户定义的属性区分开来。例如：

```js
const data = {
    message: 'Hello Vue!',
    title: '页面加载于 ' + new Date().toLocaleString(),
    sex: '男孩子'
};
const app = new Vue({
    el: '#app',
    data: data
})
app.$data === data // => true
app.$el === document.getElementById('app') // => true

// $watch 是一个实例方法
app.$watch('sex', function (newValue, oldValue) {
  // 这个回调将在 `app.sex` 改变后调用
})
```



### 生命周期钩子

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

<img src=".\lifecycle.png" width="100%" />

>不要在选项属性或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。

## 专业术语

### 声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<div id="app">
  {{ message }}
</div>
```

```js
const app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```



```html
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

 `v-bind` attribute 被称为**指令**。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊 attribute。可能你已经猜到了，它们会在渲染的 DOM 上应用特殊的响应式行为。在这里，该指令的意思是：“将这个元素节点的 `title` attribute 和 Vue 实例的 `message` 属性保持一致”。

如果你再次打开浏览器的 JavaScript 控制台，输入 `app2.message = '新消息'`，就会再一次看到这个绑定了 `title` attribute 的 HTML 已经进行了更新。



### 渐进式

可用可不用，可以一点点地把**VUE**集成到项目中去。就是用你想用或者能用的功能特性，你不想用的部分功能可以先不用。VUE不强求你一次性接受并使用它的全部功能特性。

附上一个对渐进式理解的链接：<https://www.zhihu.com/question/51907207>



### MVVM

[MVVM](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel)是Model-View-ViewModel的缩写。它本质上就是MVC 的改进版。MVVM 就是将其中的View 的状态和行为抽象化，让我们将视图 UI 和业务逻辑分开。当然这些事 ViewModel 已经帮我们做了，它可以取出 Model 的数据同时帮忙处理 View 中由于需要展示内容而涉及的业务逻辑。

![5d9e8b19e4b04913a12b1a64](.\5d9e8b19e4b04913a12b1a64.png)

深入理解资料：<https://www.liaoxuefeng.com/wiki/1022910821149312/1108898947791072#0>