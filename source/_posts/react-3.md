---
title: React学习笔记（三）组件生命周期
date: 2022-08-01 17:43:55
tags:
- React学习
categories:
- [前端学习笔记]
- [React]
---

# 生命周期流程图

{% asset_img 组件生命周期.png first %}

# 生命周期函数

## 初始化阶段

### constructor构造函数

* 在 React 组件挂载之前被调用，实现 React.Component 的子类的构造函数时，要在第一行加上 super(props)。
* React 构造函数通常只用于两个目的：
    * 通过分配一个对象到 this.state 来初始化本地 state。
    * 将事件处理程序方法绑定到实例。
* 如果没有初始化状态（state），并且没有绑定方法，通常不需要为 React 组件实现一个构造函数。
* 不需要在构造函数中调用 setState()，只需将初始状态设置给 this.state 即可 。

### static getDerivedStateFromProps(nextProps, prevState)

* getDerivedStateFromProps 在每次调用 render 方法之前调用。包括初始化和后续更新时。
* 包含两个参数：
    * 第一个参数为即将更新的 props 值；
    * 第二个参数为之前的 state 值
* 返回值：返回为 null 时，不做任何副作用处理。倘若想更新某些 state 状态值，则返回一个对象，就会对 state 进行修改。
* 该生命周期是**静态函数**，属于类的方法，其作用域内是找不到 this 。

### render()

* render() 方法是类组件中唯一必须的方法，其余生命周期不是必须要写。
* 调用时机：初始化渲染和状态发生更新
* 如果 shouldComponentUpdate() 方法返回 false ，render() 不会被调用。

### componentDidMount()

* 在 React 组件装载(mounting)（插入树）后被立即调用。
* componentDidMount 生命周期是进行发送网络请求、启用事件监听的好时机
* 如果有必要，可以在此生命周期中立刻调用 setState()

## 更新阶段

### shouldComponentUpdate(nextProps, nextState)

* 在组件准备更新之前调用，可以控制组件是否进行更新，返回 true 时组件更新，返回 false 组件不更新。
* 包含两个参数，第一个是即将更新的 props 值，第二个是即将更新后的 state 值，可以根据更新前后的 props 或 state 进行判断，决定是否更新，进行性能优化
* 不要在 shouldComponentUpdate 中调用 setState()，否则会导致**无限循环调用更新、渲染，直至浏览器内存崩溃**。

### componentDidUpdate(prevProps, prevState, snapshot)

* componentDidUpdate() 在**更新发生之后立即被调用**。这个生命周期在组件**第一次渲染时不会触发**。
* 可以在此生命周期中调用 setState()，但是**必须包含在条件语句中**，否则会造成无限循环，最终导致浏览器内存崩溃。

### getSnapshotBeforeUpdate(prevProps, prevState)

* getSnapshotBeforeUpdate() 在最近一次的渲染输出被提交之前调用。也就是说，在 render 之后，即将对组件进行挂载时调用。
* 此生命周期返回的任何值都会作为参数传递给 componentDidUpdate()。

## 卸载组件阶段

### componentWillUnmount()

* componentWillUnmount() 在组件即将被卸载或销毁时进行调用。
* 此生命周期是取消网络请求、移除监听事件、清理 DOM 元素、清理定时器等操作的好时机

# 关于setState()是否可以调用

* 初始化 state
    * constructor()
* 可以调用 setState()
    * componentDidMount()
* 根据判断条件可以调用 setState()
    * componentDidUpdate()
* 禁止调用 setState()
    * shouldComponentUpdate()
    * render()
    * getSnapshotBeforeUpdate()
    * componentWillUnmount()