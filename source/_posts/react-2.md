---
title: React学习笔记（二）面向组件编程
date: 2022-05-22 10:34:33
tags:
- React学习
categories:
- [前端学习笔记]
- [React]
---

# 创建函数式组件

```js
// 函数式创造组件
function MyComponent(params) {
    console.log(this);
    return <h2>我是用函数定义的组件（适用于【简单组件】的定义）</h2>;
}
// 渲染组件
ReactDOM.render(<MyComponent/>,document.getElementById('test'));
```

* 注意：由于babel编译会开启严格模式，在严格模式下this指向为undefined。
* 渲染组件到页面的整个过程：
    * React解析组件标签，找到MyComponent组件。
    * 发现组件时使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。

# 创建类式组件

```js
// 类式组件创建
class MyComponent extends React.Component{
    render(){
        return <h2>我是用类定义的组件（适用于【复杂组件】的定义）</h2>;
    }
}
// 渲染组件
ReactDOM.render(<MyComponent />,document.getElementById('test'))
```

* 渲染组件到页面的整个过程：
    * React解析组件标签，找到MyComponent组件。
    * 发现组件时使用类定义的，随后使用new创建该类的实例对象，并通过该实例调用到原型上的render方法。
    * 将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。

# 简单组件和复杂组件

* 简单组件：未拥有状态的组件
* 复杂组件：拥有状态的组件
* 组件拥有状态驱动着页面。

# 组件实例三大属性

## state

* state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)。
* 组件被称为"状态机", 通过更新组件的state来更新对应的页面显示