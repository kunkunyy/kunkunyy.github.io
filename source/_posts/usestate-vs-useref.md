---
title: useState vs useRef
date: 2024-04-16 19:50:04
tags:
- React
- useState
- useRef
---

`useState` 和 `useRef` 是 React 中两个常用的钩子，它们都可以用于保存组件状态。但是，它们之间有什么区别？在什么情况下应该使用哪一个？本文将详细介绍 `useState` 和 `useRef` 的区别和使用场景。

# useState

`useState` 是 React 中最基本的钩子之一，用于在函数组件中保存状态。它返回一个包含两个元素的数组：当前状态和更新状态的函数。当调用更新状态的函数时，React 会重新渲染组件，并将新状态传递给组件。

```jsx
import React, { useState } from 'react';

const CounterComponent = () => {
  const [counter, setCounter] = useState(0);

  const incrementCounter = () => {
    setCounter(counter + 1);
  };

  return (
    <div>
      <p>Counter: {counter}</p>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};

export default CounterComponent;
```

* 在上面的示例中，我们使用 `useState` 钩子创建了一个计数器组件。`counter` 是当前状态，`setCounter` 是更新状态的函数。每次单击按钮时，`incrementCounter` 函数会将计数器增加 1。
* `useState` 钩子的一个重要特性是，它会将新状态合并到旧状态中。这意味着您可以更新状态的一部分，而不必担心丢失其他部分。
* 还可以接受一个函数作为参数，该函数返回初始状态。这对于计算初始状态或惰性初始化状态非常有用。

```jsx
const [counter, setCounter] = useState(() => {
  return 0;
});
```
* 缺点是，每次更新状态时，组件都会重新渲染。这可能会导致性能问题，尤其是在处理大型组件树时。
* `useState` 钩子适用于保存与组件渲染相关的状态，例如表单输入、UI 状态等。
* `useState` 还可以立即更新状态，而不必等到下一次渲染。这对于处理用户输入或其他交互事件非常有用。

```jsx
const CounterComponent = () => {
  const [counter, setCounter] = useState(0);

  const incrementCounter = () => {
    setCounter((prevCounter) => prevCounter + 1);
  };

  return (
    <div>
      <p>Counter: {counter}</p>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};
```

# useRef

`useRef` 是另一个 React 钩子，用于保存组件状态。与 `useState` 不同，`useRef` 返回一个可变的 ref 对象，该对象的 `current` 属性包含当前状态。当更新 `current` 属性时，组件不会重新渲染。

```jsx
import React, { useRef } from 'react';

const CounterComponent = () => {
  const counterRef = useRef(0);

  const incrementCounter = () => {
    counterRef.current += 1;
    console.log(counterRef.current);
  };

  return (
    <div>
      <p>Counter: {counterRef.current}</p>
      <button onClick={incrementCounter}>Increment</button>
    </div>
  );
};

export default CounterComponent;
```

* 在上面的示例中，我们使用 `useRef` 钩子创建了一个计数器组件。`counterRef` 是一个 ref 对象，它的 `current` 属性包含当前状态。每次单击按钮时，我们直接更新 `current` 属性，而不会调用更新状态的函数。