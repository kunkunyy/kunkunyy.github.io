---
title: 算法学习笔记（四）常见的排序算法
date: 2022-03-01 22:43:17
tags:
- 算法学习
categories:
- [算法学习笔记]
- [排序算法]
---

# 前言

## 算法分类

* 比较类排序：**通过比较来决定元素间的相对次序**，由于其时间复杂度不能突破O(nlogn)，因此也称为**非线性时间比较类排序**。
* 非比较类排序：**不通过比较来决定元素间的相对次序**，它可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序。

{%asset_img pic1.webp # tu1%}

## 算法复杂度

{%asset_img pic2.webp # tu1%}

# 插入排序

* 将原序列分为已排序与未排序的两部分。
* 外层循环遍历整个序列，标记当前待插入元素。
* 内层循环遍历已排序序列，从有序表的尾部开始与当前值进行比较移动。

{%asset_img pic3.webp # tu1%}

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
    let len = nums.length;
    for(let i = 0;i < len;i++){
        for(let j = i;j > 0;j--){
            if(nums[j-1] > nums[j]){
                [nums[j - 1],nums[j]] = [nums[j],nums[j-1]];
            }else{
                break;
            }
        }
    }
    return nums;
};
```

## 选择排序

* 选择排序(Selection-sort)是一种简单直观的排序算法。它的工作原理：首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。

{%asset_img pic4.webp # tu1%}

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
    let len = nums.length;
    for(let i = 0;i < len-1;i++){
        let min = i;
        for(let j=i+1;j<len;j++){
            if(nums[min] > nums[j]){
                min = j;
            }
        }
        [nums[i],nums[min]]=[nums[min],nums[i]];
    }
    return nums;
};
```

## 快速排序

* 找一个目标值，将数组中值与与其比较，小于它的放在一个数组中，大于它的放在另一个数组中；
* 分别对以上两个数组进行同等操作，直至排序完成，然后从左至右再次将其连接起来。

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
    if(nums.length <=1 ) return nums;
    let midIndex = Math.floor(nums.length / 2);
    let mid = nums.splice(midIndex,1)[0];
    let left = [],right = [];
    nums.forEach((e)=>{
        if(e < mid) left.push(e);
        else right.push(e);
    });
    return sortArray(left).concat(mid,sortArray(right));
};
```

# 计数排序

计数排序不是一个比较排序算法，该算法于1954年由 Harold H. Seward提出，通过计数将时间复杂度降到了O(N)。

第一步：找出原数组中元素值最大的，记为max。

第二步：创建一个新数组count，其长度是max加1，其元素默认值都为0。

第三步：遍历原数组中的元素，以原数组中的元素作为count数组的索引，以原数组中的元素出现次数作为count数组的元素值。

第四步：创建结果数组result，起始索引index。

第五步：遍历count数组，找出其中元素值大于0的元素，将其对应的索引作为元素值填充到result数组中去，每处理一次，count中的该元素值减1，直到该元素值不大于0，依次处理count中剩下的元素。

第六步：返回结果数组result。

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var sortArray = function(nums) {
    let max = Math.max(...nums);
    let min = Math.min(...nums);
    let len = max - min + 1;
    let arr = new Array(len).fill(0);
    for(let i of nums){
        arr[i - min]++;
    }
    let result = [];
    for(let i = 0;i < arr.length;i++){
        while(arr[i] > 0){
            result.push(i + min);
            arr[i]--;
        }
    }
    return result;
};
```