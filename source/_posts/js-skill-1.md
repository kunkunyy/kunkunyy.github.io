---
title: JS 基础技能 1
date: 2021-10-31 17:23:09
series: JS 技巧
cover: /img/article/js-skill.webp
tags:
- 知识点总结
categories:
- [前端刷题]
- [JS]
---

> JavaScript——判断字符串中是否包含某个字符/字符串

# 方法一：indexOf()

* 返回某个指定的字符串值在字符串中首次出现的位置；如果不存在，则返回-1；

```js
let str = 'Hello World!';
if(str.indexOf("World")){
    console.log(true);
}else{
    console.log(false);
}
//true
```

# 方法二：search()

* 方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。如果没有找到任何匹配的子串，则返回 -1。

```js
let str = 'Hello World!';
if(str.search("World")){
    console.log(true);
}else{
    console.log(false);
}
//true
```

# 方法三: match()

* 方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。

```js
let str = 'Hello World!';
let reg = RegExp(/Hello/);
if(str.match(reg)){
    console.log(true);
}else{
    console.log(false);
}
//true
```

# 方法四: test()

* 方法用于检索字符串中指定的值。返回 true 或 false。

```js
let str = 'Hello World!';
let reg = RegExp(/Hello/);
if(reg.test(str)){
    console.log(true);
}else{
    console.log(false);
}
//true
```

# 方法五:exec()

* 方法用于检索字符串中的正则表达式的匹配。返回一个数组，其中存放匹配的结果。如果未找到匹配，则返回值为 null。

```js
let str = 'Hello World!';
let reg = RegExp(/Hello/);
if(reg.exec(str)){
    console.log(true);
}else{
    console.log(false);
}
//true
```