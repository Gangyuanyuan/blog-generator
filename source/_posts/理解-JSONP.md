---
title: 理解 JSONP
date: 2019-08-03 06:23:28
tags: Vue
categories: 前端探索
---

## 前置知识
1. 页面与服务器的交互
在 AJAX 出现之前，页面与服务器的交互是怎么完成的呢？
+ 通过提交 `form` 表单，但页面会被刷新，用户体验不好；
+ 通过嵌入 `iframe` 页面，实现只在 iframe 窗口刷新。
+ 用 `img` 发请求，但 img 只能知道成功或失败，不能获得更多的数据。
+ 用 `script` 发请求，通过 SRJ（Server rendered javascript），即服务器返回的 javascript，实现无刷新 局部更新页面内容。

2. `a`、`img`、`link`、`script` 等都可以发请求；
`script` 标签只有放在 `body` 或 `head` 里才会发请求，且只能发 `get` 请求。

3. `script` 是不受域名限制的，任何一个网站都可以使用另一个网站的 JS。

## JSONP
### 自己理解 JSONP
+ JSONP 要解决的问题是两个网站之间如何交流。
+ 因为 script 标签不受域名限制，所以 JSONP 用 `script` 标签发请求（跨域 SRJ）。
>**JSONP**
>请求方：frank.com 的前端（浏览器）
>响应方：jack.com 的后端（服务器）
>1、请求方创建 `script` 标签，`src` 指向响应方，同时传一个查询参数 
`?callbackName===xxx`。
>2、响应方根据查询参数 `callbackName`，构造形如 `xxx.call(undefined, '需要的数据')` / `xxx('需要的数据')` 的函数调用的响应。
>3、浏览器接收到响应，就会执行 `xxx.call(undefined, '需要的数据')`。
>4、那么请求方就得到了需要的数据。
>这就是 JSONP。
> 
>**约定**：
>callbackName --> callback
>xxx --> 随机数

+ JSONP **核心**：script + callback 参数

### JSONP 定义
JSONP（JSON with Padding）是 JSON 的一种“使用模式”，可用于解决主流浏览器的跨域数据访问的问题。其核心思想是利用 JS 标签里面的跨域特性进行跨域数据访问，在 JS 标签里存在的是一个跨域的 URL，实际执行的时候通过这个 URL 获得一段字符串，这段返回的字符串必须是一个合法的 JS 调用，通过 EVAL 这个字符串来完成对获得数据的处理。

### JSONP 为什么不支持 POST 请求
因为 JSONP 是通过动态创建 `script` 标签实现的，而 `script` 的 `src` 指定的 `url` 只能通过 `get` 方式获取，所以不支持 `post` 请求。