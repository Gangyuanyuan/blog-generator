---
title: 同源策略与 CORS 跨域
date: 2019-08-04 08:35:55
tags: 
categories: 前端探索
---

## 同源策略
同源策略：浏览器出于安全方面的考虑，只允许与本域下的接口交互。不同源的客户端脚本在没有明确授权的情况下，不能读写对方的资源。
简单来说，同源策略就是：AJAX 只能向同源网址发出 HTTP 请求，同源网址即协议、域名和端口都相同；如果发出跨域请求，则会出现报错。

## CORS 跨域
### CORS 定义
+ CORS（Cross-Origin Resource Sharing）即跨源资源共享，属于浏览器技术规范，用来规避浏览器的同源策略。
+ CORS 除了支持 GET 请求外，还支持其他的 HTTP 请求（JSONP 只支持 GET 请求）。

### CORS 用法
在被请求方的后台（server.js）加一个响应头，允许请求方访问即可：
```
response.setHeader('Access-Control-Allow-Origin', 'http://gangyuanyuan.top')
```
