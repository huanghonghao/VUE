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

*其他选项后面会讲*

写一个`VUE`应用，无非就是[**模板**](#模板语法)、[**指令**](#指令)

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

先看一个例子：

```html
<div id="app">
    <div>{{ message }}</div>
</div>
```

```js
const vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue'
        },
        // 实例创建
        beforeCreate() {
            console.log('beforeCreate...');
        },
        created() {
            // `this` 指向 vm 实例
            console.log('created...');
            console.log('通过this获取data: ' + this.message)
        },
        // 模板被编译挂载
        beforeMount() {
            console.log('beforeMount...');
        },
        mounted() {
            console.log('mounted...');
        },
        // data 被修改 虚拟DOM重新渲染
        beforeUpdate() {
            console.log('beforeUpdate...');
        },
        updated() {
            console.log('updated...');
        },
        // 解除数据绑定，销毁子组件和事件监听器，通过vm.$destroy()完成
        beforeDestroy() {
            console.log('beforeDestroy...');
        },
        destroyed() {
            console.log('destroyed...');
        }
    })
```

Console:

```js
beforeCreate...
created...
通过this获取data: Hello Vue
beforeMount...
mounted...
vm.message = 'Hello Vue!'
beforeUpdate...
updated...
"Hello Vue!"
vm.$destroy()
beforeDestroy...
destroyed...
vm.message = '23123'
```



>不要在选项属性或回调上使用[箭头函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如 `created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数并没有 `this`，`this` 会作为变量一直向上级词法作用域查找，直至找到为止，经常导致 `Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function` 之类的错误。



### 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。

如果你熟悉虚拟 DOM 并且偏爱 JavaScript 的原始力量，你也可以不用模板，[直接写渲染 (render) 函数](https://cn.vuejs.org/v2/guide/render-function.html)，使用可选的 JSX 语法。

#### 1、插值

* **文本**

  数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：

  ```html
  <span>Message: {{ msg }}</span>
  ```

  Mustache 标签将会被替代为对应数据对象上 `msg` 属性的值。无论何时，绑定的数据对象上 `msg` 属性发生了改变，插值处的内容都会更新。

  通过使用 [`v-once` 指令](https://cn.vuejs.org/v2/api/#v-once)，你也能执行**一次性**地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

  ```html
  <span v-once>这个将不会改变: {{ msg }}</span>
  ```

  

* **原生HTML**

  双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 [`v-html` 指令](https://cn.vuejs.org/v2/api/#v-html)：

  ```html
  <p>Using mustaches: {{ rawHtml }}</p>
  <p>Using v-html directive: <span v-html="rawHtml"></span></p>
  ```

  

* **Attribute**

  Mustache 语法不能作用在 HTML attribute 上，遇到这种情况应该使用 [`v-bind` 指令](https://cn.vuejs.org/v2/api/#v-bind)：

  ```html
  <div v-bind:id="dynamicId"></div>
  ```

  对于布尔 attribute (它们只要存在就意味着值为 `true`)，`v-bind` 工作起来略有不同，在这个例子中：

  ```html
  <button v-bind:disabled="isButtonDisabled">Button</button>
  ```

  如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` attribute 甚至不会被包含在渲染出来的 `<button>` 元素中。

  

* **JavaScript 表达式**

  ```html
  {{ number + 1 }}
  
  {{ ok ? 'YES' : 'NO' }}
  
  {{ message.split('').reverse().join('') }}
  
  <div v-bind:id="'list-' + id"></div>
  ```

  这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含**单个表达式**，所以下面的例子都**不会**生效。

  ```html
  <!-- 这是语句，不是表达式 -->
  {{ var a = 1 }}
  
  <!-- 流控制也不会生效，请使用三元表达式 -->
  {{ if (ok) { return message } }}
  ```

  > 模板表达式都被放在沙盒中，只能访问[全局变量的一个白名单](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9)，如 `Math` 和 `Date` 。你不应该在模板表达式中试图访问用户定义的全局变量。



#### 2、指令

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。回顾我们在介绍中看到的例子：

```html
<p v-if="seen">现在你看到我了</p>
```

这里，`v-if` 指令将根据表达式 `seen` 的值的真假来插入/移除 `<p>` 元素。

* **参数**

  一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

  ```html
  <a v-bind:href="url">百度</a>
  ```

  在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` attribute 与表达式 `url` 的值绑定。

  另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

  ```html
  <a v-on:click="doSomething">...</a>
  ```

  

* **动态参数**

  从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数：

  ```html
  <!--
  注意，参数表达式的写法存在一些约束，如之后的“对动态参数表达式的约束”章节所述。
  -->
  <a v-bind:[attributzename]="url"> ... </a>
  ```

  这里的 `attributzename` 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。例如，如果你的 Vue 实例有一个 `data` 属性 `attributzename`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`。

  > 要注意动态参数的大小写问题，HTML解析器是不区分大小写的，如`attributeName`会被解析为`attributzename`

  同样地，你可以使用动态参数为一个动态的事件名绑定处理函数：

  ```html
  <input type="text" v-on:[eventname]="action" />
  ```

  

* **修饰符**

  修饰符 (modifier) 是以半角句号 `.` 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，`.prevent` 修饰符告诉 `v-on` 指令对于触发的事件调用 `event.preventDefault()`：

  ```html
  <form v-on:submit.prevent="onSubmit">...</form>
  ```

  在接下来对 [`v-on`](https://cn.vuejs.org/v2/guide/events.html#%E4%BA%8B%E4%BB%B6%E4%BF%AE%E9%A5%B0%E7%AC%A6) 和 [`v-for`](https://cn.vuejs.org/v2/guide/forms.html#%E4%BF%AE%E9%A5%B0%E7%AC%A6) 等功能的探索中，你会看到修饰符的其它例子。

#### 3、缩写

`v-` 前缀作为一种视觉提示，用来识别模板中 Vue 特定的 attribute。当你在使用 Vue.js 为现有标签添加动态行为 (dynamic behavior) 时，`v-` 前缀很有帮助，然而，对于一些频繁用到的指令来说，就会感到使用繁琐。同时，在构建由 Vue 管理所有模板的[单页面应用程序 (SPA - single page application)](https://en.wikipedia.org/wiki/Single-page_application) 时，`v-` 前缀也变得没那么重要了。因此，Vue 为 `v-bind` 和 `v-on` 这两个最常用的指令，提供了特定简写：

* **`v-bind` 缩写**

  ```html
  <!-- 完整语法 -->
  <a v-bind:href="url">...</a>
  
  <!-- 缩写 -->
  <a :href="url">...</a>
  
  <!-- 动态参数的缩写 (2.6.0+) -->
  <a :[key]="url"> ... </a>
  ```

* **`v-on`缩写**

  ```html
  <!-- 完整语法 -->
  <a v-on:click="doSomething">...</a>
  
  <!-- 缩写 -->
  <a @click="doSomething">...</a>
  
  <!-- 动态参数的缩写 (2.6.0+) -->
  <a @[event]="doSomething"> ... </a>
  ```



### 计算属性和侦听器

#### 1、计算属性

模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 `message` 的翻转字符串。当你想要在模板中多次引用此处的翻转字符串时，就会更加难以处理。

```html
<div id="example1">
  {{ message.split('').reverse().join('') }}
</div>
<div id="example2">
  {{ message.split('').reverse().join('') }}
</div>
<div id="example3">
  {{ message.split('').reverse().join('') }}
</div>
<div id="example4">
  {{ message.split('').reverse().join('') }}
</div>
```

所以，对于任何复杂逻辑，你都应当使用**计算属性**。



先看第一个例子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>插值</title>
</head>
<body>
    <div id="app">
        <div>我是没有翻转的消息：{{ message }}</div>
        <div>我是翻转后的消息：{{ reversedMessage }}</div>
    </div>
</body>
<script src="vue.js"></script>
<script>
    const vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue!',
        },
        computed: {
            reversedMessage() {
                return this.message.split('').reverse().join('')
            }
        }
    });
</script>
</html>
```

这里我们声明了一个计算属性 `reversedMessage`。我们提供的函数将用作 property `vm.reversedMessage` 的 getter 函数：

```js
vm.message = 'Goodbye'
```

你可以打开浏览器的控制台，自行修改例子中的 vm。`vm.reversedMessage` 的值始终取决于 `vm.message` 的值。

你可以像绑定普通 property 一样在模板中绑定计算属性。Vue 知道 `vm.reversedMessage` 依赖于 `vm.message`，因此当 `vm.message` 发生改变时，所有依赖 `vm.reversedMessage` 的绑定也会更新。



#### 2、计算属性缓存和方法

计算属性的另一种实现方式，使用**方法**：

```html
<div id="app2">
    <div>我是没有翻转的消息：{{ message }}</div>
    <div>我是翻转后的消息：{{ reversedMessage() }}</div>
</div>
```

```js
const vm2 = new Vue({
        el: '#app2',
        data: {
            message: 'Hello Vue!',
        },
        methods: {
            reversedMessage() {
                // `this` 指向 vm 实例
                return this.message.split('').reverse().join('')
            },
        }
    });
```

我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。



#### 3、计算属性和侦听属性



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