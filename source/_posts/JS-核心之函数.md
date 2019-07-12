---
title: JS 核心之函数
date: 2019-07-12 23:35:29
tags: JS
categories: 前端探索
---

函数是一段可以反复调用的代码块。
函数还能接受输入的参数，不同的参数会返回不同的值。

## Function 对象
### 构造函数
`Function` 构造函数可以创建一个新的 Function 对象，它的语法：
```
new Function (arg1, arg2, ...argN, functionBody)
```
用 Function 创建函数时加不加 `new` 效果是一样的：
```
var f = new Function('a', 'b', 'return a+b')
var f = Function('a', 'b', 'return a+b')
// f(1,2)    3
```
### 字符串之间加变量
```
var n = 1
var f = new Function('x','y','return x+' + n + '+y')
// f(1,2)   4
```
### **function** 与 **Function** 的区别
`function` 是关键字，与 `var` 用法相似，`function` 用来声明一个函数。
`Function` 是全局对象，`window.Function` 用法与 `window.Object` 相似，也可以用来声明函数。

## 函数的声明
函数有五种声明方式。
### 具名函数
```
function f(x, y){
  return x+y
}
```
函数可以没有参数，但必须有返回值，不写的话则自动 `return undefined`.
### 匿名函数
```
 var f
 f = function(x, y){
     return x+y
 }
```
单独声明一个匿名函数会报错，需把它赋给一个变量才可以。
### 具名函数赋值
```
 var f
 f = function f2(x, y){ return x+y }
 console.log(f2) // Uncaught ReferenceError: f2 is not defined
```
**注意**：打印 `f2` 时报错。采用表达式声明函数时，`function` 命令后面若加上函数名，该函数名只在函数体内部有效，在函数体外部无效。
### window.Function
```
 var f = new Function('x','y','return x+y')
```
### 箭头函数
```
 var f = (x, y) => { return x+y }
 var sum = (x, y) => x+y
 var n2 = n => n*n
```
箭头函数没有函数名。
如果只有一个参数，可以省略 `()`；
如果函数体只有一句话，可以同时省略 `{}` 和 `return`，不能只省略一个。

## 函数的属性和方法
### name 属性
+ 函数的 `name` 属性返回函数的名字。
```
function f1() {}
f1.name // "f1"
```
+ 如果是通过变量赋值定义的匿名函数，那么 `name` 属性返回变量名。
```
var f2 = function () {};
f2.name // "f2"
```
+ 若变量的值是一个具名函数，`name` 属性返回 `function` 关键字之后的那个函数名。
```
var f3 = function f4() {};
f3.name // 'f4'
f4.name // 报错
```
+ 若用 Function 构造一个函数，`name` 属性返回 `anonymous`，即“匿名的”。
```
 var f5 = new Function('x','y','return x+y')
 f5.name // "anonymous"
```
### length 属性
函数的 `length` 属性返回函数定义时的参数个数。无论调用时输入了多少个参数，`length` 属性始终不变。
```
function f(x, y) {}
f.length // 2
```

### call() 方法
`f.call()` 方法用来调用函数，即执行这个函数的函数体。`f`只是代表这个函数，并不能执行它。
```
f.call(asThis, input1,input2)
```
其中 `asThis` 会被当做 `this`，`[input1,input2]` 会被当做 `arguments`.
```
function f(x,y){ return x+y }
f.call(undefined, 1,2) // 3
```
新手调用函数时最好不要直接用 `f()`，用 `f.call()`可以加深对 `this` 的理解，`this`就是 `call()` 的第一个参数。

