---
title: Vue 之 render 函数
date: 2019-09-04 18:20:37
tags: Vue
categories: 前端探索
---

## render 函数初步了解
在使用 Vue 组件的过程中，当 `template` 中的内容较多时，可以在外部用 `template` 标签来定义，然后在组件中用 `id` 引入。但 `template` 下只允许有一个子节点，如需同时设置多个子节点，可用 `div` 将它们包裹起来。例：
```
<div id="app">
	<child :level='level'>
		春暖花开
	</child>
</div>
<template id="hdom">
	<div>
		<h1 v-if="level==1">
			<slot></slot>
		</h1>
		<h2 v-if="level==2">
			<slot></slot>
		</h2>
		<h3 v-if="level==3">
			<slot></slot>
		</h3>
	</div>
</template>
<script>
	Vue.component('child',{
		props: ['level'],
		template: '#hdom'
	});
	var app = new Vue({
        el: "#app", 
        data: {
           level: 1 
        }
    })
</script>
```
## render 函数的参数
使用组件时，因为 `template` 中的内容会先在页面中渲染，如上例中，每个 `h` 标签都会被渲染，且当把模板直接写在 `template` 中时，会导致代码比较冗长。因此 Vue 为我们提供了 `render` 函数来解决这个问题。
```
<div id="app">
	<child :level='level'>
		春暖花开
	</child>
</div>
<script>
	Vue.component('child',{
		render: function(createElement){
			return createElement('h'+this.level, this.$slots.default)
		},
		props: ['level']
	})
	var app = new Vue({
        el: "#app", 
        data: {
           level: 1
        }
    })
</script>
```
### render 函数的第一个参数
在 `render` 函数的后面必须是 `createElement` 方法，`createElement` 的类型是 function。
`render` 函数的第一个参数是必选的，类型可以是 String、Object 或 Function。
>String —— html标签
>Object —— 一个含有数据选项的对象
>Function —— 方法返回含有数据选项的对象

```
<script>
	Vue.component('child',{
		render: function(createElement){
			// String
			// return createElement('div')

			// Object
			// return createElement({
			// 	template: '<div>安得广厦千万间</div>'
			// })

			// Function
			var domFun = function(){
				return { 
					template: '<div>安得广厦千万间</div>'
				}
			}
			return createElement(domFun())
		}
	})
</script>
```

### render 函数的第二个参数
`render` 函数的第二个参数是可选的，这个参数是数据对象，它的类型只能是 Object。
```
<script>
	Vue.component('child',{
		render: function(createElement){
			return createElement({
				template: '<div>天凉好个秋</div>'
			}, {
				'class': { // 因为class是关键字，建议用引号包裹
					foo: true,  // class列表中包含foo但不包含baz
					baz: false
				},
				style: {
					color: 'red',
					fontSize: '16px'
				},
				attrs: { // attributes，正常的html特性（即除了style之外的属性）
					id: 'foo',
					src: 'http://www.baidu.com'
				},
				domProps: { // 用来写原生的Dom属性
					innerHTML: '<span style="color:green;font-size:18px;">关关雎鸠，在河之洲</span>'
				}
			})
		}
	})
	var app = new Vue({
        el: "#app"
    })
</script>
```

### render 函数的第三个参数
`render` 函数的第三个参数也是可选的，类型可以是 String 或 Array，它是作为我们构件函数的子节点来使用的。
```
<script>
	Vue.component('child',{
		render: function(createElement){
			return createElement('div',[
				// 因为第二个参数只能是对象，因此后面是第三个参数
				createElement('h1','我是h1标题'), // 创建一个包含文本节点的h1节点
				createElement('h6','我是h6标题')
			])
		}
	})
</script>
```

