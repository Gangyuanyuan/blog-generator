---
title: 实现 AJAX
date: 2019-08-03 08:42:25
tags: AJAX
categories: 前端探索
---

## AJAX
### 定义
AJAX（Asynchronous JavaScript and XML），即“异步的 Javascript 和 XML”。它通过 JavaScript 的异步通信，从服务器获取 XML 文档从中提取数据，再更新当前网页的对应部分，而不用刷新整个网页（后来，只要用 JavaScript 脚本发起 HTTP 通信，都可以叫做 AJAX 通信）。

### AJAX 包括以下几个步骤：
+ 创建 XMLHttpRequest 实例，发出 HTTP 请求
+ 接收服务器返回的数据（XML 格式的字符串）
+ JS 解析 XML，并更新局部页面

即 AJAX 通过原生的 XMLHttpRequest 对象发出 HTTP 请求，得到服务器返回的数据后，再进行处理。现在服务器返回的都是 JSON 格式的数据，XML 格式已经过时了。

## 用原生 JS 写 AJAX 请求
1. 为了更好地理解 AJAX，我们可以尝试用原生 JS 构造一个 AJAX 请求。
+ XMLHttpRequest 是 window 下的一个构造函数（全局对象），没有参数。用 `new` 命令生成对象：
```
var request = new XMLHttpRequest()
```
+ 用 `open()` 方法初始化 request 对象，指定请求方式和服务器网址：
```
request.open('GET', 'http://www.baidu.com/xxx')
```
+ 用 `send()` 方法发出 HTTP 请求：
```
request.send()
```
+ 监听通信状态（readyState属性）的变化并指定函数：
```
request.onreadystatechange = function(){ // 监听request的状态变化
  if (request.readyState === 4){ // 请求和响应都已完毕
    if(request.status >= 200 && request.status < 300){ // 请求成功
      console.log('请求成功')
      let string = request.responseText // 把符合JSON语法的字符串转换成JS对应的值
      let object = window.JSON.parse(string) // JSON.parse是浏览器提供的
    }else if(request.status >= 400){
      console.log('请求失败')
    }
  }
}
```
上面代码中，一旦 XMLHttpRequest 实例的状态发生变化，就会执行函数内容。
如果拿到服务器返回的数据，AJAX 不会刷新整个网页，而是只更新网页里面的相关部分，从而不打断用户正在做的事情。

2. **注意**
在使用开发者工具时，需注意：
+ 查看请求和响应，记得点一下 view source！
+ 查看响应的内容，点一下 response！
+ 响应的第四部分返回的永远是字符串！
即使符合别的格式，它本质上还是字符串。
