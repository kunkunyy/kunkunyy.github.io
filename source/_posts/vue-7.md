---
title: Vue 学习笔记（七）模板语法
date: 2022-02-09 17:41:05
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
---

# 插值

* 双大括号的文本插值。
* 通过使用 v-once 指令，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。

```html
<span>Message: {{ msg }}</span>
<span v-once>这个将不会改变: {{ msg }}</span>
```

# 指令

* 指令（Directives）是带有 v- 前缀的特殊属性。
* 指令的职责：当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

## 参数

* 一些指令能够接收一个“参数”，在指令名称之后以冒号表示。

```js
<a v-on:click="doSomething">...</a>
```

## 动态参数

```html
<a v-bind:[attributeName]="url"> ... </a>
```
* 用方括号括起来的 JavaScript 表达式作为一个指令的参数。
* 这里的 attributeName 会被作为一个 JavaScript 表达式进行动态求值，求得的值将会作为最终的参数来使用。
* 同样地，可以使用动态参数为一个动态的事件名绑定处理函数：

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

* **对动态参数的值的约束**
    * 动态参数预期会求出一个字符串，异常情况下值为 null。
    * 这个特殊的 null 值可以被显性地用于移除绑定。
    * 任何其它非字符串类型的值都将会触发一个警告。
* **对动态参数表达式的约束**
    * 动态参数表达式有一些语法约束，因为某些字符，如空格和引号，放在 HTML attribute 名里是无效的。
    * 变通的办法是使用没有空格或引号的表达式，或用计算属性替代这种复杂表达式。

```html
<!-- 这会触发一个编译警告 -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

## 修饰符

* 修饰符 (modifier) 是以半角句号 . 指明的特殊后缀用于指出一个指令应该以特殊方式绑定。

```html
<form v-on:submit.prevent="onSubmit">...</form>
```

# 缩写

* v- 前缀作为一种视觉提示，用来识别模板中 Vue 特定的 attribute。
*  `v-bind` 和 `v-on` 这两个最常用的指令，提供了特定简写。

## `v-bind`缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>
```

## `v-on`缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```