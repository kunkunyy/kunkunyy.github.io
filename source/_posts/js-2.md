---
title: JavaScript学习笔记（二）HTML中的JavaScript
date: 2021-07-20 12:01:34
tags:
- JS学习
categories:
- [前端学习笔记]
- [JS]
---

# &lt;script&gt;元素

## &lt;script&gt;元素拥有的属性

| 属性 | 状态 | 作用 | 备注 |
| :----: | :----: | :----: | :----: |
| async | 可选 | 立即开始下载脚本，但不能阻止其他页面动作 | **只对外部脚本文件有效** |
| charset | 可选 | 使用 src 属性指定的代码字符集 | 很少使用，大多数浏览器不在乎其值 |
| crossorigin | 可选 | 配置相关请求的CORS（跨源资源共享）设置 | 默认不使用CORS |
| defer | 可选 | 表示脚本可以延迟到文档完全被解析和显示之后再执行 | 只对外部脚本文件有效 |
| integrity | 可选 | 允许比对接收到的资源和指定的加密签名以验证子资源完整性 | 用于确保内容发布网络不会提供恶意内容 |
| language | **废弃** | 用于表示代码块中的脚本语言 |  |
| src | 可选 | 表示包含要执行的外部文件 |  |
| type | 可选 | 标识代码块中脚本语言的内容类型 | 代替language |

## JavaScript代码引入方式

### 直接在网页中嵌入JavaScript代码

* 示例：
```js
<script>
    function message(){
        console.log("Hello World!")
    }
</script>
```

* &lt;script&gt;内的代码会被从上到下解释；
* 在使用行内 JavaScript 代码时，要注意代码中**不能出现字符串&lt;/script&gt;；**
* **浏览器解析行内脚本的方式决定了它在看到字符串&lt;/script&gt;时，会将其当成结束的&lt;/script&gt;标签。**想避免这个问题，只需要转义字符“\”。

```js
//错误
<script>
    function message(){
        console.log("</script>");
    }
</script>

//正确写法
<script>
    function message(){
        console.log("<\/script>");
    }
</script>
```

### 通过在网页中包含外部JS文件

* **前提：在&lt;script&gt;标签中使用src属性引入外部文件。**
* 不同文档的引入方式：
```js
//HTML
<script src="message.js"></script>
//XHTML
<script src="message.js"/>
```

* 外部JS文件的扩展名是js，不是必须的。
    * 浏览器不会检查所包含JS文件的扩展名
    * 为使用服务器端脚本语言动态生成JS代码/在浏览器中将JS扩展语言转译为JS提供了可能性。
* 使用src属性后&lt;script&gt;标签中不应该包含其他JS代码
    * **如果都存在，则会忽略行内代码同时下载并执行脚本文件**
* src属性可以是完整的URL
    ① src属性向指定路径发送一个**GET请求**
    ② 获取相应资源
* 该GET请求：
    * **不受浏览器同源策略限制**
    * **受父页面HTTP/HTTPS协议的限制**

#### 标签位置

* 将所有JS引用放在&lt;body&gt;元素中
```html
<!DOCTYPE html> 
<html> 
    <head> 
    <title>Hello World</title> 
    </head> 
    <body> 
        <!-- 这里是页面内容 --> 
        <script src="message1.js"></script> 
        <script src="message2.js"></script> 
    </body> 
</html>
```

#### 推迟执行脚本

* &lt;script&gt;的defer属性：表示脚本会被延迟到整个页面都解析完毕后再运行。
* 只对外部脚本文件有效
* H5规范要求脚本因该按照它们出现的顺序执行
* **建议：将需要推迟的脚本放在页面底部**

```html
<!DOCTYPE html> 
<html> 
    <head> 
    <title>Hello World</title>
    <script defer src="message1.js"></script>
    <script defer src="message2.js"></script> 
    </head> 
    <body> 
        <!-- 这里是页面内容 --> 
    </body> 
</html>
```

#### 异步执行脚本

* H5为&lt;script&gt;元素定义了async属性，表示：**脚本不需要等待其他脚本，同时也不阻塞文档渲染**
* 只适用于外部脚本
* 添加 async 属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到该异步脚本下载和执行后再加载其他脚本
* **异步脚本不应该再加载期间修改DOM**
* 使用async后页面不能使用document.write

#### 动态加载脚本

* 向DOM中动态添加script元素
```javascript
let script = document.createElement('script');
script.src = 'message.js';
document.head.appendChild(script);
```

* 默认为异步方式加载，可以设置同步加载
* 在文档头部显式声明，进而让预加载器知道动态请求文件的存在
```html
<link rel="preload" href="message.js">
```

## 外部文件优点

* 可维护性：用一个目录保存所有JS文件，更容易维护
* 缓存：根据特定的设置缓存所有外部链接的JS文件
* 适应未来

## 文档模式

### 作用

告诉浏览器**以哪种模式呈现**，**如何解析文档**，也就是说两种模式主要影响CSS内容的呈现，某些情况下也会影响JavaScript的执行。

### 类型

* 混杂模式：向后兼容的解析方式
* 标准模式：要求严格的DTD，根据web标准去解析页面的模式
* 准标准模式：准标准模式与标准模式非常接近，它们的差异几乎可以忽略不计

### 模式开启方法

1、 混杂模式

**混杂模式在所有浏览器中都以省略文档开头的doctype声明作为开关**

2、 标准模式

```html
<!-- HTML 4.01 Strict --> 
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" 
"http://www.w3.org/TR/html4/strict.dtd"> 

<!-- XHTML 1.0 Strict --> 
<!DOCTYPE html PUBLIC 
"-//W3C//DTD XHTML 1.0 Strict//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"> 

<!-- HTML5 --> 
<!DOCTYPE html>
```

3、 准标准模式

```html
<!-- HTML 4.01 Transitional --> 
<!DOCTYPE HTML PUBLIC 
"-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd"> 

<!-- HTML 4.01 Frameset --> 
<!DOCTYPE HTML PUBLIC 
"-//W3C//DTD HTML 4.01 Frameset//EN" 
"http://www.w3.org/TR/html4/frameset.dtd"> 

<!-- XHTML 1.0 Transitional --> 
<!DOCTYPE html PUBLIC 
"-//W3C//DTD XHTML 1.0 Transitional//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 

<!-- XHTML 1.0 Frameset --> 
<!DOCTYPE html PUBLIC 
"-//W3C//DTD XHTML 1.0 Frameset//EN" 
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">
```