---
title: Antd 组件源码分析（一）Spin
date: 2023-11-06 11:32:19
tags:
- 组件库
categories:
- [Antd组件解析]
---

# Spin组件介绍

* 可以理解为loading组件，在页面局部处于等待异步数据或正在渲染过程时，合适的加载动效会有效缓解用户的焦虑。

# 源码分析

* 定义了SpinFC组件，它是Spin组件的包装，并提供了默认的指示器设置和样式处理

## props

```ts
interface SpinProps {
  prefixCls?: string;
  className?: string;
  rootClassName?: string;
  spinning?: boolean;
  style?: React.CSSProperties;
  size?: SpinSize;
  tip?: React.ReactNode;
  delay?: number;
  wrapperClassName?: string;
  indicator?: SpinIndicator;
  children?: React.ReactNode;
}
```
* prefixCls（可选）：Spin组件的CSS类名的前缀。
* className（可选）：Spin组件的额外CSS类名。
* rootClassName（可选）：Spin组件根元素的额外CSS类名。
* spinning（可选）：指示Spin组件是否处于加载状态，默认为true。
* style（可选）：Spin组件的行内样式。
* size（可选）：Spin组件的大小，可选值为'small'、'default'、'large'。
* tip（可选）：Spin组件的提示文本。
* delay（可选）：延迟显示加载状态的时间，单位为毫秒。
* wrapperClassName（可选）：嵌套模式下Spin组件包裹元素的额外CSS类名。
* indicator（可选）：自定义的加载指示器，可以是一个React节点。
* children（可选）：Spin组件的子元素。

## Spin 函数

```ts
const Spin: React.FC<SpinClassProps> = (props) => {
  /*
    通过解构赋值将props对象中的各个属性提取出来
  **/
  const {
    spinPrefixCls: prefixCls,
    spinning: customSpinning = true,
    delay = 0,
    className,
    rootClassName,
    size = 'default',
    tip,
    wrapperClassName,
    style,
    children,
    hashId,
    ...restProps
  } = props;

  /*
    使用React.useState钩子来定义一个状态变量spinning和一个更新该状态的函数setSpinning。
    初始值通过函数表达式设置，根据 customSpinning 和 delay 的值来判断是否应该处于加载状态。
  **/
  const [spinning, setSpinning] = React.useState<boolean>(
    () => customSpinning && !shouldDelay(customSpinning, delay),
  );

  /*
    使用React.useEffect钩子来处理副作用。
    在组件挂载或delay和customSpinning发生变化时，会执行回调函数。
    如果customSpinning为true，则会创建一个带有延迟的函数showSpinning，该函数在延迟时间后将spinning状态设置为true。
    然后调用showSpinning函数并返回一个清除函数，用于在组件卸载时取消延迟函数的执行。
    如果customSpinning为false，则直接将spinning状态设置为false。
  **/
  React.useEffect(() => {
    if (customSpinning) {
      const showSpinning = debounce(delay, () => {
        setSpinning(true);
      });
      showSpinning();
      return () => {
        showSpinning?.cancel?.();
      };
    }

    setSpinning(false);
  }, [delay, customSpinning]);

  /*
    使用React.useMemo钩子来计算isNestedPattern变量的值。它根据spin是否包含子元素判断是否为嵌套模式。
  **/
  const isNestedPattern = React.useMemo<boolean>(() => typeof children !== 'undefined', [children]);

  /*
    使用devUseWarning函数创建一个警告方法，并检查tip属性是否存在以及是否处于嵌套模式。
    如果不满足条件，则会触发警告。
  **/
  if (process.env.NODE_ENV !== 'production') {
    const warning = devUseWarning('Spin');

    warning(!tip || isNestedPattern, 'usage', '`tip` only work in nest pattern.');
  }
  // 使用React.useContext钩子获取全局配置和样式。
  const { direction, spin } = React.useContext<ConfigConsumerProps>(ConfigContext);
  // 使用classNames函数生成Spin组件根元素的className。根据size、spinning、tip和direction等属性的值来动态生成不同的className。
  const spinClassName = classNames(
    prefixCls,
    spin?.className,
    {
      [`${prefixCls}-sm`]: size === 'small',
      [`${prefixCls}-lg`]: size === 'large',
      [`${prefixCls}-spinning`]: spinning,
      [`${prefixCls}-show-text`]: !!tip,
      [`${prefixCls}-rtl`]: direction === 'rtl',
    },
    className,
    rootClassName,
    hashId,
  );
  // 使用classNames函数生成包裹Spin组件内容的容器元素的className。如果spinning为true，则添加${prefixCls}-blur类名。
  const containerClassName = classNames(`${prefixCls}-container`, {
    [`${prefixCls}-blur`]: spinning,
  });

  // fix https://fb.me/react-unknown-prop
  const divProps = omit(restProps, ['indicator', 'prefixCls']);

  const mergedStyle: React.CSSProperties = { ...spin?.style, ...style };

  const spinElement: React.ReactNode = (
    <div
      {...divProps}
      style={mergedStyle}
      className={spinClassName}
      aria-live="polite"
      aria-busy={spinning}
    >
      {renderIndicator(prefixCls, props)}
      {tip && isNestedPattern ? <div className={`${prefixCls}-text`}>{tip}</div> : null}
    </div>
  );
  // 如果是嵌套模式，则需要将子元素加上，然后将 spin 元素嵌套在子元素上
  if (isNestedPattern) {
    return (
      <div
        {...divProps}
        className={classNames(`${prefixCls}-nested-loading`, wrapperClassName, hashId)}
      >
        {spinning && <div key="loading">{spinElement}</div>}
        <div className={containerClassName} key="container">
          {children}
        </div>
      </div>
    );
  }
  // 反之直接返回 spin 组件
  return spinElement;
};
```

## SpinFC函数

```ts
const SpinFC: SpinFCType = (props) => {
  /*
    定义了一个名为SpinFC的函数，它接受一个SpinProps类型的props参数。
    通过解构赋值，从props中提取prefixCls属性，并使用React.useContext获取全局配置中的getPrefixCls函数。
  **/
  const { prefixCls: customizePrefixCls } = props;
  const { getPrefixCls } = React.useContext(ConfigContext);
  
  // 使用getPrefixCls函数生成spinPrefixCls，它是用于生成Spin组件CSS类名的前缀。
  const spinPrefixCls = getPrefixCls('spin', customizePrefixCls);

  // 使用useStyle自定义钩子函数，传入spinPrefixCls作为参数。它返回一个包含两个元素的数组，分别是wrapSSR和hashId。
  const [wrapSSR, hashId] = useStyle(spinPrefixCls);
  // wrapSSR 可以理解为一个给传入的组件生成指定样式的函数
  const spinClassProps: SpinClassProps = {
    ...props,
    spinPrefixCls,
    hashId,
  };
  // 返回值为当前组件+样式后的最终组件
  return wrapSSR(<Spin {...spinClassProps} />);
};
```