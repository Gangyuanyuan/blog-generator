---
title: Vue 组件详解之插槽与动态组件
date: 2019-08-20 14:43:30
tags: Vue
categories: 前端探索
---

## 使用 slot 分发内容
### 什么是 slot (插槽)
为了让组件可以组合，我们需要一种方式来混合父组件的内容和子组件自己的模板。这个过程被称为“内容分发”。Vue.js 实现了一个内容分发 API —— 使用 `slot` 元素作为原始内容的插槽。

### 编译的作用域
在深入内容分发 API 之前，我们先明确内容在哪个作用域里编译。假定模板为：
```
// 父组件作用域
<div id="app">
    <my-component> {{message}} </my-component>
</div>
var app = new Vue({
    el: "#app", 
    components: {
        'my-component': {
            // 子组件作用域
            template: '<div>我是子组件</div>'
        }
    }
})
```
这里的 `message` 应该绑定到的是父组件的数据。组件作用域简单地说是：
父组件模板的内容在父组件作用域内编译；
子组件模板的内容在子组件作用域内编译。

### 插槽的用法
插槽将父组件的内容与子组件的模板相混合，从而弥补了视图的不足。
+ 如果想在组件中添加内容，则必须在组件注册时在 `template` 中添加插槽 `slot`。
+ 如果父组件没有插入内容，那么 `slot` 的内容就作为默认出现；若父组件插入了内容，则 `slot` 的内容将被插入的内容替换掉。
1. 单个插槽
```
<div id="app">
    <my-component>
        <p>我是父组件的内容</p>
    </my-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        components: {
            'my-component': {
                template: '<div>\
                            <slot>\
                                如果父组件没有插入内容，我就作为默认出现\
                            </slot>\
                        </div>'
            }
        }
    })
</script>
```
2. 具名插槽
有时我们会根据不同的视图来指定使用某个插槽，具名插槽用法如下：
```
<div id="app">
    <name-component>
        <h3 slot="header">我是标题</h3>
        <p>正文第一段</p>
        <p>正文第二段</p>
        <p slot="footer">我是尾部信息</p>
    </name-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        components: {
            'name-component': {
                template: '<div>\
                            <div class="header">\n' +
                                '<slot name="header">\n' +
                                '\n' +	
                                '</slot>\n' +
                            '</div>\n' +
                            '<div class="container">\n' +
                                '<slot>\n' +
                                '\n' +
                                '</slot>\n' +
                            '</div>\n' +
                            '<div class="footer">\n' +
                                '<slot name="footer">\n' +
                                '\n' +	
                                '</slot>\n' +
                            '</div>\n' +
                        '</div>'
            }
        }
    })
</script>
```
如上所示，可以给 `slot` 定义 `name` 属性。渲染模板时可以指定使用某个 `slot`，若没有指定，则使用没有 `name` 的默认 `slot`。

### 作用域插槽
作用域插槽是一种特殊的 `slot`，使用一个可以复用的模板来替换已经渲染的元素，即通过 `slot` 从子组件获取数据（但 `name` 不可以获取）。
+ `slot-scope="prop"` 中的 `prop` 是自定义名称 。
+ 从子组件获取内容一定要使用 `template` 标签（Vue.js 2.5.0 之前的版本），`template` 模板是不会被渲染的；Vue.js 2.5.0 之后的版本在任意标签下都可以。
```
<div id="app">
    <my-component>
        <template slot="abc" slot-scope="prop"> <!-- prop是自定义的名称 -->
            {{prop.text}}
            {{prop.ss}}
        </template>
    </my-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        components: {
            'my-component': {
                template: '<div>\
                        <slot name="abc" text="我是来自子组件的内容" ss="adhsfh">\
                        </slot>\
                    </div>'
            }
        }
    })
</script>
```
### 访问 slot
通过 `this.$slots.(NAME)` 进行访问。
```
mounted: function(){
    // 访问插槽
    var header = this.$slots.header
    var text = header[0].elm.innerText
    var html = header[0].elm.innerHTML
    console.log(header)
    console.log(text)
    console.log(html)
}
```

## 组件高级用法 --- 动态组件
动态组件在实际开发中是用的比较多的。
VUE 给我们提供了一个非常特殊的元素叫 `component`，作用是用来动态的挂载不同的组件，并通过绑定 `is` 特性来实现。
+ 例：通过点击不同按钮切换不同的视图
```
<div id="app">
    <component :is="thisView"></component>
    <button @click="handleView('A')">第1句</button>
    <button @click="handleView('B')">第2句</button>
    <button @click="handleView('C')">第3句</button>
    <button @click="handleView('D')">第4句</button>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            thisView: "compA" // 绑定默认值
        },
        components: {
            'compA': {
                template: '<div>白日依山尽</div>'
            },
            'compB': {
                template: '<div>黄河入海流</div>'
            },
            'compC': {
                template: '<div>欲穷千里目</div>'
            },
            'compD': {
                template: '<div>更上一层楼</div>'
            }
        },
        methods: {
            handleView: function(tag){
                this.thisView = 'comp' + tag
            }
        }
    })
</script>
```