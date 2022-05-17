---
title: Vue 学习笔记（二）Vue生命周期
date: 2022-02-03 19:36:30
tags:
- Vue学习
categories:
- [前端学习笔记]
- [VUE]
- [Vue生命周期]
---

# 基本内容和图解

* VUE生命周期共分为八个阶段：
    * 创建前/后、载入前/后、更新前/后、销毁前/后。

* 创建前/后 :
    * 在beforeCreate阶段，vue实例的挂载元素el和数据对象data都为undefined，还未初始化。
    * 在created阶段，vue实例的数据data有了，el还没有
* 载入前/后：
    * 在beforeMount阶段，vue实例的$el和data都初始化了,但还没有挂载之前都是虚拟的demo阶段,data.message还未替换。
    * 在mounted阶段,vue实例挂载完后,data.message成功渲染.
* 更新前/后：
    * 当data变化时,户触发beforeUpdate和update方法。
* 销毁前/后：
    * 在执行destroy方法后，对data的改变不会再触发周期函数，说明此时vue实例已经结束了事件监听以及和dom的绑定，但是dom结构依然存在。

{%asset_img pic2.png # tu1%}

# 面试题

## 1、什么是vue生命周期？
vue实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载DOM→渲染、更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期。

## 2、vue生命周期的作用是什么？
生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程中更容易形成好的逻辑

## 3、vue生命周期总共有几个阶段？
总共可以分8个阶段：创建前/后、载入前/后、更新前/后、销毁前/后

## 4、第一次页面加载会触发哪几个钩子?
第一次页面加载时会触发beforeCreate、created、beforeMount、mounted这几个钩子

## 5、请列举出3个Vue常用的声明周期钩子函数
* created：实例已经创建完成之后调用，在这一步，实例已经完成数据观测、属性和方法的运算，watch、event事件回调，然而，挂载阶段还没有开始，$el属性目前还不可见
* mounted：el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子,如果root实例挂在了一个文档内元素，当mounted被调用时vm.$el也在文档内。
* activated：keep-alive组件激活时调用

## 6、DOM渲染在那个周期中已完成？
DOM渲染在mounted中就已经完成了