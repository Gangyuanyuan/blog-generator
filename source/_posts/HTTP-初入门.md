---
title: HTTP 初入门
date: 2019-04-21 14:57:14
tags: HTTP
categories: 前端探索
---

## 什么是 HTTP

**HTTP**（HyperText Transfer Protocol），即超文本传输协议。
HTTP 协议用于浏览器和服务器建立连接，并指导浏览器和服务器如何进行沟通的。
服务器与浏览器的交互过程如下：
+ 浏览器发起请求
+ 服务器在 **80** 端口接收请求
+ 服务器返回响应内容
+ 浏览器下载响应内容
![服务器与浏览器的交互](https://upload-images.jianshu.io/upload_images/13038962-b053f21889824234.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## HTTP 请求
### 请求的格式
```
第1部分 请求方法 路径 协议/版本

第2部分 Key1: value1
第2部分 Key2: value2
第2部分 Key3: value3
第2部分 Content-Type: application/x-www-form-urlencoded
第2部分 Host: www.baidu.com
第2部分 User-Agent: curl/7.54.0

第3部分 

第4部分 要上传的数据
```
+ 请求最多包含四部分，最少包含三部分，即第四部分可以为空
+ 第三部分永远都是一个回车（`\n`）
+ 动词有 **GET**（获取）、**POST**（新增或上传）、**PUT**（整体更新）、**PATCH**（局部更新）、**DELETE**（删除）、**HEAD**、**OPTIONS** 等
+ 这里的路径包括“查询参数”，但不包括“锚点”
+ 如果没有写路径，那么路径默认为`/`
+ 第 2 部分中的 Content-Type 标注了第 4 部分的格式

### 用 Chrome 发请求
1. 打开 Network（右键 - 检查 - Network 或 F12）
2. 地址栏输入网址
3. 在 Network 点击，查看 request，点击 view source，可以看到请求的前三部分
4. 如果有请求的第四部分，点击 FormData 或 Payload 可以看到

## HTTP 响应
### 响应的格式
```
第1部分 协议/版本号 状态码 状态解释

第2部分 Key1: value1
第2部分 Key2: value2
第2部分 Content-Length: 17931
第2部分 Content-Type: text/html

第3部分

第4部分 要下载的内容
```
+ 响应也包含四部分。
+ 状态码是服务器对浏览器的回应：
    - 1xx ---- 消息（不常用）
    - 2xx ---- 成功
    `200` 响应正常、成功获取资源；
    `204` 请求成功，但没有返回任何内容；
    - 3xx ---- 重定向
    `301` 请求的资源已永久移动到新位置，URL已更新；
    `302` 请求的资源被临时分配了新的 URL；
    `304` 资源未曾被修改，不需要重新传输资源；
    - 4xx ---- 客户端错误
    `403` 服务器已经理解请求，但是拒绝执行它；
    `404` 请求失败，请求的资源未被在服务器上发现；
    - 5xx ---- 服务器错误
    `500` 服务器内部错误，无法完成请求；
    `502` 网关错误；
+ 第 2 部分中的 Content-Type 标注了第 4 部分的格式
+ 第 2 部分中的 Content-Type 遵循 MIME 规范

### 用 Chrome 查看响应
1. 打开 Network（右键 - 检查 - Network 或 F12）
2. 输入网址
3. 选中第一个响应
4. 查看 Response Headers，点击 view source，会看到响应的前两部分
5. 点击 Response 或者 Preview，可以查看响应的第 4 部分

## curl 命令
我们也可以使用命令行来发起请求，这里需要用到 `curl` 命令。
### GET 请求
```
curl -s -v -- "https://www.baidu.com"
```
`-s`：不显示进程； `-v`：显示请求和响应；
行首`*`：表示注释；`>`：表示请求； `<`：表示响应。
![curl请求及响应](https://upload-images.jianshu.io/upload_images/13038962-a08294355a9ea48e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对上图中的请求进行解读：
>+ 获取根目录，使用的协议是 HTTP 1.1
>+ 访问的域名为 www.baidu.com
>+ 用的什么软件发起的请求
>+ 接受返回的任何内容
>+ 空行

### POST 请求
```
curl -X POST -s -v -H "Frank: xxx" -- "https://www.baidu.com"
```
`-H`：加请求头，可以是任意内容

### 带数据的 POST 请求
```
curl -X POST -d "1234567890" -s -v -H "Frank: xxx" --"https://www.baidu.com"
```
`-d`： data，表示上传数据，引号内为上传内容

### GET 请求与 POST 请求的区别
get 是获取内容，post 是上传内容。
除此之外没有任何区别。