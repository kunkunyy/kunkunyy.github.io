---
title: 算法学习笔记（二）二分查找
date: 2022-02-12 21:07:45
tags:
- 算法学习
categories:
- [算法学习笔记]
- [二分查找]
---

# 一、算法基础

* 使用前提：**有序数组**且**数组中无重复元素**。
* 二分查找涉及的很多的边界条件，逻辑比较简单，但就是写不好。
* 写二分法，区间的定义一般为两种，左闭右闭即[left, right]，或者左闭右开即[left, right)。

# 二、算法框架

```js
function binarySearch(nums, target) {
    let left = 0, right = ...;
    while(...) {
        int mid = (right + left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }else{

        }
    }
    return ...;
}
```
* 计算 mid 时需要技巧防止溢出，建议写成: mid = left + (right - left) / 2。
* ...标记的部分，就是可能出现细节问题的地方，当你见到一个二分查找的代码时，首先注意这几个地方。

# 三、二分法第一种写法

* 第一种写法，我们**定义 target 是在一个在左闭右闭的区间里，也就是[left, right]** （这个很重要非常重要）。
* 区间的定义这就决定了二分法的代码应该如何写，因为定义target在[left, right]区间，所以有如下两点：
    * while (left <= right) 要使用 <= ，因为left == right是有意义的，所以使用 <=
    * if (nums[middle] > target) right 要赋值为 middle - 1，因为当前这个nums[middle]一定不是target，那么接下来要查找的左区间结束下标位置就是 middle - 1

# 四、二分法第二种写法

* 如果说定义 target 是在一个在左闭右开的区间里，也就是[left, right) ，那么二分法的边界处理方式则截然不同。
* 有如下两点：
    * while (left < right)，这里使用 < ,因为left == right在区间[left, right)是没有意义的
    * if (nums[middle] > target) right 更新为 middle，因为当前nums[middle]不等于target，去左区间继续寻找，而寻找区间是左闭右开区间，所以right更新为middle，即：下一个查询区间不会去比较nums[middle]

# 五、总结

* 区间的定义就是不变量，那么在循环中坚持根据查找区间的定义来做边界处理，就是循环不变量规则。