---
title: 模块化与 MVC
date: 2019-08-04 09:22:49
tags:
categories: 前端探索
---

## 代码模块化
### 定义
模块化就是把一个复杂的代码段拆分成若干个小片段（模块），使实现不同功能的代码区分开，然后就可以在其他模块中调用这些模块了。
### 模块化缺点
+ 缺点：声明的全局变量会相互影响。
+ 思考：我们需要使用局部变量，但 ES 5 中只有函数有局部变量。
加花括号是不可以的，因为变量提升会把它提升到前面，还是全局变量。
将变量声明在匿名函数里（函数名也是全局变量）并立即执行，Chrome 会报错。
+ 解决方法：
（1）函数前面加 `!`：
```
!function(){}.call()
```
我们不在乎这个匿名函数的返回值，所以加个 `!` 取反没关系。
（2）函数体外面加 `()`：
```
(function(){}).call()
```
不推荐这种写法，容易当成前一句代码的调用而出 bug。
（3）函数名用随机数，不推荐。
```
function jdhfu8477r67(){}.call()
```

### 实现两个模块之间交流
```
// 第一个模块
!function(){
  var person = window.person = {
    name: 'gang',
    age: '18'
  }
}.call()
// 第二个模块
!function(){
  var person = window.person
}.call()
```
使一个模块的局部变量 person 和 全局变量 window.person 不同的变量存相同的地址，第二个模块来调用这个 window.person 就可以了。

### 闭包的应用
一个函数用了它函数体外的变量，这个函数+变量就叫做闭包。
闭包应放在立即执行函数里。
闭包的作用是隐藏细节，控制间接访问变量。
```
var accessor = function(){
  var person = window.person = {
    name: 'gang',
    age: '18'
  }
  return function(){
    person.gae += 1
    return person.age
  }
}

var growUp = accessor.call()
growUp.call()
```
由以上代码可以看出：
立即执行函数使得 person 无法被外部访问；
闭包使得匿名函数可以操作 person；
外部任何地方都可以使用匿名函数操作 person，但是不能直接访问 person。

## MVC 模式
**MVC**（Model View Controller）是一种代码组织形式，用一种业务逻辑、数据、界面显示分离的方法组织代码。
>+ View（视图）：负责处理数据显示部分，表明代码位置。
>+ Model（模型）：负责跟数据逻辑相关的操作，通常包含请求和存取数据。
>+ Controller（控制器）：负责处理用户交互部分，对 View 和 Model 进行控制。

通常 Controller 负责监听 View，用户操作 View（如点击按钮），Controller 从 View 中获取到数据，就会去调用 Model，Model 会与服务器交互，得到数据后返回给 Controller，Controller 得到数据后再去更新 View。

![MVC](https://upload-images.jianshu.io/upload_images/13038962-16c4d9d6dd38769a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)MVC 模式同时提供了对 HTML、CSS 和 JavaScript 的完全控制。
MVC 分层有助于管理复杂的应用程序，让应用程序的测试更加容易，同时也简化了分组开发。
