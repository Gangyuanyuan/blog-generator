---
title: Vue 基础入门
date: 2019-08-07 21:50:19
tags: Vue
categories: 前端探索
---

## Vue.js 实例和数据绑定
通过构造函数 `Vue()` 就可以创建一个 Vue 的根实例，并启动 Vue 应用。
Vue 实例就是使用 Vue.js 的入口。
```
<!-- 挂载到这里 -->
<div id="app">
    {{message}}
</div>
<!-- 环境搭建 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 创建实例 -->
<script>
var app = new Vue({
    el: '#app',  // element，用于指定页面中已经存在的DOM元素，挂载到DOM中
    data: {  // 声明应用内需要双向绑定的数据
        message: 'Hello,Vue!'
    }
})
</script>
```
+ **el**
`el` 即 element，是必不可少的选项，用于指定一个页面中已存在的 DOM 元素来挂载 Vue 实例，用标签（如 `div`）或 CSS 语法（如 `#app`）都可以。挂载后在 Vue 实例中定义的所有属性和方法都可以在此 DOM 中使用。
+ **data**
通过 Vue 实例的 `data` 选项，可以声明应用内需要双向绑定的数据。建议所有会用到的数据都预先在 `data` 内声明，这样不至于将数据散落在业务逻辑中，难以维护。也可以指向一个已经有的变量。
+ 访问 Vue 实例的属性，在属性前加 `$`，如 `app.$e1`，`app.$data`；
访问 data 元素的属性，直接 `app.属性名`，如 `app.message`。

## 生命周期钩子
+ **created**
实例创建完成后调用，此阶段完成了数据的观测等，但尚未挂载， `$el` 还不可用。需要初始化处理一些数据时会比较有用。（**还未挂载**）
+ **mounted**
`el`挂载到实例上后调用，一般我们的第一个业务逻辑会在这里开始 。相当于 jQuery 的 `$(document).ready()`。（**刚刚挂载**）
+ **beforeDestroy**
实例销毁之前调用，要解绑一些使用 `addEventListener` 监听的事件等。

下面我们通过一个例子来理解生命周期钩子，如在页面中实时显示当前时间：
```
<div id="app">
    {{date}}
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            date: new Date()
        },
        // 生命周期钩子
        created: function() {
              alert("我是Vue实例，创建完成，还未挂载到Dom")
        },
        mounted: function(){
            alert("我是Vue实例，已经挂载到DOM")
            var _this = this // this代表Vue实例本身
            this.timer = setInterval(function(){
                _this.date = new Date()
            }, 1000)
        },
        beforeDestroy: function(){
            if(this.timer){
                clearInterval(this.timer)
            }
        }
    })
</script>
```

## 文本差值和表达式
### 语法
双大括号（Mustache 语法）`{{}}` 是最基本的文本插值方法，它会自动将我们双向绑定的数据实时显示出来。在前文中我们已经使用过：
```
<div id="app">
    {{message}}
</div>
```
### 用法
+ 在文本差值的 `{{}}` 中，除了简单的绑定属性值外，还可以使用 JavaScript 表达式进行简单的运算 、 三元运算等。
```
<div id="app">
    {{6*3}}
    {{3>6 ? message : a}}
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            message: 'Hello,Vue!',
			a: 1
        }
    })
</script>
```
+ Vue.js 只支持单个表达式，不支持多行表达式，不支持语句和流控制。
>例：
`{{ 6+6*3 }}` --- 可以进行简单的运算
`{{ 6<3 ? message : a}}` --- 可以使用三元运算符
注意：
`{{ if(6>3) {return message} }}` --- 不支持流控制，要使用三元运算
`{{ var a = 6 }}` --- 相当于 `var a ; a = 6;`，这是语句，不是表达式

## 过滤器
1. Vue 支持在 `{{}}` 插值的尾部添加管道符 `|` 对数据进行过滤。过滤器经常用于格式化文本，比如字母全部大写、货币千位使用逗号分隔等。过滤的规则是自定义的， 通过给 Vue 实例添加选项 `filters` 来设置：
```
{{data | filter}}
```

