---
title: Vue 自定义指令
date: 2019-08-29 15:37:38
tags: Vue
categories: 前端探索
---

## 自定义指令基本用法
Vue 中已有的指令往往不能满足我们开发过程中的全部要求，因此有时我们需要自定义一些指令来实现某些特有的功能。
和组件类似，自定义指令也分为全局注册和局部注册，二者区别就是把`component` 换成了 `directive`。例：
```
Vue.directive('指令名称', {
  // 指令的选项
});
```

## 钩子函数
Vue 提供了几个钩子函数作为自定义指令的选项：
+ `bind`：只调用一次，指令第一次绑定到元素时调用。用这个钩子函数可以定义一个在绑定时执行一次的初始化动作。
+ `inserted`：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 `document` 中）。
+ `update`：被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新（详细的钩子函数参数见下）。
+ `componentUpdated`：被绑定元素所在模板完成一次更新周期时调用。
+ `unbind`：只调用一次， 指令与元素解绑时调用。

## 钩子函数的参数
+ `el`：指令所绑定的元素，可以用来直接操作 DOM 。
```
<div id="app">
	获取焦点：<input type="text" v-focus> <br>
</div>
<script>
	// 需求：input 框在初始化时就获取焦点
	Vue.directive('focus', {
		// 指令的选项
		inserted: function(el){  // 插入到父节点时就调用
			el.focus()
		}
	});
	var app = new Vue({
        el: "#app", 
    })
</script>
```
+ `binding`：一个对象，包含以下属性：
--- `name`：指令名，不包括 `v-`­ 前缀。
--- `value`：指令的绑定值， 例如 `v-apple="1 + 1"`, `value` 的值是 `2`。
--- `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用，无论值是否改变都可用。
--- `expression`：绑定值的字符串形式。 例如 `v-apple="1 + 1"` ，`expression` 的值是 `1 + 1`。
--- `arg`：传递给指令的参数。例如 `v-apple:pear`， `arg` 的值是 `pear`。
--- `modifiers`：一个包含修饰符的对象。 例如 `v-apple:pear.a.b.c`，修饰符对象 `modifiers` 的值是 `{"a":true,"b":true,"c":true}`。
```
<div id="app">
	<!-- 自定义指令 -->
	<div v-apple:pear.a.b.c="red"></div>
</div>
<script>
	Vue.directive('apple', {
		bind: function(el, binding){
			el.innerHTML =
			'name' + ' --- ' + binding.name + '<br>' +
			'value' + ' --- ' + binding.value + '<br>' +
			'expression' + ' --- ' + binding.expression + '<br>' +
			'argument' + ' --- ' + binding.arg + '<br>' +
			'modifiers' + ' --- ' + JSON.stringify(binding.modifiers) + '<br>'
		}
	});
	var app = new Vue({
        el: "#app", 
        data: {
        	red: '我是自定义指令所绑定的值'
        }
    })
</script>
```
运行结果为：
![自定义指令 --- binding](https://upload-images.jianshu.io/upload_images/13038962-1ee20122dc5cfef4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ `vnode`：Vue 编译生成的虚拟节点。
```
<div id="app">
	<div v-apple:pear.a.b.c="red"></div>
</div>
<script>
	Vue.directive('apple', {
		bind: function(el, binding, vnode){
			var keys = [];
			for(var key in vnode){
				keys.push(key)
			};
			el.innerHTML =
				'vnode 中的 keys：' + keys.join("--") + '<br>'
		}
	});
	var app = new Vue({
        el: "#app", 
        data: {
        	red: '我是自定义指令所绑定的值'
        }
    })
</script>
```
运行结果为：
![自定义指令 --- vnode](https://upload-images.jianshu.io/upload_images/13038962-cef36f8ca0227aa0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


+ `oldVnode`: 上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。
