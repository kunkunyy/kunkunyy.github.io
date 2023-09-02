---
title: React Hooks
date: 2022-09-04 11:29:13
tags:
- React学习
categories:
- [前端学习笔记]
- [React]
---

* react hook 不能放在循环、判断语句中，以及函数体中

# useState()

* 解决函数式组件中无法使用setState管理状态的弊端。

```jsx
export default function Demo(){
  const [count, setCount] = useState(0)；
  const onClick = ()=>{
    // 直接调用setCount修改值
    setCount(count + 1);
    // 支持回调函数
    setCount((count) => count + 1);
    // 错误写法，页面不会刷新
    setCount(()=>{
      return count++;
    })
  }
  return (
    <div>
      <div>{count}</div>
      <button onClick={onClick}>button</button>
    </div>
  )
}
```

# useReducer()

* useState() 的代替方案，可以形如 redux 来处理对象数据
* 它接收一个形如 (state, action) => newState 的 reducer。

```jsx
export default function Demo(){
  // 一般放在reducer文件夹里，然后使用import进行引入。
  const countReducer = (state, action)=>{
    switch (action.type){
      case 'add':
        return state + 1;
      case 'minus':
        return state - 1;
      default:
        return state;
    }
  }
  const [count, countDispatch] = useReducer(countReducer, 0);
  const onClick = ()=>{
    countDispatch({type: 'add'});
  }
  return (
    <div>
      <div>{count}</div>
      <button onClick={onClick}>button</button>
    </div>
  )
}
```

# useEffect()

* 副作用：纯函数和外部交互即为副作用，即：只要不是在组件渲染时用到的变量，所有的操作都是副作用。
  - 引用外部变量
  - 调用外部函数
* 常见的副作用操作：和外部变量的交互
  - 修改DOM
  - 修改全局变量
  - ajax请求
  - 计时器
  - 存储相关
* 纯函数：相同的输入 === 相同的输出。

* useEffect()包含三个生命周期
  - 真实DOM构建之前调用componentDidMount、componentDidUpdate、componentWillUnmount
  - 真实DOM构建以后调用useEffect()
* useEffect为异步执行，不会阻塞页面。
* 每一次副作用函数是不同的函数。
* 如果存在清理函数：
  - render + useEffect -> render + 清理函数 + useEffect -> ...循环... ->
* 第二个参数：
  - 指定当前 effect函数所需要的依赖项；
  - 依赖是空数组，只会在初次渲染和卸载的时候执行；
  - 有依赖项，并且依赖项不一致的时候会重新执行。

```jsx
export default function Demo(){
  const [count, setCount] = useState(0);
  console.log("render")
  useEffect(()=>{
    let timer = setInterval(()=>{
      setCount(count => count + 1);
    },1000);
    // 清除副作用函数
    return ()=>{
      clearInterval(timer);
    }
  },[])
  return (
    <div>
      <div>{count}</div>
      <button onClick={()=>setCount(count + 1)}>button</button>
    </div>
  )
}
```

# useContext()

* 父子/祖孙之间传递值的一种方式。
* 在函数式组件中使用 useContext 来获取值。

```jsx
const AppContext = createContext();

export default function Father(){
  return(
    <AppContext.Provider value="zhangsan">
      <Child />
    </AppContext.Provider>
  )
}

const Child = (params)=>{
  const value = useContext(AppContext);
  return (
    <div>{value}</div>
  )
}
```

# useRef() 和 forwardRef()

* ref 用于获取真实DOM。
* 只有一个组件：

```jsx
export default function App(){
  const inputRef = useRef();
  const onClick = ()=>{
    inputRef.current.focus();
  }
  return (
    <div>
      <input type= "text" ref={ inputRef }/>
      <button onClick = {onClick}>聚焦</button>
    </div>
  )
}
```
* 父子组件：

```jsx
const Demo = forwardRef((inputRef)=>{
  const onClick = ()=>{
    inputRef.current.focus();
  }
  return (
    <div>
      <input type= "text" ref={ inputRef }/>
      <button onClick = {onClick}>聚焦</button>
    </div>
  )
})

export default function App(){
  const inputRef = useRef();
  const onClick = ()=>{
    inputRef.current.focus();
  }
  return (
    <div>
      <button onClick = {onClick}>父组件控制聚焦</button>
    </div>
  )
}
```

# useMemo() 和 useCallback()

* 两者均用于性能优化使用，且在渲染阶段调用。
* useMemo() 用于控制值，useCallback用于控制函数。
* useCallback(fn,deps) ==等价于== useMemo(() => fn,deps)