## this.$slots 在 render 函数中的应用
`render` 函数的第三个参数存储的是 VNODE（虚拟节点）。
之前我们直接用 html 控制页面时，用 JS 来操作 DOM 树存在一些缺陷，即每次操作 DOM 所有的 html 都会进行重绘。现在由于 Vue 的数据双向绑定， Vue 检测到哪个虚拟节点发生变化，就直接更新这个 VNODE，因此效率比直接操作 DOM 元素高很多。
```
<div id="app">
	<my-component>
		<p>君不见黄河之水天上来</p>
		<p>奔流到海不复回</p>
		<h3 slot="header">我是标题</h3>
		<h5 slot="footer">我是正文最后一段</h5>
	</my-component>
</div>
<script>
	Vue.component('my-component', {
		render: function(createElement){
			var header = this.$slots.header
			var main = this.$slots.default
			var footer = this.$slots.footer
			return createElement('div', [
				createElement('header',header), // header是html5中的标签
				createElement('main',main),
				createElement('footer',footer),
			])
		}
	})
	var app = new Vue({
        el: "#app"
    })
</script>
```
`var header = this.$slots.header` 返回的内容就是含有 VNODE 的数组，
`createElement('header', header)` 返回的就是 VNODE。

## 在 render 函数中使用 props 传递数据
例：实现点击按钮切换图片：
```
<div id="app">
	<button @click="switchShow">点击切换图片</button> {{show}}
	<my-component :show="show"></my-component>
</div>
<script>
	Vue.component('my-component', {
		props: ['show'],
		render: function(createElement){
			var imgSrc
			if(this.show){
				imgSrc = 'img/001.jpg'
			}else{
				imgSrc = 'img/002.jpg'
			}
			return createElement('img',{
				attrs: {
					src: imgSrc

				},
				style: {
					width: '600px',
					height: '400px'
				}
			})
		}
	})
	var app = new Vue({
        el: "#app", 
        data: {
        	show: false
        },
        methods: {
        	switchShow: function(){
        		this.show = !this.show
        	}
        }
    })
</script>
```

## v-model 在 render 函数中的使用
例：将在输入框输入的内容实时显示在下方：
```
<div id="app">
	<my-component :name="name" v-model="name"></my-component>
	<br>{{name}}
</div>
<script>
	Vue.component('my-component', {
		render: function(createElement){
			var self = this // 指的就是当前的Vue实例
			return createElement('input',{
				domProps: {
					value: self.name // 将接收的name赋值给input框的value
				},
				on: {
					input: function(event){
						self.$emit('input',event.target.value) // 触发input事件并将输入的value值传递给父组件
					}
				}
			})
		}
	})
	var app = new Vue({
        el: "#app", 
        data: {
        	name: 'Jack'
        }
    })
</script>
```

## 作用域插槽在 render 函数中的使用
作用域插槽即子组件向父组件的插槽中传递数据的一个过程，在 render 函数中我们使用 `this.$scopedSlots.default()` 来通过作用域插槽向父组件插槽中传递数据，传递的内容就是括号里面的对象。
```
<div id="app">
	<my-component>
		<template scope="prop"> <!-- prop是自定义的 -->
			{{prop.text}} <br>
			{{prop.message}}
		</template>
	</my-component>
</div>
<script>
	Vue.component('my-component', {
		render: function(createElement){
			// 相当于 <div><slot :text="text"></slot></div>
			return createElement('div',this.$scopedSlots.default({
				text: '我是子组件传递过来的数据',
				message: 'scopetext'
			}))
		}
	})
	var app = new Vue({
        el: "#app"
    })
</script>
```

## 函数化组件的应用
`functional: true` 表示该组件无状态无实例，无实例即组件内部没有 `this` 的概念，此时我们想要拿到从外部传递过来的数据，可以通过 `context` 进行读取。`context` 即上下文对象，读取父组件的内容可以用 `context.parent.xxx`。
```
<div id="app">
	<my-component value="haha"></my-component>
</div>
<script>
	Vue.component('my-component', {
		// 表示该组件无状态无实例（组件内部没有this的概念）
		functional: true, 
		render: function(createElement, context){
			return createElement('button',{
				on: {
					click: function(){
						console.log(context)
						console.log(context.parent) // 父组件
						console.log(context.parent.msg) 
						console.log(context.props.value)
					}
				}
			},'点击学习context')
		},
		props: ['value']
	})
	var app = new Vue({
        el: "#app",
        data: {
        	msg: '我是父组件的内容'
        }
    })
</script>
```
>使用 context 的转变：
>`this.text` —— `context.props.text`
>`this.$slots.default` —— `context.children`

