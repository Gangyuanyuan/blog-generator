---
title: vue-router路由和前端状态管理
date: 2019-09-09 23:08:26
tags: Vue
categories: 前端探索
---

## vue-­router 路由基本加载
路由，通俗地来讲就是输入不同的网址，加载不同的组件。
1. 进入项目目录**安装**
```
npm install --save vue-router
```
2. 在文件中**引用**
```
import router from 'vue-router'
Vue.use(router)
```
3. 配置路由文件，并在 vue 实例中注入
```
var rt = new router({
  routes: [{
    path: '/', // 指定要跳转的路径
    component: HelloWorld // 指定要跳转的组件
  }]
})
new Vue({
  el: '#app',
  router: rt,
  components: { App },
  template: '<App/>'
})
```
4. 确定视图加载的位置
```
<router-view></router-view>
```

## vue-­router 路由的跳转
```
<router-link to="/"></router-link>
```
例：
```
<template>
  <ul>
    <li>
      <router-link to="/helloworld">HELLO WORLD</router-link>
    </li>
    <li>
      <router-link to="/helloearth">HELLO EARTH</router-link>
    </li>
  </ul>
</template>
```

