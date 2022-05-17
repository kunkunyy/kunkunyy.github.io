---
title: Vue 学习笔记（五）事件处理
date: 2022-02-05 21:36:25
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
---

# 事件处理

## 监听事件

* 可以用 `v-on` 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

```html
<div id="example-1">
    <button v-on:click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
</div>
```
```js
var example1 = new Vue({
    el: '#example-1',
    data: {
        counter: 0
    }
})
```

## 事件处理方法

* 在method中创建事件处理方法，然后使用`v-on` 绑定对应方法名。

```html
<div id="example-2">
    <!-- `greet` 是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
</div>
```
```js
var example2 = new Vue({
    el: '#example-2',
    data: {
        name: 'Vue.js'
    },
    // 在 `methods` 对象中定义方法
    methods: {
        greet: function (event) {
        // `this` 在方法里指向当前 Vue 实例
        alert('Hello ' + this.name + '!')
        // `event` 是原生 DOM 事件
        if (event) {
            alert(event.target.tagName)
        }
        }
    }
})
```

## 内联处理器中的方法

* 除了直接绑定到一个方法，也可以在内联 JavaScript 语句中调用方法。

```html
<div id="example-3">
    <button v-on:click="say('hi')">Say hi</button>
    <button v-on:click="say('what')">Say what</button>
</div>
```
```js
new Vue({
    el: '#example-3',
    methods: {
        say: function (message) {
        alert(message)
        }
    }
})
```

* 使用$event特殊变量可用于访问原始的DOM事件。

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
    Submit
</button>
```
```js
// ...
methods: {
    warn: function (message, event) {
        // 现在我们可以访问原生事件对象
        if (event) {
        event.preventDefault()
        }
        alert(message)
    }
}
```

## 事件修饰符

* Vue.js 为 `v-on` 提供了**事件修饰符**｡
* 修饰符是由点开头的指令后缀来表示的。主要有6个修饰符。
  * `.stop`
  * `.prevent`
  * `.capture`
  * `.self`
  * `.once`
  * `.passive`
* 使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

## 按键修饰符

* Vue 允许为 `v-on` 在**监听键盘事件**时添加按键修饰符。
* 以下代码只有在 'key'是 ‘Enter’时会调用’vm.submit ’方法。

```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

* 你可以直接将 `KeyboardEvent.key`暴露的任意有效按键名转换为 kebab-case 来作为修饰符。

```html
<input v-on:keyup.page-down="onPageDown">
```

## 系统修饰键

* 可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。
  * `.ctrl`、`.alt`、`.shift`、`.meta`。
* 修饰键与常规按键不同，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。
* 只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`。而单单释放 `ctrl` 也不会触发事件。
* 如果你想要这样的行为，请为 `ctrl` 换用 `keyCode`：`keyup.17`。

### .exact修饰符

* `.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

```html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button v-on:click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button v-on:click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button v-on:click.exact="onClick">A</button>
```

### 鼠标修饰符

* `.left`、`.right`、 `.middle`。
* 这些修饰符会限制处理函数仅响应特定的鼠标按钮。