## 函数其他知识点
### 参数 this 和 arguments
`call()` 的第一个参数可以用 `this` 得到；
`call()` 的后面的参数可以用 `arguments` 得到。
1. `this`
在**普通模式**下，如果 `this` 是 `undefined`，浏览器会自动把 `this` 变成`window`.
在**严格模式**下，`this` 传的参数的值不会改变。
![this](https://upload-images.jianshu.io/upload_images/13038962-502dce90b8672a36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 易错题 ![易错题](https://upload-images.jianshu.io/upload_images/13038962-8cde95dea5764902.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)(1)`this` 就是 call 的第一个参数。
(2)每个函数都有自己的 `this`，第一个 `this` 对应的 `call` 是 `f1.call(obj)`，第二个 `this` 对应的 `call` 是 `f2.call()`。
(3)`this` 和 `arguments` 都是参数，参数都要在函数执行（call）的时候才能确定。

2. `arguments`
+ `arguments` 对象包含了函数运行时的所有参数，`arguments[0]` 是第一个参数，`arguments[1]` 是第二个参数，以此类推。
+ `arguments` 对象是**伪数组**。
+ `arguments` 对象只有在函数体内部才可以使用。
![arguments](https://upload-images.jianshu.io/upload_images/13038962-5a503d05dd190f0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### call stack 调用栈
+ 代码段执行时，每进入一个函数之前，都会把函数的位置记入到 `stack` 里面，依次进行，`return` 时再回到 `stack` 里最上面的位置。例：
```
function a() { console.log('a1'); b.call(); console.log('a2'); return 'a' }
function b() { console.log('b1'); c.call(); console.log('b2'); return 'b' }
function c() { console.log('c'); return 'c' }
a.call()
console.log('end')
```
上面代码段调用栈的过程图解如下：![call stack 调用栈](https://upload-images.jianshu.io/upload_images/13038962-ab1673f73bdfdae2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 递归
```
function sum(n){
  if(n === 1) { return 1 }
  else { return n + sum.call(undefined, n-1) }
}
sum.call(undefined, 3) // 6
// sum(3) = 3 + sum(2)，sum(2) = 2 + sum(1)，sum(1) = 1
```
`n` 不可以无限大，超过栈内存就会发生溢出 `stack overflow`.
### 作用域
1. 定义：作用域（scope）指变量存在的范围。
**全局作用域**：变量在整个程序中一直存在，所有地方都可以读取；
**函数作用域**：变量只在函数内部存在。
函数外部声明的变量是“全局变量”（global variable），它可以在函数内部读取。
在函数内部定义的变量，为“局部变量”（local variable），外部无法读取。
+ 对于`var`命令，局部变量只能在函数内部声明，在其他区块中声明，一律都是全局变量。
+ 对于`a = 3` 语句，会优先认为它是一个**赋值语句**，并按作用域从小到大找变量`a`，找到后给变量`a`赋值；若整个作用域里都找不到变量`a`，则认为它是**声明并赋值**，此时会声明一个**全局变量**`a`并赋值为`3`.
2. 变量提升
函数作用域内部也会产生“变量提升”现象。`var`命令声明的变量，不管在什么位置，变量声明都会被提升到函数体的头部。
+ 易错题 1：变量提升
```
var a = 1
function f1(){
    f2.call()
    console.log(a)  // 断点 1
    var a = 2
    function f2(){
        var a = 3
        console.log(a)  // 断点 2
    }
}
f1.call()
console.log(a)
```
问：运行到断点1时，打印出 `a` 的值为? 
答：由于变量提升，正确答案是 `undefined`.
技巧：看到题目后，第一件事是先做变量提升。
```
// 变量提升后
var a = 1
function f1(){
    var a  // f1 的变量提升
    function f2(){
        var a  // f2 的变量提升
        a = 3
        console.log(a)  // 断点 2
    }
    f2.call()
    console.log(a)  // 断点 1
    a = 2
}
f1.call()
console.log(a)
```
+ 易错题 2：作用域
```
var a = 1
function f1(){
    console.log(a)
    var a = 2
    f4.call()
}
function f4(){
    console.log(a)  // 断点 3
}
f1.call()
console.log(a)
```
问：断点 3 处 函数 f4 打印出 a 的结果是？
答：a = 1。函数 f4 的变量只在它自己的作用域或它的父作用域里有效。
+ 易错题 3
```
var a = 1
function f1(){
    console.log(a)
    var a = 2
    f4.call()
}
function f4(){
    console.log(a)  // 断点 4
}
a = 2
f1.call()
console.log(a)
```
问：断点 3 处 函数 f4 打印出 a 的结果是？
答：a = 2。因为函数的执行在语句`a = 2`之后发生。
+ 易错题 4
问：点击“选项3”，打印出的两个 i 分别是多少？![易错题 4](https://upload-images.jianshu.io/upload_images/13038962-c89f2c47ff39bc9b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)答：打印出的两个 i 都是 6。因为 “用户移动鼠标点击的时间” 远远小于 “for 循环的时间”，所以点击 “选项 2” 时 for 循环已执行完毕，第一个 i=6；第二个 i 打印是在跳出 for 循环之后，所以结果也为 6。

### 闭包
+ 定义：如果一个函数使用了它作用域以外的变量，那么“这个函数”+“函数内部能访问到的变量”（环境）的总和，就叫做闭包。下面的一段代码就是闭包：
```
var a = 1
function f(){
  console.log(a)
}
```
+ 闭包的用途：
闭包可以用来间接访问一个变量，即隐藏一个变量。
如果一个变量不方便别人直接访问，这时可以在一个函数作用域中声明这个变量，用子函数操作这个变量并将结果 return 到外部函数，使他人间接访问这个变量。例：
```
!function f1(){
  var a = 0;
  function f2(){
    return a += 1
  }
  return f2
}()
```
