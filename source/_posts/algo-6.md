---
title: 算法学习笔记（六）二叉树的层序遍历（理论）
date: 2022-03-13 16:16:12
series: 算法学习
cover: /img/article/waterfall.webp
tags:
- 算法学习
categories:
- [算法学习笔记]
- [二叉树]
- [层序遍历]
---

# 理论基础

* 层序遍历一个二叉树就是：**从左到右一层一层的去遍历二叉树。**
* 需要借用一个辅助数据结构即队列来实现，**队列先进先出，符合一层一层遍历的逻辑，而是用栈先进后出适合模拟深度优先遍历也就是递归的逻辑。**

{%asset_img pic1.jif # tu1%}

# 题目汇总

{% post_link lc-102 102. 二叉树的层序遍历 %}
{% post_link lc-107 107. 二叉树的层序遍历 II %}
{% post_link js-5-1 Undefined、Null、Boolean %}