---
title: 剑指 Offer 58 - II. 左旋转字符串
date: 2022-02-21 16:26:06
tags:
- LeetCode刷题
categories:
- [前端刷题]
- [JS]
- [双指针]
- [字符串]
---

# 题目

[点击前往](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

示例1：
```
输入: s = "abcdefg", k = 2
输出: "cdefgab"
```

# 解题思路

* 局部翻转+整体翻转；
* 先翻转0-n位置元素，再翻转n-len位置元素，最后整体翻转。

```js
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function(s, n) {
    let strArr = Array.from(s);
    reverse(strArr, 0, n-1);
    reverse(strArr, n, strArr.length-1);
    reverse(strArr, 0, strArr.length-1);
    return strArr.join("");
    function reverse(arr,start,end){
        let l = start,r = end;
        while(l < r){
            [arr[l],arr[r]] = [arr[r],arr[l]];
            l++;
            r--;
        }
    }
};
```

## slice字符串截取，然后拼接

```js
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function(s, n) {
    return s.slice(n,s.length) + s.slice(0,n);
};
```