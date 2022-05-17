---
title: Markdown学习笔记
date: 2021-03-12 18:55:11
categories:
- markdown
tags:
- markdown
- study notes
comment: true
---
 之前只是用markdown写过博客并没有系统的去了markdown语法，现在将创建博客后的第一篇文章用来记录我学习markdown的经历以及对于知识点的理解。

# markdown官方介绍和个人理解

## 菜鸟教程介绍
* Markdown 是一种轻量级标记语言，它允许人们使用易读易写的纯文本格式编写文档。
* Markdown 语言在 2004 由约翰·格鲁伯（英语：John Gruber）创建。
* Markdown 编写的文档可以导出 HTML 、Word、图像、PDF、Epub 等多种格式的文档。
* Markdown 编写的文档后缀为 .md, .markdown。

## 个人理解
* Markdown是一种纯文本标记语言
* 语法简单，容易上手
* 按照语法，自动排版，格式易操作
* ......

# 语法学习

## 标题
标题有两种书写格式，推荐使用第一种。

### # 号标识标题
* 用法：使用 # 号可表示 1-6 级标题，每升一级添加一个 # 号
* 易错点：最后一个 # 号后空一格
* 示例：
```markdown
# 一级标题
## 二级标题
```
* 结果
> {% asset_img head1.png # 标识标题 %}  

### = 和 - 标记标题
* 用法：在对应文字下方输入 = （-）即可
* 示例：
```markdown
一级标题
=
二级标题
-
```
* 结果
> {% asset_img head1.png # 标识标题 %}


## 段落格式

### 换行

* 实现方式：
    - 句子末尾空两格然后回车
    - 空一行实现换行

### 字体
```markdown
斜体
*猫猫头*
_猫猫头_
```
```markdown
粗体
**猫猫头**
__猫猫头__
```
```markdown
粗斜体
***猫猫头***
___猫猫头___
```

### 分割线
* 实现方式：
    - 在一行中用**三个以上**的星号、减号、底线来建立一个分隔线。
    - **行内不能有其他东西**。
    - 可以在星号或是减号中间插入空格。

### 删除线（在文字上添加删除线）
* 实现方式：
```markdown
~~猫猫头的小窝~~
```
* 效果展示：
~~猫猫头的小窝~~

### 下划线
* 实现方式：
```markdown
<u>猫猫头的小窝</u>
```
* 效果展示：
<u>猫猫头的小窝</u>

### 脚注
* 补充说明词语或者句子内容。
* 可用于标注出处，翻译等。
* 实现方式：
```markdown
“[^情不知所起，一往而深。]”
[^情不知所起，一往而深。]:感情不知道什么时候就开始了，而且愈来愈深厚。
```
* 效果展示：
“[情不知所起，一往而深。]”
[情不知所起，一往而深。]: 感情不知道什么时候就开始了，而且愈来愈深厚。

## 列表

### 有序、无序列表

* 实现方式：
    - 有序：数字并加上 . 号来表示。
    - 无序：星号(*)、加号(+)或是减号(-)作为列表标记，且标记后面要添加一个空格。

### 列表嵌套

* 实现方式：在子列表中的选项前面添加四个空格（或者一个Tab键）
* 例子：
```markdown
1. 猫猫头：
    - 猫猫头的代码库
    - 猫猫头的图片库
```
> {% asset_img list.png # 嵌套列表 %}

## 区块

* 实现方式：在段落开头使用 > 符号，后面紧跟一个空格符号。
* 嵌套使用：
    - 区块嵌套：每增加一个 > 符号增加一层。
    - 区块嵌套列表：
    ```markdown
    > + 猫猫头的代码库
    > + 猫猫头的图片库
    ```
    > + 猫猫头的代码库
    > + 猫猫头的图片库
    - 列表嵌套区块：
    ```markdown
    1. 猫猫头
        > 猫猫头的图片库
        > 猫猫头的代码库
    ```
    1. 猫猫头
        > 猫猫头的图片库
        > 猫猫头的代码库

## 代码

* 实现方式：
    - 代码区块：
        + 使用 4 个空格或者一个制表符（Tab 键）。
        + 用 ``` 包裹一段代码，并指定一种语言，也可以不指定（推荐）。

## 链接

* 实现方法：`[链接名称](链接地址)`或者`<链接地址>`
* 例子：`[百度](www.baidu.com)`

## 图片

* 实现方式：
```
![alt 属性文本](图片地址)
![alt 属性文本](图片地址 "可选标题")
```
    - 开头一个感叹号 !
    - 接着一个方括号，里面放上图片的替代文字
    - 接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上选择性的 'title' 属性的文字。
* 补充：可以使用`<img>`标签，用于指定图片的高度与宽度。

## 表格

* 实现方式：
    - 使用 | 来分隔不同的单元格，使用 - 来分隔表头和其他行。
    - -: 设置内容和标题栏居右对齐。
    - :- 设置内容和标题栏居左对齐。
    - :-: 设置内容和标题栏居中对齐。
* 示例：
```markdown
| 左对齐 | 右对齐 | 居中对齐 |
| :-----| ----: | :----: |
| 单元格 | 单元格 | 单元格 |
| 单元格 | 单元格 | 单元格 |
```
> {% asset_img form.png # 表格 %}

# 总结

这里就暂时总结了markdown一些基本用法，能够满足用来写博客就可以了，后面继续加油吧！有什么问题可以留言哦！