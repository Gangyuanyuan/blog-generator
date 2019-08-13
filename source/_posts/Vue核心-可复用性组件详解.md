---
title: Vue核心---可复用性组件详解
date: 2019-08-13 22:44:44
tags: Vue
categories: 前端探索
---

## 使用组件的原因
组件在 Vue 中占据着非常重要的角色，可谓是 Vue 的核心内容。
我们在写代码时经常会遇到大量代码相似和重复的现象，使代码能够被重复利用则可以很好地解决这个问题。
+ 组件作用：提高代码的复用性，进而极大程度地提高工作效率。

## 组件的使用方法
组件需要注册之后才能使用，注册组件和创建 Vue 实例的方法相似。
组件注册分为全局注册和局部注册。
### 全局注册
```
Vue.component('my-component', {
    template: '<div>我是组件的内容</div>'
})
```
第一个参数是自定义的 tag 标签，即要注册的组件的名称。
+ 优点：所有的 Vue 实例都可以使用。
+ 缺点：权限太大，容错率降低（因此全局注册在实际开发中使用较少）。
### 局部注册
```
var app = new Vue({
    el: '#app',
    components: {
        'app-component': {
            template: '<div>我是app局部注册的组件</div>'
        }
    }
})
```
### 组件使用
注册过的组件就可以在 Vue 实例中被当作 html 标签使用了，但局部注册的组件只能在它所注册的 Vue 实例中使用：
```
<div id="app">
    <my-component></my-component>
    <app-component></app-component>
</div>
```
渲染到页面时，组件标签会被替换成我们自定义的内容。
### 解决标签限制
Vue 组件的模板在某些情况下会受到 html 标签的限制，比如 `<table>` 中只能还有 `<tr>`、`<td>`、`tbody` 这些元素，所以直接在 `<table>` 中使用组件是无效的，此时可以使用 `is` 属性来挂载组件，如：
```
<table>
    <tbody is="my-component"></tbody>
</table>
```
## 组件使用小技巧
1. 推荐（必须）使用小写字母加 `-` 方式进行命名，如用 `child`、`my-­componnet` 等命名组件（驼峰式命名会报错）。
2. `template` 中的内容必须被一个 DOM 元素包裹 ，也可以进行嵌套。
3. 在组件的定义中，除了 `template` 选项外还可以定义其他选项，如`data`、`computed`、`methods` 等。
4. **组件注册中的 `data` 必须是一个方法**。
+ 例：用组件实现 两个按钮分别点击分别自增：
```
<div id="app">
    两个按钮共用同一个count，点击任一按钮两个按钮都会自增：<br>
    <button @click="plus">{{count}}</button>
    <button @click="plus">{{count}}</button>
    <hr>
    用组件实现两个按钮分别点击分别自增：<br>
    <btn-component></btn-component>
    <btn-component></btn-component>	
</div>
<script>
	var app = new Vue({
	    el: "#app", 
            data: {
            	count: 1
            },
	    components: {
	    	'btn-component': {
	    		template: '<button @click="count++">{{count}}</button>',
	    		data: function(){
	    			return { count: 0 }
	    		}
	    	}
	    },
        methods: {
            plus: function(){
                this.count++
            }
        },
    })
</script>
```
