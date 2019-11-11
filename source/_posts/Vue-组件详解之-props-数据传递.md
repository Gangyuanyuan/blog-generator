---
title: Vue 组件详解之 props 数据传递
date: 2019-08-14 10:24:15
tags: Vue
categories: 前端探索
---

组件的作用不仅仅是将模板的内容进行复用，组件与组件之间的通信也尤为重要。
## props - 父组件向子组件传递数据
### 在子组件中使用 props 从父组件接收参数
注意：在 `props` 中定义的属性，都可以在组件中直接使用。
```
在父组件里向子组件传递消息：
<div id="app" style="border: 2px solid orange; height: 120px;">
    <h5 style="text-align: center;">我是父组件</h5>
    <child-component message="我是来自父组件的内容"></child-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        components: {
            'child-component': {
                props: ['message'],
                template: '<div style="border: 2px solid green; height: 60px;">{{message}}</div>'
            }
        }
    })
</script>
```
+ 组件中 `props` 中定义的内容是来自父级的，而组件中 `data return` 的数据是组件自己的数据。两种情况 **作用域** 都是组件本身，可以在 `template`，`computed`，`methods` 中直接使用。
+ `props` 的值有两种，一种是字符串数组，一种是对象。

### 可以使用 v­-bind 动态绑定来自父组件的内容
首先在 `input` 中用 `v-model` 绑定父组件 `data` 中的数据，然后把绑定的数据同时传递给子组件，使用 `v-bind` 绑定属性，在子组件中用 `props` 进行接收，并且可以在 `template` 中直接使用。
```
使用v-bind进行数据的动态绑定 --- 把input中的message传递给子组件：
<div id="app" style="border: 2px solid orange; height: 120px;">
    <input type="text" v-model="parentmsg">
    <bind-component v-bind:message="parentmsg"></bind-component>
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            parentmsg: '今天的月亮真圆啊'
        },
        components: {
            'bind-component': {
                props: ['message'],
                template: '<div style="border: 2px solid green; height: 60px;">{{message}}</div>'
            }
        }
    })
</script>
```
+ 传递数据时是否使用 `v-bind` 结果是有区别的（还可以进行布尔值、对象进行测试）：
```
v-bind 使用与否的区别：
<div id="app" style="border: 2px solid orange; height: 120px;">
    <child-component message="[3,6,9]"></child-component>
    <child-component v-bind:message="[3,6,9]"></child-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        components: {
            'child-component': {
                props: ['message'],
        		template: '<div style="border: 2px solid green; height: 60px;">{{message}}  length:{{message.length}}</div>'
            }
        }
    })
</script>
```
得到的结果如下：![v-bind 使用与否的区别](https://upload-images.jianshu.io/upload_images/13038962-9a14b7e44456b082.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)由上例可知，不使用 `v-bind` 时，Vue 默认以字符串的形式向组件传递数据；当使用 `v-bind` 时，Vue 将对传递的内容进行识别，符合数组的样式时将按照数组来进行解析。

## 单向数据流
通过 `props` 来传递数据是单向的， 即父组件数据发生变化时会传递给子组件，但是反之不可以。
+ 目的：尽可能将父子组件解稿，避免子组件无意中修改了父组件的状态。
+ 应用场景：业务中会经常遇到两种需要改变 `props` 的情况：

>一种情况是父组件传递初始值进来，子组件将它作为初始值保存起来，在自己的作用域下可以随意使用和修改。可以在组件的 `data` 内再声明一个数据来引用父组件的 `props`。
步骤一：注册组件
步骤二：将父组件的数据传递进来，并在子组件中用 `props` 接收
步骤三：将传递进来的数据通过初始值保存起来

```
<div id="app">
    <my-component msg="我是父组件传递的数据"></my-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        components: {
            'my-component': {
                props: ['msg'],
                template: '<div>{{count}}</div>'
                data: function(){
                    return {
                        count: this.msg // props中的值可以通过this.xxx直接获取
                    }
                }
            }
        }
    })
</script>
```

>另一种情况是 `props` 作为需要被转变的原始值传入，然后用计算属性处理即可。
步骤一：注册组件
步骤二：将父组件的数据传递进来，并在子组件中用 `props` 接收
步骤三：将传递进来的数据通过计算属性进行重新计算

例：通过 input 输入框输入的数据直接改变 div 宽度：
```
<div id="app">
    请输入宽度：<input type="text" v-model="width">
    <width-component :width="width"></width-component>
</div>
<script>
    var app = new Vue({
        el: "#app",
        data: {
            width: ''
        },
        components: {
            'width-component': {
                props: ['width'],
                template: '<div :style="style"></div>',
                computed: {
                    style: function(){
                        return {
                            width: this.width + 'px',
                            height: '30px',
                            background: 'red'
                        }
                    }	
                }
            }
        }
    })
</script>
```
