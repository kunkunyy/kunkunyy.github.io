---
title: Vue 学习笔记（三）条件渲染
date: 2022-02-04 18:46:54
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
---

# 条件渲染语句

* ```v-if```、```v-else-if```、```v-else```、```v-show```、```v-for```。

## 一、条件判断语句

* ```v-if```、```v-else-if```、```v-else```。

### v-if

* v-if指令用于条件性地渲染一块内容。
    * 这块内容只有当指令的表达式返回true值的时候被渲染。

```html
<h1 v-if="awesome">Vue is awesome!</h1>
```

### v-else

* 与v-if联合使用。

```html
<div v-if="Math.random() > 0.5">
    Now you see me
</div>
<div v-else>
    Now you don't
</div>
```

* **```v-else``` 元素必须紧跟在带 ```v-if``` 或者 ```v-else-if``` 的元素的后面，否则它将不会被识别**。

### v-else-if

* ```v-else-if```，顾名思义，充当 ```v-if``` 的“else-if 块”，可以连续使用：

```html
<div v-if="type === 'A'">
    A
</div>
<div v-else-if="type === 'B'">
    B
</div>
<div v-else-if="type === 'C'">
    C
</div>
<div v-else>
    Not A/B/C
</div>
```

* 类似于 v-else，```v-else-if```也必须紧跟在带 ```v-if```或者 ```v-else-if``` 的元素之后。

# 用key管理可复用的元素

* Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。

```html
<template v-if="loginType === 'username'">
    <label>Username</label>
    <input placeholder="Enter your username">
</template>
<template v-else>
    <label>Email</label>
    <input placeholder="Enter your email address">
</template>  
```
```js
var vm=new Vue({
    el:"#app",
    data:{
        loginType:'email'
    },
    methods:{
        qiehuan:function(){
            if(this.loginType=="email"){
                this.loginType=="username"
            }else{
                this.loginType=="email"
            }
        }
    }
});   
```
* 那么在上面的代码中切换 loginType 将不会清除用户已经输入的内容。
* Vue 提供了一种方式来声明“这两个元素是完全独立的——不要复用它们”。只需添加一个具有唯一值的 key 属性即可：
```html
<template v-if="loginType === 'username'">
    <label>Username</label>
    <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
    <label>Email</label>
    <input placeholder="Enter your email address" key="email-input">
</template>  
```

# v-show

* 带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 是简单地切换元素的 CSS 属性 display 。

```html
<h1 v-show="ok">Hello!</h1>  
```

# v-if vs v-show

* v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。
* **如果需要非常频繁地切换**，则使用 v-show 较好；
* **如果在运行时条件不太可能改变**，则使用 v-if 较好。

* v-if是通过控制dom节点的存在与否来控制元素的显隐；v-show是通过设置DOM元素的display样式，block为显示，none为隐藏；
* v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换；
* v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留；
* v-if有更高的切换消耗；v-show有更高的初始渲染消耗；

# v-if与v-for一起用

* 当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级。

```html
<ul>
   <li v-for="item in items" v-if="item.isOk">{{item.text}}</li>
</ul>
```
```js
var vm=new Vue({
    data:{
        items:[
            {text:"chifan",isOk:true},
            {text:"shuijue",isOk:false},
            {text:"kandianshi",isOk:true},
            {text:"dayouxi",isOk:true},
            {text:"kandianying",isOk:false},
        ]                   
    }
});  
```
* 如果你的目的是有条件地跳过循环的执行，那么可以将 v-if 置于外层元素 (或```<template>```)上。