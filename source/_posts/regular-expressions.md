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

# 正则ES2018

## 后行断言 Lookbehind assertions

完整的断言定义分为：`正/负向断言`与`先/后行断言`的笛卡尔积组合，在`ES2018`之前仅支持先行断言，现在终于支持了后行断言。

### 正向先行断言

`(?=...)`表示之后的字符串**能**匹配 pattern。

```ts
const re = /js(?= design)/;

console.log(re.exec("js"));
// → null

console.log(re.exec("jsdesign"));
// → null

console.log(re.exec("jser"));
// → null

console.log(re.exec("js design"));
// → ['js', index: 0, input: 'js design', groups: undefined]
```

### 负向先行断言

`(?!...)` 表示之后的字符串**不能**匹配 pattern。

```ts
const re = /VS(?!Code)/;

console.log(re.exec("VSCode"));
// → null

console.log(re.exec("VSCold"));
// → ['VS', index: 0, input: 'VSCold', groups: undefined]

console.log(re.exec("VSCool"));
// → ['VS', index: 0, input: 'VSCool', groups: undefined]

console.log(re.exec("VS"));
// → ['VS', index: 0, input: 'VS', groups: undefined]
```

在 ES2018 后，又支持了两种新的断言方式：

### 正向后行断言

`(?<=...)` 表示之前的字符串**能**匹配 pattern 。

```ts
const re = /(?<=js)\w+/;

console.log(re.exec("gees"));
// → null

console.log(re.exec("jsdesign"));
// → ['design', index: 2, input: 'jsdesign', groups: undefined]

console.log(re.exec("jsai"));
// → ['ai', index: 2, input: 'jsai', groups: undefined]
```

### 负向后行断言

`(?<!...)` 表示之前的字符串**不能**匹配 pattern。

```ts
const re = /(?<!\d{3}) meters/;

console.log(re.exec("10 meters"));
// → [" meters", index: 2, input: "10 meters", groups: undefined]

console.log(re.exec("100 meters"));
// → null
```

## 命名捕获组 Named capture groups

* 命名捕获组可以给正则捕获的内容命名，比起下标来说更可读
* 其语法是 `?<name>`：

```ts
const re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/;
// const [match, year, month, day] = re.exec("2020-03-04");
const {year, month, day} = re.exec("2020-03-04").groups;

console.log(year); // → 2020
console.log(month); // → 03
console.log(day); // → 04
```

也可以在正则表达式中，通过下标 `\1` 直接使用之前的捕获组，比如：

```ts
console.log(/(\w\w)\1/.test("abab")); // → true

console.log(/(\w\w)\1/.test("abcd")); // → false
```

对于命名捕获组，可以通过 `\k<name>` 的语法访问，而不需要通过 `\1` 这种下标：

```ts
const re = /\b(?<dup>\w+)\s+\k<dup>\b/;

console.log(re.exec("I'm not lazy, I'm on on energy saving mode"));
// → ['on on', 'on', index: 18, input: "I'm not lazy, I'm on on energy saving mode", groups: {dup: 'on'}]
```

## 匹配任意字符 s (dotAll) Flag

虽然正则中 `.` 可以匹配任何字符，但却无法匹配换行符。聪明的开发者们用 `[\w\W]` 巧妙的解决了这个问题。然而这终究是个设计缺陷，在 ES2018 支持了 `/s` 模式，这个模式下，`.` 等价于 `[\w\W]`：

```ts
console.log(/./s.test("\n")); // → true
console.log(/./s.test("\r")); // → true
```

## Unicode 属性转义 Unicode property escapes

* 正则支持了更强大的 `Unicode` 匹配方式。在 `/u` 模式下，可以用 `\p{Number}` 匹配所有数字：

```ts
const regex = /^\p{Number}+$/u;
regex.test("²³¹¼½¾"); // true
regex.test("㉛㉜㉝"); // true
regex.test("ⅠⅡⅢⅣⅤⅥⅦⅧⅨⅩⅪⅫ"); // true
```

* `\p{Alphabetic}` 可以匹配所有 `Alphabetic` 元素，包括汉字、字母等：

```ts
const str = "即时设计";

console.log(/\p{Alphabetic}/u.test(str)); // → true

// the \w shorthand cannot match 即时设计
console.log(/\w/u.test(str)); // → false
```

## 兼容性

* 总体上只有 `Chrome` 与 `Safari` 支持，`Firefox` 与 `Edge` 都不支持。所以大型项目使用要再等几年。不过这些方法在 `VS Code` 的搜索里面是可以使用的。