`|` 后面是过滤器的名字。
+ 例：将实时时间进行过滤使其格式化：
```
<div id="app">
    {{date}}
    {{date | formatDate}} <!-- 过滤器,| 后面是过滤器的名字 -->
</div>
<script>
    // 实现功能：在页面中实时显示当前时间
    // 在月份、日期、小时小于10时前面补0
    var plusZero = function(value){
        return value<10 ? '0'+value : value
    }
    var app = new Vue({
        el: '#app',
        data: {
            date: new Date(),
        },
        // 定义过滤器
        filters: {
            formatDate: function(value,a,b){ // 这里的value就是需要过滤的数据
                var date = new Date(value) // 将字符串转化为date类型
                var year = date.getFullYear()
                var month = plusZero(date.getMonth()+1)
                var day = plusZero(date.getDay())
                var hours = plusZero(date.getHours())
                var min = plusZero(date.getMinutes())
                var sec = plusZero(date.getSeconds())
                return year+'--'+month+'--'+day+'  '+hours+':'+min+':'+sec+' '+a+' '+b
            }
        },
        mounted: function(){
            var _this = this // this代表Vue实例本身
            this.timer = setInterval(function(){
                _this.date = new Date()
            }, 1000)
        },
        beforeDestroy: function(){
            if(this.timer){
                clearInterval(this.timer)
            }
        }
    })
</script>
```
效果如下图所示，第一行是直接获取的时间，第二行是经过滤器过滤后的时间，即格式化后的时间：![过滤器](https://upload-images.jianshu.io/upload_images/13038962-ff3a518c60319b82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 过滤器的串联：
```
{{data | filter1 | filter2}}
```

3. 过滤器的参数：
```
{{date | formatDate(arg1, arg2)}}
```
实例中第一个和第二个参数，分别对应过滤器的第二个和第三个参数，如：`{{date | formatDate(11, 22}}` 对应 `formatDate: function(value,a,b){}` 过滤器函数中的参数 a 和 b。

## 指令和事件
1. 指令（ Directives ）是 Vue 模板中最常用的一项功能，它带有前缀 `v-`，能帮我们快速完成 DOM 操作，循环渲染、显示和隐藏。

2. 常用指令
+ `v-text`：­解析文本，和 `{{}}` 作用一样
+ `v-html`：解析 html
+ `v-bind`：动态更新 HTML 元素上的属性，如 `id`、`class` 等（注意 v-bind 后面是冒号）
+ `v-on`：绑定事件监听器，如 `click`、`keyup` 等（函数必须写在 methods 里面
）
**注意**：前两者后面跟 `=`，后两者后面跟 `:`

>v-­on 具体介绍：
>在普通元素上， `v-­on` 可以监听原生的 DOM 事件，除了 `click` 外，还有
>`dbclick`（双击）、`keyup`、`mousemove` 等。表达式可以是一个方法名，这些方法都写在 Vue 实例的 `methods` 属性内，并且是函数的形式，函数内的 `this` 指向的是当前 Vue 实例本身，因此可以直接使用 `this.xxx` 的形式来访问或修改数据。
3. 常用指令实例演示：
```
<style>
    .transRed{ background: red; height: 30px; width: 100px; }
</style>
<div id="app">
    v-text 指令：解析文本 <br>
    <span v-text="apple"></span> <br>
    v-html 指令：解析html <br>
    <span v-html="banana"></span> <br>
    v-bind 指令：绑定活的属性 <br>
    <div v-bind:class="className"></div> <br>
    v-on 指令：绑定事件监听器 <br>
    <button v-on:click="count">{{countNum}}</button> <!--为按钮添加监听事件--><br>
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            apple: '苹果',
            banana: '<span style="color: yellow;">香蕉</span>',
            className: 'transRed',
            countNum: 0
        },
        methods: {
            count: function(){
                this.countNum = this.countNum + 1
            }
        }
    })
</script>
```
![常用指令演示](https://upload-images.jianshu.io/upload_images/13038962-664b0efc181c3175.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4.  Vue 中用到的所有方法都定义在 `methods` 中。

## 语法糖
语法糖是指在不影响功能的情况下 ， 添加某种简洁方法实现同样的效果 ， 从而更加方便程序开发：
`v-bind` ——> `:`
`v-on` ——> `@`

上例中可以替换为语法糖的形式：
```
<div v-bind:class="className"></div>
<div :class="className"></div>

<button v-on:click="count">{{countNum}}</button>
<button @click="count">{{countNum}}</button>
```
