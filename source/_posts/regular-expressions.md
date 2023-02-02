---
title: 正则表达式
date: 2023-02-02 10:53:56
tags:
- 正则表达式
categories:
- [正则表达式]
---

# 正则表达式的核心

* **正则就是匹配模式，要么匹配位置，要么匹配字符。**

# 模式

| 模式 | 说明 |
| :--: | :--: |
| ^ | 匹配开头的位置 |
| $ | 匹配结尾的位置 |
| \b | 匹配单词边界 |
| \B | 匹配非单词边界 |
| (?=表达式) | 正向先行断言：(?=表达式)，指在某个位置的右侧必须能匹配表达式 |
| (?!表达式) | 反向先行断言：(?!表达式)，指在某个位置的右侧不能匹配表达式 |
| (?<=表达式) | 正向后行断言：(?<=表达式)，指在某个位置的左侧必须能匹配表达式 |
| (?<!表达式) | 反向后行断言：(?<=表达式)，指在某个位置的左侧必须能匹配表达式 |

> 1.匹配/开头结尾的位置

```js
const str1 = 'hello world';
const str2 = 'world';
const expHead = /^hello/;
const expEnd = /^world/;
expHead.test(str1);// true
expHead.test(str2);// false
expEnd.test(str1);// true
expEnd.test(str2);// false
```

> 2.匹配（非）边界

```js
const str1 = 'hello world';
const str2 = 'helloworld';
const exp1= /\bhello\b/;
exp1.test(str1);// true
exp1.test(str2);// false
const exp2 = /\Bowo\B/;
exp2.test(str1);// false
exp2.test(str2);// true
```
