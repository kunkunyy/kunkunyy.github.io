---
title: Vue 学习笔记（六）表单输入绑定
date: 2022-02-05 21:36:25
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
---

# 表单输入绑定

## 基础用法

* 可以用 v-model 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。
* `v-model` 本质上不过是语法糖。
* 它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。
* **`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源。**
* `v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：
    * text 和 textarea 元素使用 `value` property 和 `input` 事件；
    * checkbox 和 radio 使用 `checked` property 和 `change` 事件；
    * select 字段将 `value` 作为 prop 并将 `change` 作为事件。

## 修饰符

### .lazy

* 在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步。
* 可以添加 `lazy` 修饰符，从而转为在 change 事件_之后_进行同步。

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

### .number

* 给 `v-model` 添加 `number` 修饰符，可以自动将用户的输入值转为数值类型。

```html
<input v-model.number="age" type="number">
```

### .trim

* 给 v-model 添加 trim 修饰符，可以自动过滤用户输入的首尾空白字符。

```html
<input v-model.trim="msg">
```