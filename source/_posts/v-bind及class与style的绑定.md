---
title: v-bind及class与style的绑定
date: 2019-08-09 23:44:36
tags: Vue
categories: 前端探索
---

`v-bind` 应用场景: 
DOM 元素经常会动态地绑定一些 class 类名或 style 样式。

## 了解 v-bind 指令
>`v-bind` 指令复习：
动态更新 HTML 元素上的属性，如 `id`、`class` 等。

+ 在数据绑定中，最常见的两个需求就是元素的样式名称 class 和内联样式 style 的动态绑定，它们也是 HTML 的属性，因此可以使用 `v-­bind` 指令。
+ 我们只需要用 `v-­bind` 计算出表达式最终的字符串就可以，不过有时候表达式的逻辑较复杂，使用字符串拼接方法较难阅读和维护，所以 Vue.js 增强了对 class 和 style 的绑定。

## 绑定 class 的几种方式
### 对象语法
对象语法即给 `v-­bind: class` 设置一个对象，可以动态地切换 class。
语法：对象的键是类名，值对应布尔值 `true` 和 `false`（布尔值在 data 中设置）。
+ 某个 class 是否会渲染到页面上，是根据其布尔值来判断的。
```
<style>	
    .red { background: red; }
    .blue { background: blue; }
</style>
<div id="app">
    <div v-bind:class="{divStyle: isActive, borderStyle: isBorder}"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isRed: true,
            isBlue: false
        }
    })
</script>
```
当 class 的表达式过长或逻辑复杂时，还可以**绑定计算属性**，这是一种很友好和常见的用法。一般当条件多于两个时， 都可以使用 `data` 或 `computed`。

### 数组语法
当需要应用多个 class 时， 可以使用数组语法 ，即给 `v-bind: class` 绑定一个数组，应用一个 class列表，数组中的成员直接对应类名（在 `data` 中定义）。
+ 数组中定义的 class 都会被渲染到页面上。
```
<style>	
    .active { background: black; height: 100px; width: 100px; }
    .error { border: 10px solid red; }
</style>
<div id="app">
    <div v-bind:class="[activeClass, errorClass]"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            activeClass: 'active',
            errorClass: 'error'
        }
    })
</script>
```
### 数组语法和对象语法混用
可以用三目运算实现对象和数组混用。数组语法和对象语法混用时，使数组的第一个成员是对象，第二个成员是数组成员（一个由布尔值决定是否出现，一个永远存在）。
```
<style>	
    .active {......}
    .error {......}
</style>
<div id="app">
    <div v-bind:class="[{active:isActive}, errorClass]"></div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            isActive: true,
            errorClass: 'error'
        }
    })
</script>
```
### 在组件上使用（暂不考虑）

## 绑定内联样式
使用 `v­-bind:style` 可以给元素绑定内联样式，方法与 `v-bind:class` 类似，也有对象语法和数组语法，看起来很像直接在元素上写 CSS。
### 对象语法
绑定内联样式，键代表 style 的属性值，值代表属性对应的值。
```
<div id="app">
    <div v-bind:style="{'color':color, 'fontSize':fontSize+'px'}">
        Hello！
    </div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            color: 'red',
            fontSize: 16
        }
    })
</script>
```
### 数组语法
应用多个样式对象时，可以使用数组语法，在实际业务中，style 的数组语法并不常用，因为往往可以写在一个对象里面，而较为常用的是计算属性。
```
<div id="app">
    <div v-bind:style="[styleA, styleB]">
        Hello！
    </div>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            styleA:{
                color: 'green',
                fontSize: 16,
            },
            styleB:{
                width: 100px,
                height: 100px
            }
        }
    })
</script>
```

### 注意
+ CSS 属性名称使用 驼峰式命名（camelCase）或 短横线分隔命名（kebab-­case）。
+ Vue 中所有大写字母都会转换成“横杠加小写”，
如 `fontSize` ---- `font-size`，`dsfASDghG` ---- `dsf-a-s-dgh-g`。
+ 使用 `v-bind:style` 时， Vue.js 会自动给特殊的 CSS 属性名称增加前缀，如 `transform`，无需再加前缀属性！！！