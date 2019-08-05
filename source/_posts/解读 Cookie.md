---
title: 解读 Cookie
date: 2019-08-05 19:16:07
tags:
categories: 前端探索
---

## Cookie
我们知道，响应头中包括 Cookie 和 Cache-Control（缓存控制），我们今天就先来解读一下 Cookie。
### 设置 Cookie
Cookie 是在响应头中设置的，格式为：`Set-Cookie: <cookie-name>=<cookie-value>`，例设置一个用户登录的 Cookie：
```
response.setHeader('Set-Cookie', `sign_in_email=${email}`)
```
### Cookie 特点
+ 服务器通过 Set-Cookie 响应头设置 Cookie
+ 浏览器得到 Cookie 之后，之后每次请求（相同语言相同域名下）都会带上 Cookie
+ 客户端要在一段时间内保存这个 Cookie
+ Cookie 默认在用户关闭页面后就失效，后台代码可以任意设置 Cookie 的过期时间
+ Cookie [大小大概在 4kb 以内](https://stackoverflow.com/questions/640938/what-is-the-maximum-size-of-a-web-browsers-cookies-key "null")

### 关于 Cookie 的几个问题
1\. 用户在 Chrome 登录得到了 Cookie，用 Safari 访问，Safari 会带上 Cookie 吗？
答：不会。
2\. Cookie 存在哪？
答：Windows 系统存在 C 盘的一个文件里。
3\. Cookie 能造假吗？
答：Cookie 可以被篡改，开发者模式 —— Application —— Cookie.
4\. Cookie 有有效期吗？
答：默认有效期 20 分钟左右，不同浏览器策略不同。后端也可以强制设置有效期。
5\.  Cookie 遵守同源策略吗？
答：也有，不过跟 AJAX 的同源策略稍微有些不同。
当请求 qq.com 下的资源时，浏览器会默认带上 qq.com 对应的 Cookie，不会带上 baidu.com 对应的 Cookie。
当请求 v.qq.com 下的资源时，浏览器不仅会带上 v.qq.com 的 Cookie，还会带上 qq.com 的 Cookie。
另外 Cookie 还可以根据路径做限制，但这个功能用得比较少。

### Cookie 如何设置过期时间
用 `max-age` 和 `expires` 都可以设置 Cookie 的过期时间。
`max-age` 设置的是一个时间段，而 `expires` 设置的是时间点。
例：设置 cookie 在 60 秒后过期：
```
document.cookie = "name=cookie; max-age=60"
// 或
var currDate = new Date()
currDate.setTime(currDate.getTime()+60*1000) 
document.cookie = "name=cookie; expires=" + currDate.toGMTString()
```

### 如何删除 Cookie
将 `max-age` 设置为零或负数，即可将 cookie 删除。
`expires` 设置的时间内是 cookie 的有效期，将 `expires` 设置为比当前时间早的值，cookie 也会被删除。
```
document.cookie="name=cookie; max-age=0"
// 或
document.cookie="name=cookie; max-age=-1"
```

### Cookie 存在的问题
用户可以随意篡改 Cookie，把 Cookie 直接暴露给用户缺乏安全性，
可以用 Session 来解决这个问题。
