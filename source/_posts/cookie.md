---
title: Cookies 详解
date: 2023-11-10 10:13:51
tags:
- 浏览器相关
categories:
- [前端学习笔记]
- [Cookies]
---

# 一、Cookies 是怎么来的？

> 90 年代初期，Netscape 的开发人员 Lou Montoulli 遇到了一个问题 —— 他正在为另一家公司 MCI 开发一个在线商店，该商店会在服务器中存储每个客户购物车中的商品。这意味着人们必须首先创建一个帐户，创建账户需要花费一些时间，存储每个用户购物车的内容也会占用大量存储空间。
> MCI 要求将这些数据存储在客户自己的计算机上。他们还希望客户在没有登录时也能添加商品到购物车。
> 为了解决这个问题，Lou 提出了一个在程序员中已经广为人知的想法：魔力 cookie。

因此，使用cookie最初的想法是：
- 可以在客户端和服务器之间传递少量数据。
- 通常在cookie中村的数据是随机密钥或令牌，只对使用它的软件才有意义。

# 二、Cookies 是怎么工作的？

## Cookies 是由谁创建的

- 虽然浏览器提供了 `document.cookie` 来创建（前端），但大部分时间主要是由 `backend` 在生成 `response` 的时候，设置需要让客户端记住的 `cookie`，并发送到 `client`（后端）。
- 当我们说 `backend` 的时候，可能指的有两种情况：
  - 由 `application` 本身设置的 `cookie`（`Python，JavaScript，PHP，Java`等）
  - 由 `webserver` 或其他反向代理设置的 `cookie`（如: `Nginx，Apache`）
- 通过 `HTTP Response` 的 `headers` 里的 `Set-Cookie` 字段，可设置某个 `cookie` 的键值对，并带上跟这个 `cookie` 有关的一定的参数。

## Cookies 的限制

### 数据存储的限制

|  | Cookies | LocalStorage | SessionStorage |
| --- | --- | --- | --- |
| 容量 | 4KB | 10MB | 5MB |
| 可访问 | 任何窗口 | 任何窗口 | 相同标签页 |
| 过期 | 手动设置 | 永不过期 | 标签页被关闭 |
| 存储位置 | 浏览器和服务端 | 浏览器 | 浏览器 |
| 通过请求发送 | 是 | 否 | 否 |

### 作用域的限制

1. 域（Domain）：出于安全的原因，浏览器**仅允许cookie在一个域名**中可见。
  * 域的作用范围：允许在**该域名及其子域名内**起作用。
2. 路径（Path）：默认为 `/`

> **⚠️注意**：cookie时随着浏览器中的请求一起发送的，不管你是否愿意不该有的想法：通过javascript脚本，从cookie里面将内容读出来，然后塞到AJAX请求的headers里面发送出去。

### 过期时间

* `max-age`: 用于控制 `Cookie` 的存活时间，以秒为单位。
  * 如果 `max-age` 未设置，则 `Cookie` 将在浏览器关闭时过期。
  * 如果 `max-age` 和 `expires` 两者同时出现，那么以 `max-age` 为准；一般来说，两者应该设为同一个时间
值。
* `expires`: 指定 `Cookie` 的过期时间，以 `UTC` 格式的日期字符串为值。

```
Set-Cookie: myfirstcookie=somecookievalue; expires=Tue, 09 Jun 2020 15:46:52 GMT; Max-Age=1209600
```

### 访问控制（Secure, HttpOnly, SameSite）

#### 用于 `HTTPS` 访问控制的 `Secure, HttpOnly`

* 为确保 `Cookie` 只能通过安全的 `HTTPS` 连接访问，并且不能被 `JavaScript` 等客户端脚本访问，我们可以设置这两个属性：
  * `Secure` 只能通过 `HTTPS` 链接发送。
  * `HttpOnly` 只能由服务器访问，不能由浏览器的 `Document.cookie` API访问。

