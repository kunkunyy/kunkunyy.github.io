---
title: Vue 学习笔记（一）MVVM模式
date: 2022-02-03 17:04:13
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
- [MVVM]
---

# 一、什么是MVVM

* MVVM**是一种架构设计模式，是一种简化用户界面的事件驱动编程方式**。
* MVVM源自于MVC模式。
* MVVM的核心是ViewModel层：
    * 该层向上与视图层进行双向数据绑定；
    * 向下与Model层通过接口请求进行数据交互。

# 二、MVVM的优点和好处

* MVVM模式和MVC模式一样，主要目的是分离视图和模型。
* 低耦合、可复用、独立开发、可测试。

# 三、MVVM的组成部分

* 视图层：用户界面，主要是HTML、CSS、Template；
* ViewModel：JavaScript、Runtime、Compiler（核心枢纽）；
* Model：指数据模型，泛指后端进行各种业务逻辑处理和数据操控，主要围绕数据库系统展开。

* **ViewModel所封装出来的数据模型包括视图的状态和行为两部分，而Model层的数据模型是只包含状态的。**
    * 比如页面的这一块展示什么，那一块展示什么这些都属于视图状态；
    * 页面加载进来发生什么。点击这一块发生什么，这一块滚动时发生什么都属于视图行为。

* 开发者只需要处理和维护ViewModel，更新数据视图就会自动得到响应更新，真正实现事件驱动。
* View层展现的不是Model层的数据，而是ViewModel的数据，由ViewModel负责与Model层交互，是前后端分离方案实施重要的一环。

# 四、双向绑定原理

* 双向绑定：data和view之间的自动化处理。
* data > view：正向Objest.defineProperty(ES5的js api)/reflect.defineProperty（ES6）。api能监听到这个data的变化，监听到变化后会有一个回调函数，回调函数里写清data和view关系。
* view > data:input时间
* Object.defineProperty和reflect.defineProperty区别：
    * Objest.defineProperty：返回一个新的对象；
    * reflect.defineProperty：返回一个布尔值。
* **手写Object.defineProperty：**

```js
var obj = {
    test:"hello"
}
//对象已有的属性添加特性描述
Object.defineProperty(obj,"test",{
    configurable:true | false,
    enumerable:true | false,
    value:任意类型的值,
    writable:true | false
});
 
// 在对象中添加一个属性与存取描述符的示例
var bValue;
Object.defineProperty(obj,"newKey",{
    get : function(){
        return bValue;
    },
    set : function(newValue){
       bValue = newValue;
    },    
    configurable:true,
    enumerable:true,
});
obj.newKey = 38;
```

* configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。
* enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。
* value属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
* writable为true时，value才能被赋值运算符改变。默认为 false。
* get: 一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。当访问该属性时，该方法会被执行，方法执行时没有参数传入，但是会传入this对象（由于继承关系，这里的this并不一定是定义该属性的对象）。默认为 undefined。
* set:一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。默认为 undefined。

# 五、使用的设计模式

* **观察者模式。**

{%asset_img pic1.png # tu1%}

* 实现一个数据监听器Observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者（Dep）
* 实现一个指令解析器Compile，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数
* 实现一个Watcher，作为连接Observer和Compile的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图
* mvvm入口函数，整合以上三者
