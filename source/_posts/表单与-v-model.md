---
title: 表单与 v-model
date: 2019-08-11 11:05:34
tags: Vue
categorie: 前端探索
---

VUE 提供了 `v­-model` 指令， 用于在 **表单类** 的元素上双向绑定事件。
## v­-model 基本用法
### input 和 textarea
`v­-model` 可以用于 `input` 框，以及 `textarea` 多行文本框等。
```
<div id="app">
    <input type="text" v-model="value">
    {{value}}
    <textarea name="" id="" cols="30" rows="10" v-model="message">我是初始化值</textarea>
    {{message}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            value: '',
            message: ''
        }
    })
</script>
```
**注意**： 所显示的值只依赖于所绑定的数据，不再关心初始化时的插入的 `value` 值。

### 单选按钮
+ 单个单选按钮，直接用 `v-­bind` 绑定一个布尔值，用 `v­-model` 则不可以。
```
<div id="app">
    <input type="radio" v-bind:checked="oneradio">
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            oneradio: true,
        }
    })
</script>
```
+ 若多个单选框组合使用（用相同 name 实现多个单选框互斥），就需要 `v­-model` 来配合 `value` 使用，绑定选中的单选框的 value 值，此处所绑定的初始值可以随意设置。
```
<div id="app">
    <input type="radio" name="checks" value="小猫" v-model="checkname"> 小猫
    <input type="radio" name="checks" value="小狗" v-model="checkname"> 小狗
    <input type="radio" name="checks" value="小刺猬" v-model="checkname"> 小刺猬
    现在选中的是：{{checkname}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            checkname: '小猫' // 初始值
        }
    })
</script>
```
### 复选框
+ 单个复选框，直接绑定一个布尔值，可以用 `v-­model` 或 `v-­bind`。
+ 多个复选框
（1）若组合使用，需要 `v­-model` 配合 `value` 使用，`v­-model` 绑定一个数组；
（2）若绑定的是字符串，则会转化为 `true` 和 `false`，与**所有绑定的复选框**的 `checked` 属性相对应。
```
<div id="app">
    单个复选框---v-bind：<input type="checkbox" v-bind:checked="onecheck"> <br>
    单个复选框---v-model：<input type="checkbox" v-model="onecheckbox"> <br>
    多个复选框：
    <input type="checkbox" value="香蕉" v-model="checks"> 香蕉
    <input type="checkbox" value="苹果" v-model="checks"> 苹果
    <input type="checkbox" value="桃子" v-model="checks"> 桃子
    现在选中了：{{checks}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            onecheck: true,
            onecheckbox: true,
            cheks: []
        }
    })
</script>
```

### 下拉框
+ 单选的下拉框，所绑定的 `value` 初始化值可以为字符串，也可以为数组；若有 `value` 会直接优先匹配一个 `value` 值，没有 `value` 则匹配一个 `text` 值。
+ 多选的下拉框，需要 `v-­model` 配合 `value` 来使用，`v­-model` 绑定一个数组，与复选框类似。
**注意**：`v-­model` 一定是绑定在 `select` 标签上。
```
<div id="app">
	单选下拉框：
	<select v-model="selected">
		<option value="小猫">小猫</option>
		<option value="小狗">小狗</option>
		<option value="小刺猬">小刺猬</option>
	</select>
	现在选中的是 {{selected}}
	<br><br><br><br>
	多选下拉框（按Ctrl键点选）：
	<select v-model="selectedmul" multiple>
		<option value="小猫">小猫</option>
		<option value="小狗">小狗</option>
		<option value="小刺猬">小刺猬</option>
	</select>
	现在选中的是 {{selectedmul}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            selected: '',
            selectedmul: []
        }
    })
</script>
```
### 总结
+ 如果是单选，初始化最好给定字符串，因为 `v-­model` 此时绑定的是静态字符串或者布尔值。
+ 如果是多选，初始化最好给定一个数组。

## 绑定值
### 单选按钮
只需要用 `v-­bind` 给单个单选框绑定一个 `value` 值，此时 `v-­model` 绑定的就是它的 `value` 值。
```
<div id="app">
    <input type="radio" v-model="picked" v-bind:value="value"> ----{{picked}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            picked: true,
            value: '111', 
        }
    })
</script>
```
上例中，起初 `v-model` 并没有生效；手动选中单选框后，`v-­model` 绑定的 `picked` 会被替换成 `value` 的值。

### 复选框
动态控制，使复选框选中和未选中时的 `value` 不同：复选框选中时取的是 `true-value` 的值，未选中时取的是 `false-value` 的值。
```
<div id="app">
    <input type="checkbox" v-model="toggle" :true-value="value1" :false-value="value2">
    {{toggle}}
    被选中：{{toggle == value1}}
    未被选中：{{toggle == value2}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            toggle: true,
            value1: '我被选中了',
            value2: '我未被选中'
        }
    })
</script>
```
### 下拉框
在 select 标签上绑定 value 值对 option 并没有影响。
```
<div id="app">
    绑定在select上：
    <select v-model="valueselect1" :value="{num:111}">
        <option value="小猫">小猫</option>
    </select>
    现在选中的是 {{typeof valueselect1}} --- {{valueselect1}} <br>

    绑定在option上：
    <select v-model="valueselect2">
        <option value="小猫" :value="{num:111}">小猫</option>
    </select>
    现在选中的是 {{typeof valueselect2}} --- {{valueselect2}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            valueselect1: '',
            valueselect2: ''
        }
    })
</script>
```
## 修饰符
### lazy
`v-­model` 默认是在 input 输入时实时同步输入框的数据，而 `.lazy` 修饰符可以使其在失去焦点或敲回车键之后再更新。
### number
将输入的字符串转化为 `number` 类型。
### trim
`trim` 会自动过滤输入过程中首尾输入的空格。
```
<div id="app">
    1. lazy
    <input type="text" v-model="inputStr1"> --- {{inputStr1}} <br>
    <input type="text" v-model.lazy="lazyStr"> --- {{lazyStr}} <br>
    2. number  <br>
    <input type="text" v-model.number="inputStr2"> --- {{typeof inputStr2}} <br>
    <input type="text" v-model.number="isNum"> --- {{typeof isNum}} <br>
    3. trim <br>
    <input type="text" v-model="inputStr3"> length: {{inputStr3.split('').length}} <br>
    <input type="text" v-model.trim="trimStr"> length: {{trimStr.split('').length}} <br>
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            inputStr1: '',
            lazyStr: '',
            inputStr2: '',
            isNum: '',
            inputStr3: '',
            trimStr: ''
        }
    })
</script>
```
