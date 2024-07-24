---
title: 前端面试题-CSS篇（二）
date: 2022-02-27 14:28:50
cover: /img/article/winter-pools.webp
tags:
- JS学习
categories:
- [前端面试题]
- [CSS]
---

# 1、自适应布局

* 使用flex；
* 左侧浮动或者绝对定位，右侧margin撑开使用div包含，靠-margin形成BFC；

# 2、display相关参数

* block：块级元素；
* inline：等行元素；
* table：table类型
* flex：弹性布局，子元素的float、clear和vertical-align属性将失效
* inline-block：行内块元素

# 3、display:none 和 visibility:hidden 的区别

* display:none 隐藏对应的元素，**在文档布局中不再给它分配空间**，它各边的元素会合拢，就当他从来不存在。
* visibility:hidden 隐藏对应的元素，但是**在文档布局中仍保留原来的空间**。

# 4、position的absolute 与fixed共同点与不同点

* 共同点：
    * 改变行内元素的呈现方式，display被置为block；
    * 让元素脱离普通流，不占据空间；
    * 默认会覆盖到非定位元素上不同点；
* 不同点：
    * absolute的“根元素”是可以设置的，而 fixed 的“根元素”固定为浏览器窗口。

# 5、position的值分别是相对于谁进行定位的

* absolute：相对于static定位以外的第一个祖先元素。
* fixed：相对于浏览器窗口进行定位。
* relative：相对于其在普通流中的位置进行定位
* static，默认值，没有定位，元素出现在正常的流中。
* inherit：从父元素继承position。

# 6、为什么要初始化CSS

* 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。

# 7、canvas在标签山设置宽高和在style中设置宽高有什么区别

* 标签的width和height是画布实际宽度和高度；
* style的是canvas在浏览器中被渲染的高度和宽度。

# 8、CSS预处理器

* Sass，Less，Postcss
* 原理：将类CSS语言通过Webpack编译转成浏览器可读的真正CSS。

# 9、哪些属性可以继承

* 可继承：font-size, font-family, color等
* 不可继承border, padding, margin, width, height

# 10、::before 和 :after 中双冒号和单冒号有什么区别

* 单冒号用于CSS3伪类，双冒号用于CSS3伪元素。
* CSS 伪类用于向某些选择器添加特殊的效果。
* CSS 伪元素用于将特殊的效果添加到某些选择器。

# 11、什么是响应式设计？其基本原理是什么？

* 响应式网站设计(Responsive Web design)是一个网站能够兼容多个终端，而不是为每一个终端做一个特定的版本。
* 基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理。
* 页面头部必须有meta声明的viewport。

# 12、浏览器是怎样解析CSS选择器的？

* CSS选择器的解析是从右向左解析的。若从左向右的匹配，发现不符合规则,需要进行回溯，会损失很多性能。
* 若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。
* 在CSS解析完毕后，需要将解析的结果与DOM Tree 的内容一起进行分析建立一棵Render Tree，最终用来进行绘图。

# 13、CSS优化、提高性能的方法有哪些？

* 避免过度约束
* 避免后代选择符避免链式选择符使用紧凑的语法
* 避免不必要的命名空间
* 避免不必要的重复
* 最好使用表示语义的名字。一个好的类名应该是描述他是什么而不是像什么
* 避免! important，可以选择其他选择器
* 尽可能的精简规则，你可以合并不同类里的重复规则

# 14.position跟display、overflow、float这些特性相互叠加后会怎么样?

* display属性规定元素应该生成的框的类型;
* position 属性规定元素的定位类型;
* float属性是一种布局方式，定义元素在哪个方向浮动。
* 类似于优先级机制: 
    * position: absolute/fixed优先级最高,有他们在时, float不起作用, display值需要调整。
    * float或者absolute定位的元素，只能是块元素或表格。
