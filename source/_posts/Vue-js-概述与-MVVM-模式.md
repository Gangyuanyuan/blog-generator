---
title: Vue.js 概述与 MVVM 模式
date: 2019-08-07 10:15:33
tags: Vue
categories: 前端探索
---

## Vue.js
### Vue.js 是什么
Vue.js 是一个轻巧、高性能、可组件化的MVVM库，拥有非常容易上手的 API；
Vue.js是一个构建数据驱动的 Web 界面的库。

### Vue.js 的特性
1\. 轻量级的框架
2\. 双向数据绑定
3\. 指令
4\. 插件化（组件化）

### MVVM 框架
+ MVVM（Model-View-ViewModel）是对 MVC（Model-View-Control）和 MVP（Model-View-Presenter）的进一步改进。
>『View』：视图层（UI 用户界面）
>『ViewModel』：业务逻辑层（一切 js 可视为业务逻辑）
>『Model』：数据层（存储数据及对数据的处理如增删改查）
+ MVVM 将数据双向绑定（data-binding）作为核心思想，View 和 Model 之间没有联系，它们通过 ViewModel 这个桥梁进行交互。

+ Model 和 ViewModel 之间的交互是双向的，因此 View 的变化会自动同步到 Model，而 Model 的变化也会立即反映到 View 上显示。
+ 当用户操作 View，ViewModel 感知到变化，然后通知 Model 发生相应改变；反之当 Model 发生改变，ViewModel 也能感知到变化，使 View 作出相应更新。
![MVVM](https://upload-images.jianshu.io/upload_images/13038962-96704c499078e5b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ [Angular](https://angularjs.org/) 和 [Ember](http://emberjs.com/) 都采用这种模式。

### Vue 的开发模式
+ 通过 script 标签直接引入 vue.js
+ 通过 Vue 的脚手架工具 vue-cli 来进行一键项目搭建

### Vue.js 的优点
+ 简单轻巧，功能强大，拥有非常容易上手的 API；
+ 可组件化 和 响应式设计
+ 实现数据与结构分离，高性能，易于浏览器的加载速度；
+ MVVM 模式，数据双向绑定，减少了 DOM 操作，将更多精力放在数据和业务逻辑上。

## 问答
### 简述 MVVM
+ MVVM 是 Model-View-ViewModel 的缩写。MVVM 是一种设计思想。Model 层代表数据模型，也可以在 Model 中定义数据修改和操作的业务逻辑；View 代表 UI 组件，它负责将数据模型转化成 UI 展现出来，ViewModel 是一个同步 View 和 Model 的对象。
+ 在 MVVM 架构下，View 和 Model 之间并没有直接的联系，而是通过 ViewModel 进行交互，Model 和 ViewModel 之间的交互是双向的， 因此 View 数据的变化会同步到 Model 中，而 Model 数据的变化也会立即反应到 View 上。
+ ViewModel 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。

### 简述 Vue.js 的优点
+ 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的 "View" 上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
+ 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 View 重用这段视图逻辑。
+ 独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计。
+ 方便测试。界面素来是比较难于测试的，开发中大部分 Bug 来至于逻辑处理，由于 ViewModel 分离了许多逻辑，可以对 ViewModel 构造单元测试。
+ 易用 灵活 高效。

