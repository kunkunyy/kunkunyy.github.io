---
title: React学习笔记（二）面向组件编程1
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

* 函数式组件没有 state、refs 属性，它可以通过函数传参的方式来间接性获取props中的数据。

## state

* state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)。
* 组件被称为"状态机", 通过更新组件的state来更新对应的页面显示。

* 组件中render方法中的this为组件实例对象
* 组件自定义的方法中this为undefined，如何解决？
    * 强制绑定this: 通过函数对象的bind()
    * 箭头函数
* 状态数据，不能直接修改或更新，需要使用setState

## props

* 每个组件对象都会有props(properties的简写)属性
* 组件标签的所有属性都保存在props中
* 作用：
    * 通过标签属性从组件外向组件内传递变化的数据
    * 注意: 组件内部不能修改props数据

### 编码操作

* 读取props中某个属性值。

```js
this.props.name
```

* 对props中的属性值进行类型限制和必要性限制
    * 使用prop-types库进限制

```js
Person.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number. 
}
```

* 扩展属性: 将对象的所有属性通过props传递

```js
<Person {...person}/>
```

* 默认属性值

```js
Person.defaultProps = {
  age: 18,
  sex:'男'
}
```

* 组件类的构造函数
    * 使用props必须在super中传入

```js
constructor(props){
  super(props)
  console.log(props)//打印所有属性
}
```

## refs与事件处理

* refs用于访问真实 DOM 节点。
* 函数式组件使用useRef创建ref对象，类式组件使用React.createRef()创建ref对象。
* Refs 是 React 提供的用来保存 object 引用的一个解决方案，在函数式组件使用 useRef 创建一个 ref 对象，ref 对象存在一个可直接修改的 current 属性，内容都是存在 current 上。

### 使用场景

* Refs 使用场景主要分为两个方向：
    * 实现 DOM 访问与操控，
    * 在两次render之间传递数据内容。

### 使用步骤

1. 创建ref对象
2. 赋值和使用
3. 访问ref内容

```tsx
import React, { useRef, useState } from "react";

export default function CompA() {
  // 第一步：使用useRef 创建 ref 对象
  const ref = useRef();
  const [isSending, setIsSending] = useState(false);

  function send() {
    // ...
    window.alert("消息已发送！");
    setIsSending(false);
  }

  function undo() {
    // 第三步： 访问存在 ref 上的 timeout ID， 进行定时取消
    clearTimeout(ref.current);
  }

  function handleClickSendBtn() {
    setIsSending(true);
    // 第二步： 赋值，将 timeout ID 存在 ref 上
    ref.current = setTimeout(send, 3000);
  }

  function handleClickCancelBtn() {
    undo();
    setIsSending(false);
  }

  return (
    <>
      <button onClick={handleClickSendBtn} disabled={isSending}>
        {isSending ? "发送中..." : "发送"}
      </button>
      {isSending && <button onClick={handleClickCancelBtn}>取消发送</button>}
    </>
  );
}
```

### 核心要点

1. 避免重复创建 ref 内容
    * 使用useRef创建ref对象时可以初始化内容，同时会保存**一次**初始值，并带到下一次render中。
    * 因此在 useRef 在创建ref的时候传递重复的内容是不生效的。
2. ref.current 存储的内容修改是突变
    * ref存储的实际就是一个引用，因此是可突变的。
    * ref 可直接修改 current 属性上的内容，并且修改后可以立即取到值。
3. ref 作为数据存储时内容的变化不会引起 re-render
    * React 组件的 re-render 的触发一般是【state、props、context】中的出现变化引起的。
    * 修改 Ref 的内容不会引起组件的 re-render 因此不能用 ref 去干预 React 生成jsx。
4. ref 的读写只能在 useEffect 或者回调函数中进行
5. 跨组件传递ref 获取dom时需要借助 forwardRef 包裹组件
```tsx
import React, { useEffect, useRef, useState, forwardRef } from "react";

export function ParentComp() {
  const childInputRef = useRef(null);

  function handleClick() {
    childInputRef.current?.focus();
  }

  return (
    <>
      <button onClick={handleClick}>编辑</button>
      <ChildComp ref={childInputRef} />
    </>
  );
}

// 使用forwordRef 包裹组件，接受 ref 并转发绑定到对应dom上
const ChildComp = forwardRef((props, ref) => {
  return (
    <div>
      <input {...props} ref={ref} />
    </div>
  );
});
```
6. ref 绑定的dom在离屏或者未挂载时ref.current 值会被修改为null

### 事件处理

* React将方法是小写的都重写了一遍。
    * 即：onClick、onBlur。
* 通过event.target得到发生事件的DOM元素对象。