```
Set-Cookie: "id=3db4adj3d; Secure; HttpOnly"
```

#### 用于跨站场景的 SameSite

我们目前的网站中，会出现大量的跨站访问请求（如CDN、Analytics、第三方服务等）。当访问站点A的时候，可能会到站点B、C请求内容，当B、C想设置自己的 `Cookie` 的时候，对A站点的整个会话过程来说，就出现了第三方（third-party）`Cookie`的存储请求。 `SameSite` 是新提出的一个致力于改进 `Cookie` 安全，防止 `CSRF攻击` 以及隐私泄漏的浏览器`Cookie`特性。

* 跨站场景案例
  * 用户访问 https://www.a-example.dev
  * 用户点击按钮发起了一个跨站的请求：https://api.b-example.dev
  * https://api.b-example.dev 设置了一个 Cookie Domain=api.b-example.dev
  * 现在 https://www.a-example.dev 页面得到了一个来自 https://api.b-example.dev 的第三方cookie的设置请求

* 通过设置`SameSite`属性，可以限制`Cookie`在跨站场景下的cookie存储和发送策略：
  * `SameSite` 属有三个值
    * **Strict**
      * 浏览器将会拒绝存储第三方`Cookie`。意味着我方应用只能在本站访问时设置`Cookie`，不能作为第三方给其他站点提供访问。
    * **Lax（默认）**
      * 无论是第一方还是第三方的`Cookie`，浏览器在`Cookie`缺少`SameSite`属性时，都会默认设置为此模式。
      * 在此模式下，针对`Safe HTTP Methods`（安全的HTTP请求，GET/HEAD/OPTIONS/TRACE）可以向第三方发送该`Cookie`，但POST请求就不会。
    * **None**
      * 如果我方是向第三方提供内嵌（`iframe`）应用的，比如WPS编辑器。需要将我方的`Cookie`设置为 `SameSite=None`;`Secure`，否则浏览器在第三方中访问我方应用时，会拒绝存我方的`Cookie`。

| VALUE | INCOMING COOKIE | OUTGOING COOKIE |
| :---: | :---: | :---: |
| Strict | Reject | - |
| Lax | Accept | Send with safe HTTP methods|
| None + Secure | Accept | Send |

#### CORS 场景

在`CORS（Cross-Origin Resource Sharing）`跨源资源共享中，浏览器默认情况下不会发送包含`Cookie`的跨域请求，这是为了防止`跨站点请求伪造（CSRF）`攻击。
要让CORS请求带上`Cookie`，则需要以下步骤：

* 访问第一方页面的时候，服务端返回这两个响应头
  * `Access-Control-Allow-Origin`
  * `Access-Control-Allow-Credentials`
* `fetch()` 加上 `credentials: "include"` 参数

# Cookies 的常见使用场景

## 会话跟踪

* 当你向网站发出登录请求时，服务器检查你的用户名和密码，创建并存储会话，生成唯一的会话ID，然后发送回带有该会话ID的cookie
* 当你向网站发出其他请求时，浏览器将带有会话ID的`cookie`发送回服务器。
* 服务器使用你的会话ID检查会话，如果是已登录并有权访问数据的时候，返回请求的数据
* 当你要离开网站时，将带有会话ID连同注销请求发送到服务端，服务端删除会话，并告知浏览器删除去相应session id的cookie

一个典型的session cookie会类似这样

```
Set-Cookie: sessionid=sty1z3kz1am3xdjv348mqwlx4ginpt6c; expires=Tue, 09 Jun 2020 15:46:52 GMT; HttpOnly; Max-Age=1209600; Path=/; SameSite=Lax
```

## 单点登录

## 内嵌页面

# 常见安全问题

## 中间人攻击（MitM）

{%asset_img mitm.png # mitm%}

## 跨站点脚本攻击（XSS）

{%asset_img xss.png # xss%}

## 跨站点请求伪造攻击（CSRF）

{%asset_img csrf.png # csrf%}