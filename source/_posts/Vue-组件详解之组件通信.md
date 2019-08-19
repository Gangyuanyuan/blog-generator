---
title: Vue 组件详解之组件通信
date: 2019-08-18 10:13:58
tags: Vue
catagories: 前端探索
---

组件中的关系可分为父子组件通信、兄弟组件通信和跨级组件通信。

## 自定义事件 ---- 子组件给父组件传递数据
我们用 `props` 来接收父组件传递给子组件的数据，那么子组件如何给父组件传递数据呢？
JavaScript 的一种设计模式 一一 观察者模式，包含 `dispatchEvent` 和 `addEventListener` 这两个方法。Vue 组件也有与之类似的一套模式，子组件用 `$emit()` 来**触发事件**，父组件用 `$on()` 来**监听子组件**的事件 。
+ `v­-on` 除了监听 DOM 事件外，还可以用于组件之间的自定义事件。
>+ 第一步：在子组件标签上自定义事件
>+ 第二步： 在子组件的方法中用 `$emit()` 触发事件，第一个参数是事件名，后面的参数是要传递的数据（可以有 n 个参数）
>+ 第三步：在父组件的 `methods` 的自定义事件中用一个参数来接受
```
<div id="app">
    您现在的银行卡余额是：{{total}} 元
    <son-component @change="handleTotal"></son-component> <!-- change为自定义事件 -->
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            total: 2000
        },
        // 需求: 通过加号按钮和减号按钮来给父组件传递数据
        components: {
            'son-component': {
                template: '<div>\
                            <button @click="handleincrease">+1000</button>\
                            <button @click="handlereduce">-1000</button>\
                        </div>',
                data: function(){
                    return {
                        count: 2000
                    }
                },
                methods: {
                    handleincrease: function(){
                        this.count = this.count + 1000
                        // 触发change事件，并把 this.count 传递过去
                        this.$emit('change',this.count) 
                    },
                    handlereduce: function(){
                        this.count = this.count - 1000
                        this.$emit('change',this.count)
                    }
                },
            }
        },
        methods: {
            handleTotal: function(value){
                // 此处的形参 value 就是传递过来的数据 count
                this.total = value
            }
        }
    })
</script>
```

## 在组件中使用 v-­model
1. `v-­model` 其实是一个语法糖，这背后其实做了两个操作：
+ `v-­bind` 指令绑定一个 `value` 属性
+ `v-­on` 指令给当前元素绑定 `input` 事件

2. 使用 `v-­model` 要做到：
+ 接收一个 `value` 属性
+ 在有新的 `value` 时触发 input 事件
```
<div id="app">
    您现在的银行卡余额是：{{total}} 元
    <son-component v-model="total"></son-component>
    v-model 其实就是绑定了 input 事件， 当触发 input 时，<br>
    input 事件就会自动接收传递过来的参数， 并赋值给已经绑定的 total。
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            total: 2000
        },
        components: {
            'son-component': {
                template: '<div>\
                            <button @click="handleincrease">+1000</button>\
                        </div>',
                data: function(){
                    return {
                        count: 2000
                    }
                },
                methods: {
                    handleincrease: function(){
                        this.count = this.count + 1000
                        this.$emit('input',this.count)
                    }
                },
            }
        }
    })
</script>
```
`this.$emit('input',this.count)` 这行代码会触发一个 `input` 事件，后面的参数就是传递给 `v­-model` 绑定的属性 `total` 的值。

## 非父组件之间的通信
+ 官网描述：![官网描述](https://upload-images.jianshu.io/upload_images/13038962-b1cb9814975106fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ 图形实例：子组件 A 与子组件 B 之间的通信![图形实例](https://upload-images.jianshu.io/upload_images/13038962-8abf2c5466b0ce4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1. 非父组件之间进行通信时（如将 A 组件数据传递给 B 组件），首先需要在根组件中定义一个 `bus` 中介，然后在 A 组件中触发一个事件并将参数传递过去 ，B 组件在实例创建的时候就监听这个事件，并接收传递进来的参数。
```
<div id="app">
    <my-acomponent></my-acomponent>
    <my-bcomponent></my-bcomponent>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            bus: new Vue()
        },
        components: {
            'my-acomponent': {
                template: '<button @click="handle">点击我向B组件传递数据</button>',
                data: function(){
                    return {
                        amessage: '我是来自组件 A 的内容'
                    }
                },
                methods: {
                    handle: function(){
                        this.$root.bus.$emit('lala', this.amessage)
                    }
                }
            },
            'my-bcomponent': {
                template: '<div></div>',
                created: function(){
                    // A组件在实例创建的时候就监听事件---lala事件
                    this.$root.bus.$on('lala', function(value){
                        alert(value)
                    })
                }
            }
        }
    })
</script>
```
2. 父链：`this.$parent`
子组件访问父组件的内容：
```
<div id="app">
    <child-component></child-component> --- {{msg}}
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            msg: '数据未修改'
        },
        components: {
            'child-component': {
                template: '<button @click="setParentData">点击修改父组件内容</button>',
                methods: {
                    setParentData: function(){
                        this.$parent.msg = "数据已经修改了"
                    }
                }
            }
        }
    })
</script>
```
3. 子链：`this.$refs`
父组件访问子组件内容，用 `this.$children` 会找到所有的子组件。Vue 提供了为子组件提供索引的方法，用 `ref` 来指定一个属性名称，即为其增加一个索引。
```
<div id="app">
    <my-acomponent ref="a"></my-acomponent>
    <button @click="getChildData">我是父组件的按钮，我要拿到子组件内容</button> --- {{fromChild}}
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            fromChild: '还未拿到'
        },
        methods: {
            getChildData: function(){
                this.fromChild = this.$refs.a.amessage
            }
        },
        components: {
            'my-acomponent': {
                template: '<div></div>',
                data: function(){
                    return {
                        amessage: '我是来自组件 A 的内容'
                    }
                }
            }
        }
    })
</script>
```
