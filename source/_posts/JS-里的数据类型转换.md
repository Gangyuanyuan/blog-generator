---
title: JS 里的数据类型转换
date: 2019-05-25 14:43:02
tags: JS
categories: 前端探索
---

## 数据类型转换
JavaScript 中，变量可以赋予任何类型的值。但是运算符对数据类型是有要求的，如果运算符发现，运算子的类型与预期不符，就会自动转换类型。数据类型除了可以自动转换以外，还可以手动强制转换。
### 转为字符串（String）
1. `toString()`方法 ：可将其它类型转为字符串类型，但对`null`和`undefined`不适用：
![toString()](https://upload-images.jianshu.io/upload_images/13038962-d62b3a6a2659ecd6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ 当某些场合只能用字符串类型时，系统会自动调用`toString()`将非字符串的内容转为字符串：
![自动调用 toString](https://upload-images.jianshu.io/upload_images/13038962-84eadc921bf89887.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)上图命令2中系统会自动将数字`1`转换为字符串`1`，等价于命令3.

2. 转字符串**快捷方法**
`其它类型 + ''` 或 `'' + 其它类型`：其他类型与一个空字符串相加，此方法对`null`和`undefined`也适用：
![转字符串快捷方法](https://upload-images.jianshu.io/upload_images/13038962-880b898bb5c9db13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ 不同类型相加
因为 `+` 只能用于相同类型相加，遇到不同类型相加时会尝试改变其中的一个类型，下图中命令2等价于命令3：
![不同类型相加](https://upload-images.jianshu.io/upload_images/13038962-da598ca57b960f27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 转字符串**全局方法**
`String()`函数：可将任意类型的值转为字符串，对`null`和`undefined`也适用：
![转字符串全局方法](https://upload-images.jianshu.io/upload_images/13038962-b1c6116237e8a19c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 转为布尔值（Boolean）
1. 转布尔值**全局方法**
`Boolean()`函数：可将其他类型转为布尔值。
![Boolean()](https://upload-images.jianshu.io/upload_images/13038962-91d22d7b08e141a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ 由上图可知，转为布尔值的规则为：
>**数字**：0 —— false；NaN —— false；其他值 —— true.
>**字符串**：空字符串 —— false；非空字符串 —— true.
>**对象**：所有对象 —— true.
>**null** —— false.
>**undefined** —— false.

+  **falsy** 
falsy 是在 Boolean 上下文中认定可转换为false 的值。
五个 falsy 值：`0`，`NaN`，`''`(`""`)，`null`，`undefined`

2. 转布尔值**快捷方法**
`!! 其它类型`：`!`为“取反”之意，因此加两个`!!`取反两次即得到本身的布尔值。
![转布尔值快捷方法](https://upload-images.jianshu.io/upload_images/13038962-71e4d94a3691f3a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 转为数字（Number）
1. 转数字**全局方法**
`Number()`函数：可以将任意类型的值转化成数值。
![Number()](https://upload-images.jianshu.io/upload_images/13038962-64b7edc5d414bc0e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注意：Number() 函数将不可以被解析为数值的字符串转为NaN，将空字符串转为0。
2. 转数字**全局函数**
`parseInt()`函数：转为整数时最好加上进制；因为浮点数只有十进制，所以转为浮点数时进制可省略。（parse：解析）
+ 转为整数：`parseInt(字符串，进制)`
+ 转为浮点数：`parseFloat()`
![parseInt()](https://upload-images.jianshu.io/upload_images/13038962-5a32196d5de7979f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![parseFloat()](https://upload-images.jianshu.io/upload_images/13038962-5a8787da38abc628.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 转数字**快捷方法**：
`- '0'` 或 `+ '数字'`：减去字符 0 或用 0 加这个字符，都等于这个数本身的数值
![转数字快捷方法](https://upload-images.jianshu.io/upload_images/13038962-26bd84a9c83dae31.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 内存图

### 内存 vs 外存
+ 内存：速度快，断电后内容丢失
+ 外存：速度慢，断电后内容不丢失

### 内存图
+ 打开浏览器，各个网页都会分配一定数量的内存，这些内存要分给页面渲染器、网络模块、浏览器外壳和 JS 引擎（V8引擎）
+ 数字都是 64 位，字符都是 16 位，值有七种数据类型
+ JS 引擎将内存分为**代码区**和**数据区**
   数据区分为 **Stack**（栈内存） 和 **Heap**（堆内存）
+ 简单类型的数据直接存在 Stack 里：`（n/s/nul/boo/und/def/sym/）`
   复杂类型的数据是把 Heap 地址存在 Stack 里（引用）：`（obj）`
+ 例：![内存图](https://upload-images.jianshu.io/upload_images/13038962-9f8e0a67133dc385.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### a.self 是如何存储的
Stack 里存储 Heap 的地址，Heap 里也储存 Stack 的内容（即地址），不会占用无数个内存。
```
var a = {}
a.self = a
a.self.self.self
```

### GC 垃圾回收
**如果一个对象没有被引用，它就是垃圾，将被回收。**
+ 例：
```
var fn = function()
document.body.onclick = fn
fn = null
// 问：function() 是垃圾吗？   // 答：function() 被引用，不是垃圾。
```
![垃圾回收](https://upload-images.jianshu.io/upload_images/13038962-ccd5a4cd84f8c0ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 当浏览器页面关掉时，document 将不存在，body、onclick 和 function() 三者都会变成垃圾（IE6 Bug 除外）。

### 内存泄漏
+ 内存泄漏：由于浏览器的一些 bug，使得该被标记为垃圾的东西没有被标记为垃圾，内存就被永久地占用着，直到关掉浏览器。
+ 解决办法：浏览器关闭之前，把所有不再引用的事件都置为 null.
```
window.onunload = function(){
    document.body.onclick = null
}
```

### 浅拷贝 vs 深拷贝
+ **深拷贝**：b 变不影响 a
对于简单类型的数据来说，赋值就是深拷贝。
对于复杂类型的数据（对象）来说，才要区分浅拷贝和深拷贝。
```
var a = 1
var b = a
b = 2
// 问：a = ?    // 答：a = 1
```
+ **浅拷贝**：b 变致 a 变
```
var a = {name: 'a'}
var b = a
b.name = 'b'
// 问：a.name = ?    // 答：a.name 也变成了 'b'
```
