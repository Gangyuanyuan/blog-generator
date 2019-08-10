---
title: Vue 的内置指令
date: 2019-08-10 12:10:26
tags: Vue
categories: 前端探索
---

## 基本指令
### v­-cloak
一般与 `display: none;` 结合使用。
**作用**：解决初始化较慢时导致的页面闪动。
**原理**：`v-cloak` 使其在 Vue 实例渲染之后再进行渲染，改变了渲染顺序。 
### v-­once
**作用**：定义它的元素或组件只渲染一次，后续都不会再重新渲染。

## 条件渲染指令
### v-­if，v-­eles-­if，v-­else
+ 用法：
`v-if` 和 `v-else-if` 后接的是等号，等号后必须是布尔值。
```
<p v-if="6<3">{{apple}}</p>
<p v-else-if="6<9">{{banana}}</p>
<p v-else>{{pear}}</p>
```
+ 弊端 :
Vue 在渲染元素时，出于效率考虑，会尽可能地复用已有的元素而非重新渲染，因此会出现乌龙。（只会渲染变化的元素，即同一类元素会被复用，如 `input`）
+ 解决方法：
对不想使其复用的元素或标签加（唯一的） `key` 值。

### v-­show
显现与否取决于布尔值，只改变了 css 属性 `display`。
+ 用法：
```
<p v-show="9>3">我被渲染了</p>
```
### v-­if 和 v­-show 的比较
是否删除dom节点。
+ v-­if：
实时渲染：页面显示就渲染，不显示则移除。
+ v-­show：
v-­show 的元素永远存在于页面中，只是改变了 CSS 的 `display` 属性。

## 列表渲染指令 v­-for
当需要将一个数组遍历或枚举一个对象属性循环显示时，就会用到列表。
1. 两种使用场景：
遍历多个对象。
遍历一个对象的多个属性。

2. 用法：`v-for` 一定要写在要遍历的元素上，`v-for` 后接等号，类似于 `item in items`。遍历多个对象遍历的一定是数组。

（1）遍历多个对象，vueMth 是自己定义的变量：
```
<ul>
    <li v-for="vueMth in vueMethods">{{vueMth.name}}</li>
</ul>

data: {
    vueMethods: [
    	{name: '多思考'},  // 每个对象对应一个li
    	{name: '多练习'},
    	{name: '多使用'}
    ]
}
```
带索引的写法：括号里第一个变量代表 item，第二个变量代表 index：
```
<ul>
    <li v-for="(vueMth,index) in vueMethods">{{index}}---{{vueMth.name}}</li>
</ul>
```
（2）遍历一个对象的多个属性：
```
<span v-for="value in nvshen">{{value}}</span>

data: {
    nvshen: {
    	girl2: '汤唯 ',
    	girl1: '高圆圆 ',
    	girl3: '朱茵 '
    }
}
```
拿到 value，key，index 的写法，v-k-i 谐音外开：
```
<div v-for="(value,key,index) in nvshen">第{{index}}个，键是{{key}}，{{value}}</div>
```

## 数组更新，过滤与排序
1. 改变数组的一系列方法：
+ `push()`：在末尾添加元素
+ `pop()`：将数组的最后一个元素移除
+ `shift()`：删除数组的第一个元素
+ `unshift()`：在数组的第一个元素位置添加一个元素（返回新数组的长度）
+ `splice()`：可以删除或者添加元素（返回删除的元素，添加元素时返回空数组）
（1）三个参数：
第一个参数表示开始操作的位置
第二个参数表示要操作的长度
第三个为可选参数，添加时使用
（2）用法
```
app.arr.splice(1,1) // 从第一个元素位置开始，删除一个元素（即删除第二个元素）
app.arr.splice(1,0,"eraser") // 从第一个元素位置开始，添加一个元素
app.arr.splice(1,0,"eraser","rule") // 从第一个元素位置开始，添加两个元素
```
+ `sort()`：排序
+ `reverse()`：翻转数组

2. 两种数组变动 Vue 检测不到：
+ 改变数组的指定项：`app.arr[0] = 'car'`
+ 改变数组长度：`app.arr.length = 1`
3. 解决方法：
+ set 
改变数组指定项： `Vue.set(app.arr,index,"car")`
+ splice 
改变数组长度：`app.arr.splice(0)`（从第0个位置开始，删除所有项）

## 方法和事件
### 基本用法
`v­-on` 绑定的事件类似于原生的 `onclick` 等写法。
```
methods:{
    handle:function (count) {
        count = count || 1;
        this.count += count;
    }
}
```
使用带有参数的方法时，一定要在方法后面加括号 `()`。
如果方法中带有参数，但使用方法时没有加括号，则默认传原生事件对象 event。

### 修饰符
在 Vue 中传入 event 对象用 `$event`。
1. 监听单机事件
+ `stop`：阻止单击事件向上冒泡（用于子元素）
+ `prevent`：提交事件并且不重载页面
+ `self`：只是作用在元素本身而非子元素的时候调用（用于父元素）
+ `once`：只执行一次的方法

2. 监听键盘事件

+ 指定的 keyCode：`<input @keyup.13 ="submitMe">`
+ Vue.js 还为我们提供了 `.enter`，`.tab`，`.delete` 等快捷用法
