---
title: Vue 学习笔记（四）列表渲染
date: 2022-02-04 23:14:59
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
---

# 列表渲染

## 用`v-for`把一个数组对应为一组元素

* 可以用`v-for`指令基于一个数组来渲染一个列表。
  * `v-for`指令需要使用 `item in items` 形式的特殊语法；
  * `items`是源数据数组，`item`是被迭代数组的别名。

```html
<ul id="example-1">
    <li v-for="item in items" :key="item.message">
        {{ item.message }}
    </li>
</ul>
```
```js
var example1 = new Vue({
    el: '#example-1',
    data: {
        items: [
            { message: 'Foo' },
            { message: 'Bar' }
        ]
    }
})
```

* 还支持 `(item,index) in items`形式，index表示当前项的索引。

```html
<ul id="example-2">
    <li v-for="(item, index) in items">
        {{ parentMessage }} - {{ index }} - {{ item.message }}
    </li>
</ul>
```
```js
var example2 = new Vue({
    el: '#example-2',
    data: {
        parentMessage: 'Parent',
        items: [
            { message: 'Foo' },
            { message: 'Bar' }
        ]
    }
})
```

* 可以使用of来代替in作为分隔符，因为它更接近JavaScript选代器的语法。

```html
<div v-for="item of items"></div>
```

## 在 v-for里使用对象

* 即：使用v-for来遍历一个对象的property｡ 

```html
<ul id="v-for-object" class="demo">
    <li v-for="value in object">
        {{ value }}
    </li>
</ul>
```
```js
new Vue({
    el: '#v-for-object',
    data: {
        object: {
            title: 'How to do lists in Vue',
            author: 'Jane Doe',
            publishedAt: '2016-04-10'
        }
    }
})
```

* 同样，也可以像遍历数组一样使用相同的遍历方式。
  * （value, name） in object
* 同时还可以使用第三个参数来代表索引。
  * (value, name, index) in object

## 维护状态

* 当Vue正在更新使用v-for渲染的元素列表时，它默认使用“**就地更新**”的策略。
* 如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。
* **为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key attribute。
* 尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
* 因为它是 Vue 识别节点的一个通用机制，`key` 并不仅与 `v-for` 特别关联。

## 数组更新检测

### 变更方法

* `push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`。
* 调用以上方法后将会改变原始数组进而触发视图的更新。

### 替换数组

* `filter()`、`concat()` 和 `slice()`等。
* 使用以上方法后并不会改变原始数组，相应的会生成一个新数组。
* 当使用以上方法时，可以**使用新数组替换旧数组**。
* Vue 为了使得 DOM 元素得到最大范围的重用而实现了一些智能的启发式方法，所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。
* 由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化。

## 显示过滤/排序后的结果

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```
```js
data: {
    numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
    evenNumbers: function () {
        return this.numbers.filter(function (number) {
            return number % 2 === 0
        })
    }
}
```

* 在计算属性不适用的情况下，可以调用方法来完成过滤等效果。

```html
<ul v-for="set in sets">
    <li v-for="n in even(set)">{{ n }}</li>
</ul>

```
```js
data: {
    sets: [[ 1, 2, 3, 4, 5 ], [6, 7, 8, 9, 10]]
},
methods: {
    even: function (numbers) {
        return numbers.filter(function (number) {
            return number % 2 === 0
        })
    }
}
```

## 在v-for里使用值范围

* v-for也可以接受整数，它会把模板重复对应次数。

```html
<div>
    <span v-for="n in 10">{{ n }} </span>
</div>
```

## v-for与v-if一同使用

* v-for的优先级比v-if更高。
* 同时，可以使用 v-if置于外层元素来有目的跳出循环。

```html
<ul v-if="todos.length">
    <li v-for="todo in todos">
        {{ todo }}
    </li>
</ul>
<p v-else>No todos left!</p>
```

## 其它

* 同时可以在自定义组件中来使用v-for。
* 当在组件上使用v-for时，key是必须的。
* 但由于任何数据都不会被自动传递到组件里（**因为组件有自己的独立作用域**），为了把迭代数据传到组件里，需要使用prop｡

```html
<my-component
    v-for="(item, index) in items"
    v-bind:item="item"
    v-bind:index="index"
    v-bind:key="item.id"
></my-component>
```