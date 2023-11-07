---
title: 前端工具包 - className
date: 2023-11-06 15:07:13
tags:
- 前端工具包
categories:
- [前端工具包]
- [className]
---

# 简介

* 用于有条件地将类名连接在一起，常用于 React 应用。

# 基本使用

* 该classNames函数接受任意数量的参数，可以是字符串或对象。该参数`'foo'`是 的缩写`{ foo: true }`。
* 如果与给定键关联的值是假的，则该键将不会包含在输出中。

```js
const classNames = require('classnames') // import classNames from 'classnames'
classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'

// lots of arguments of various types
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// other falsy values are just ignored
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```

* 配合模版字符串使用

```js
const classNames = require('classnames')
let buttonType = 'primary';
classNames({ [`btn-${buttonType}`]: true });
```

* 通过 `classname/bind` 实现类名合并。
* 注：如果有重复的类名不会被去重

```js
const classNames = require('classnames')

console.log(classNames('a', ['a','b','c'], {d: true,e:false,f:true}))
// a a b c d f
```

# 源码分析

```js
// 采用立即执行函数避免样式污染等问题
(function () {
	'use strict';

  // hasOwn是一个引用了 {}.hasOwnProperty 方法的变量。
  // nativeCodeString 是一个字符串，表示原生代码。
	var hasOwn = {}.hasOwnProperty;
	var nativeCodeString = '[native code]';

  // 该函数接受任意数量的参数，并根据参数的类型进行处理
	function classNames() {
		var classes = [];// 所有类名的集合

		for (var i = 0; i < arguments.length; i++) {
			var arg = arguments[i];
			if (!arg) continue;

			var argType = typeof arg;
      // 如果是字符串和数字直接放入结果中
			if (argType === 'string' || argType === 'number') {
				classes.push(arg);
			}
      // 数组：递归调用 classNames 函数
      else if (Array.isArray(arg)) {
				if (arg.length) {
					var inner = classNames.apply(null, arg);
					if (inner) {
						classes.push(inner);
					}
				}
			}
      // 对象：首先检查对象的toString方法是否被重写且不是原生方法，如果满足条件，则将对象转换为字符串，并将结果加入到classes数组中
      else if (argType === 'object') {
				if (arg.toString !== Object.prototype.toString && !arg.toString.toString().includes('[native code]')) {
					classes.push(arg.toString());
					continue;
				}

				for (var key in arg) {
					if (hasOwn.call(arg, key) && arg[key]) {
						classes.push(key);
					}
				}
			}
		}
    // 结果返回字符串
		return classes.join(' ');
	}

  // 根据不同的运行环境，将classNames函数导出为模块或在全局作用域中定义。
  /**
   * 如果代码运行在 CommonJS 环境（如Node.js），则将classNames函数赋值给module.exports，使其可以被其他模块引用。
   * 同时，将classNames函数赋值给classNames.default属性，以便在导入模块时可以通过default属性访问到该函数。
  */
	if (typeof module !== 'undefined' && module.exports) {
		classNames.default = classNames;
		module.exports = classNames;
	}
  /**
   * 如果代码运行在 AMD 环境（如RequireJS），则使用define函数将classNames函数定义为一个模块，模块名为classnames。
  */
  else if (typeof define === 'function' && typeof define.amd === 'object' && define.amd) {
		// register as 'classnames', consistent with npm package name
		define('classnames', [], function () {
			return classNames;
		});
	}
  /**
   * 假设代码运行在浏览器环境，将classNames函数赋值给全局对象window的classNames属性，使其在全局作用域中可访问。
  */
  else {
		window.classNames = classNames;
	}
}());
```