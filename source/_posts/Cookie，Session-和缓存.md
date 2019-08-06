---
title: Cookie，Session 和缓存
date: 2019-08-06 16:24:22
tags:
categories: 前端探索
---

## Cookie
1. Cookie
1\. 服务器通过 Set-Cookie 头给客户端一串字符串
2\. 客户端每次访问相同域名的网页时，必须带上这段字符串
3\. 客户端要在一段时间内保存这个 Cookie
4\. Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
5\. [大小大概在 4kb 以内](https://stackoverflow.com/questions/640938/what-is-the-maximum-size-of-a-web-browsers-cookies-key "null")

2. Cookie 如何设置过期时间？如何删除 Cookie？
+ 用 `max-age` 和 `expires` 都可以设置 cookie 的过期时间。
`max-age` 设置的是一个时间段，而 `expires` 设置的是时间点。
例：设置 cookie 在 60 秒后过期：
```
document.cookie = "name=cookie; max-age=60"
// 或
var currDate = new Date()
currDate.setTime(currDate.getTime()+60*1000) 
document.cookie = "name=cookie; expires=" + currDate.toGMTString()
```

+ 将 `max-age` 设置为零或负数，即可将 cookie 删除。
`expires` 设置的时间内是 cookie 的有效期，将 `expires` 设置为比当前时间早的值，cookie 也会被删除。
```
document.cookie="name=cookie; max-age=0"
// 或
document.cookie="name=cookie; max-age=-1"
```

3. **Cookie 存在的问题**：
用户可以随意篡改 Cookie；可以用 Session 来解决这个问题。

4. 建议：前端永远不要去读 / 写 Cookie。

## Session
1. Session
1\.  将 SessionID（随机数）通过 Cookie 发给客户端
2\.  客户端访问服务器时，服务器读取 SessionID
3\.  服务器有一块内存（哈希表）保存了所有 session
4\.  通过 SessionID 我们可以得到对应用户的隐私信息，如 id、email
5\.  这块内存（哈希表）就是服务器上的所有 session

服务器通过 cookie 给用户一个 sessionID（随机数），sessionID 对应服务器里面的一小块内存，每次用户访问服务器时，服务器就通过 sessionID 去读取对应的 session，然后知道用户的隐私信息。

2. Session 也可以用 【LocalStorage + 查询参数】实现（不基于 Cookie 实现）。

## LocalStorage
1. LocalStorage 是 html5 提供的 API，它是浏览器里的 hash 表，特点如下：
1\. LocalStorage 跟 HTTP 无关
2\. HTTP 不会带上 LocalStorage 的值
3\. 只有相同域名的页面才能互相读取 LocalStorage（没有同源那么严格）
4\. 每个域名的 localStorage 最大存储量为 5Mb 左右（每个浏览器不一样）
5\. 常用场景：记录有没有提示过用户（没有用的信息，不能记录密码）
6\. LocalStorage 永久有效，除非用户清理缓（持久化存储）
7\. LocalStorage 常用于 **跨页面的存储** 和 **持久化存储** 。
2. 属性
+ `localStorage.setItem('name', value)`
+ `localStorage.getItem('name')`
+ `localStorage.clear()`

查看位置：开发者工具 —— Application —— Local Storage

## SessionStorage（会话存储）
1. SessionStorage
1、2、3、4 同上
5\.  SessionStorage 在用户关闭页面（会话结束）后就失效。

2. SessionStorage 与 Session 没有关系。

## HTTP 缓存控制 
### Cache-Control
1. 功能
Cache-Control 可以让浏览器在一段事件内不发请求，直接用本地的硬盘或内存作为响应，这样速度会极快。

2. 用法
网页文件过大（如 js 和 css 文件）时请求时间会比较长，可以用 Cache-Control 对 Web 进行性能优化：
`response.setHeader('Cache-Control', 'max-age=30')` 
即 30 秒内同样的 URL 不再重新发送请求，直接从内存里获取。

2. 为什么首页不能设置 Cache-Control ？
这段时间内如果页面更新了，用户没有办法获取到页面的最新版本。
所以首页不要设置缓存，尤其是 html 页面。

3. 文件更新
Cache-Control 在实际使用中一般把时间设置的长一些，比如 10 年。
如果期间这些文件有更新，只需要在入口处（html）的文件路径中加查询参数（改变 URL）即可：`<link rel="stylesheet" href="./css/default.css?v=2">`，
`<script src="./js/main.js?v=3></script>`，浏览器就不再使用之前的缓存。

### Expire
1. Expires 设置过期时间点，例：
`response.setHeader('Expires', 'Fri, 05 Jul 2019 04:47:35 GMT')`
响应头中会出现 Expires 和时间，即在此日期之后，响应过期。
例：`Expires: Wed, 21 Oct 2015 07:28:00 GMT`

2. 如何获取当前时间
`let t = new Date()` // 获取中国标准时间
`t.toGMTString()` // 转为格林威治时间（世界时钟）

3. 如果在 Cache-Control 响应头设置了 "max-age" 或者 "s-max-age" 指令，那么 `Expires` 头会被忽略。

### ETag
1. MD5
MD5 是摘要算法，一般用于校验文件（nodejs 中有 MD5 的库）。

2. ETag
客户端初次发出请求时，服务器会在响应头中给出一个标记 ETag（由 MD5 生成），如：
`Etag: 440e570c372631aa20b9c778ad9e7273`
当等下次客户端发请求时会在请求头中带上这个 MD5：
`If-None-Match: 440e570c372631aa20b9c778ad9e7273`
服务器对比两次的 MD5，如果没有改变，则不需要下载，返回状态 304（Not Modified）。

### last-modified
+ 当浏览器初次发请求时，服务器会在响应头中附带一个 Last-Modified 的属性，标记此文件在服务期端最后被修改的时间，例如：
`Last-Modified: Tue, 24 Feb 2009 08:01:04 GMT`
客户端再次发请求时，根据 HTTP 协议的规定，浏览器会在请求头中带上 If-Modified-Since，询问该时间之后文件是否有被修改过：
`If-Modified-Since: Tue, 24 Feb 2009 08:01:04 GMT`
如果服务器端的资源没有变化，则直接返回 304（Not Modified）状态码，返回内容为空，这样就节省了传输数据量。

+ 当服务器端代码发生改变或者重启服务器时，浏览器再发请求，则会像第一次一样重新下载资源。从而保证不向客户端重复发出资源，也保证当服务器有变化时，客户端能够得到最新的资源。

## 经典问答
1. **Session 与 Cookie 的关系**：
一般来说，Session 是基于 Cookie 来实现的。
（因为 session 必须将 sessionID 放在 Cookie 里发给客户端）

2. **Cookie 和 Session 的区别**：
Cookie 保存在客户端，每次都随请求发送给服务器。
Session 保存在服务器的内存里，其 Session ID 是通过 Cookie 发送给客户端的。

3. **Cookie 和 LocalStorage 的区别**：
+ 客户端每次向服务器发请求都会携带 Cookie，而 LocalStorage 不会随 HTTP 发给服务器。
+ Cookie 的大小一般在 4k 以内，LocalStorage 的最大存储容量是 5M 左右。LocalStorage 的大小限制比 Cookie 要大很多。

4. **LocalStorage 和 SessionStorage 的区别**：
LocalStorage 永久有效，除非用户清理缓存；
SessionStorage 在用户关闭页面（会话结束）后就失效。

5. **Cache-Control 与 Expire 的区别**？
Cache-Control 设置的是事件长度，Expire 设置的是时间点。
如果客户端本地时间错乱，Expire 指令可能发挥不了作用，因此优先使用 Cache-Control。

6. **Cache-Control 与 ETag（304） 的区别**？
Cache-Control 没有请求；
ETag（304）有请求，有响应，但是响应没有第四部分（不下载）。
