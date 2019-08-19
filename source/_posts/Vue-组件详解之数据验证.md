---
title: Vue 组件详解之数据验证
date: 2019-08-15 09:21:07
tags: Vue
categories: 前端探索
---

## camelCased（驼峰式命名）与 kebab--case（短横线命名）
+ 在 html 中渲染组件时不可以使用 camelCased（驼峰式命名）。因为在 html 中 `myComponent` 和 `mycomponent` 是一致的（除非被双引号包裹），所以在组件的 html 中必须使用 kebab--case（短横线命名）。
+ 在组件中，父组件给子组件传递数据必须用短横线命名。
+ 在组件的 `template` 中，必须使用驼峰式命名；若为短横线命名方式，则会直接报错。
+ 在组件的 `data` 中，用 `this.xxx` 引用时，只能使用驼峰式命名；若为短横线的命名方式，则会报错。

## 数据验证
数据验证主要是对 `props` 传递进来的数据进行类型的验证，并可以对其设定一个选项，如设置默认值或特定的数据类型。

###### 可验证的 type 类型包括：
+ String
+ Number
+ Boolean
+ Object
+ Array
+ Function

数据验证示例：
```
<div id="app">
    <type-component :a="a" :b="b" :c="c" :d="d" :f="f" :g="g"></type-component>
</div>
<script>
    var app = new Vue({
        el: "#app", 
        data: {
            a: '1',
            b: '666',
            c: true,
            d: 12345,
            e: [],
            f: 88,
            g: console.log()
        },
        components: {
            'type-component': {
                props: {
                    // 必须是数字类型
                    a: String,
                    // 必须是字符串或数字类型
                    b: [String,Number],
                    // 布尔类型，如果没有定义，默认值就是true
                    // 参数：type 类型，required 是否必传，default 默认值
                    c: {
                        type: Boolean,
                        default: true
                    },
                    // 数字类型，而且d在页面中必传，不传则报错
                    d: {
                        type: Number,
                        required: true
                    },
                    // 数组或对象，默认值必须用函数来返回（页面不绑定e时取默认值）
                    e: {
                        type: Array,
                        default: function(){
                            return [666]
                        }
                    },
                    // 自定义一个验证函数
                    f: {
                        validator: function(value){
                            return value>10
                        }
                    },
                    // g必须传函数，否则报错
                    g: {
                        type: Function
                    }
                },
                template: '<div>{{a}}--{{b}}--{{c}}--{{d}}--{{e[0]}}</div>'
            }
        }
    })
</script>
```