---
title: 剑指 Offer 05. 替换空格
date: 2022-02-20 20:42:13
tags:
- LeetCode刷题
categories:
- [前端刷题]
- [JS]
- [双指针]
- [字符串]
---

# 题目

[点击前往](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

示例1：
```
输入：s = "We are happy."
输出："We%20are%20happy."
```

# 解题思路

## 双指针法

* 将字符串转换为数组，查询空格的个数；由于一个空格将会被三个字符给替换，所以此时需要将数组的长度扩大。
* 然后利用双指针从后往前进行查找替换：
    * 左指针每遇到一个空格就需要将右指针接下来三个位置换成指定内容；反之将右指针内容替换为左指针。

```js
/**
 * @param {string} s
 * @return {string}
 */
var replaceSpace = function(s) {
    let arr = Array.from(s);
    let count = 0;
    arr.forEach(e=>{
        if(e === " "){
            count++;
        }
    })
    let left = arr.length - 1;
    let right = arr.length + count * 2 - 1;
    while(left >=0 ){
        if(arr[left] === " "){
            arr[right--] = "0";
            arr[right--] = "2";
            arr[right--] = "%";
            left--;
        }else{
            arr[right--] = arr[left--];
        }
    }
    return arr.join("")
};
```

## replace()

```js
/**
 * @param {string} s
 * @return {string}
 */
var replaceSpace = function(s) {
    return s.replace(/ /g,"%20");
};
```