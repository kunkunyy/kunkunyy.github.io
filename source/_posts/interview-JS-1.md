---
title: 前端面试题-JS篇（一）
date: 2022-02-27 16:18:12
tags:
- JS学习
categories:
- [前端面试题]
- [JS]
---

# 1、浏览器本地存储是怎样的？

* Cookie存储：明文，大小限制 4k 等。
* localStorage：持久化存储方式之一，不用在两端之间传输，且限制大小为10M
* sessionStorage，会话级存储方式，浏览器关闭立即数据丢失
* indexDb，浏览器端的数据库

# 2、AJAX的优缺点

* 优点：
    * 可以无刷新页面与服务端进行通信
    * 允许你根据用户事件来更新部分页面内容
* 缺点：
    * 没有浏览历史，不能回退
    * 存在跨域问题（同源）
    * SEO不友好（爬虫获取不到信息）

# 3、防抖和节流

* 防抖：用户触发事件过于频繁，只需要最后一次事件的结果。

```js
let el = document.querySelector("input");
let t = null;
el.oninput = function(){
    if(t !== null){
        clearTimeout(t);
    }
    t = setTimeout(()=>{
        console.log(this.value);
    },500)
}
//=======封装========
function antiShaking(fn,delay){
    let t = null;
    return function(){
        if(t !== null){
            clearTimeout(t);
        }
        t = setTimeout(()=>{
            fn.call(this);
        },delay);
    }
}
```

* 节流：控制高频事件的执行次数。

```js
let flag = true;
window.onscroll = function(){
    if(flag){
        setTimeout(()=>{
            console.log("Hello world");
            flag = true;
        },500);
    }
}
//=======封装========
function throttle(fn,delay){
    let flag = true;
    return function(){
        if(flag){
            setTimeout(()=>{
                fn.call(this);
                flag = true
            },delay)
        }
        flag = false;
    }
}
```

# 4、call、apply、bind

* 这三个方法均为函数对象上的方法。
* 三个方法均用于改变函数中this指向。
* 区别：
    * call vs apply ： 传参形式不一样；
    * call vs bind ： call改变this指向后会立即调用，bind不会。

```js
let dog = {
    food : "骨头",
    name : "汪汪",
    eat(food1,food2){
        console.log("我爱吃" + food1 + food2);
    },
    sayName(){
        console.log("我是" + this.name);
    }
}
let cat = {
    name: "喵喵",
    food: "鱼",
}

dog.say(cat);//我是喵喵
dog.eat.call(cat,"鱼","猫粮");//我爱吃鱼猫粮
dog.eat.apply(cat,["鱼","猫粮"]);//我爱吃鱼猫粮

let fn = dog.eat.bind(cat,"鱼","猫粮");
fn();//我爱吃鱼猫粮
```