---
title: vue-router 路由和前端状态管理
date: 2019-09-09 23:08:26
tags: Vue
categories: 前端探索
---

## vue-router 路由基本加载
路由，通俗地来讲就是输入不同的网址，加载不同的组件。
1. 进入项目目录
```
cd my-project
```
2. 安装
```
npm install -save vue-router
```
3. 在文件中**引用**
```
import router from 'vue-router'
Vue.use(router)
```
4. 配置路由文件，并在 vue 实例中注入
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
5. 确定视图加载的位置
```
<router-view></router-view>
```

## vue-router 路由的跳转
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

## vue-router 路由参数的传递
>+ 必须在路由中加入路由的 `name` 属性
>+ 必须在 `path` 后加 `/: + 传递的参数`
>+ 传递参数和接收参数见下方代码

### 配置路由
```
export default new router({ // 导出路由
  routes: [{
    name: 'helloworld',
    path: '/helloworld/:worldmsg', // 指定要跳转的路径
    component: HelloWorld // 指定要跳转的组件
  }, {
    name: 'helloearth',
    path: '/helloearth/:earthmsg',
    component: HelloEarth
  }]
})
```
### 传递和读取参数（两种方法）
1. 方法一
+ 传递参数：
```
<router-link :to="{name:'helloworld', params:{worldmsg:'你好世界'}}">
    HELLO WORLD
</router-link>
<router-link :to="{name:'helloearth', params:{earthmsg:'你好地球'}}">
    HELLO EARTH
</router-link>
```
+ 读取（接收）参数：`$route.params.xxx`
要往哪个组件跳转，就由哪个组件来接收。
```
我是传递过来的参数：<h3>{{$route.params.worldmsg}}</h3> 
```
2. 方法二（不常用）
+ 传递参数：
```
<router-link :to="{path:'/helloearth', query:{worldmsg:'你好世界'}}">
    HELLO WORLD
</router-link>
```
+ 读取（接收）参数：函数模式
可以创建一个函数返回 `props`，这样便可以将参数转换成另一种类型，将静态值与基于路由的值结合等。
```
const router = new VueRouter({
  routes: [{
    path: '/search',component: SearchUser, props: (route) => ({
        query: route.query.q })
  }]
})
```
3. 两种传递方式的**区别**：
+ params 传递方式：`http://localhost:8080/helloworld/你好世界`
+ query 传递方式：`http://localhost:8080/helloworld?name=xx&count=xxx`

## Axios 详解
### Axios 简介
axios 是一个基于 Promise 用于浏览器和 node.js 的 HTTP 客户端，它的功能与 ajax 相似，其本身具有以下特征：
>+ 在浏览器中创建 XMLHttpRequest
>+ 从 node.js 发出 http 请求
>+ 支持 Promise API
>+ 拦截请求和响应
>+ 转换请求和响应数据
>+ 取消请求
>+ 自动转换 JSON 数据
>+ 客户端支持防止 CSRF/XSRF

### Axios 之 get 请求详解
1. 安装
```
npm install axios
```
2. 在文件中引入加载
```
import axios from 'axios'
```
3. 将 axios 全局挂载到 VUE 原型上
```
Vue.prototype.$http = axios // $ 后面的内容自定义
```
4. 发送请求
+ 以 CNode 社区官方 API 为例：
```
getData(){
  this.$http.get('https://cnodejs.org/api/v1/topics')
    .then((res) => {
      this.items = res.data.data
      console.log(res)
    })
    .catch(function(err){
      console.log(err)
    })
}
```
+ GET 请求的两种参数传递形式：
```
axios.get('https://cnodejs.org/api/v1/topics', {
  params: {      // 只有一个参数时可省略params直接写参数
    page: 1,
    limit: 10
   }
})

axios.get('https://cnodejs.org/api/v1/topics?page=1&limit=15')
```

### Axios 之 post 请求详解
POST 请求和 GET 类似，将请求中的 `get` 替换成 `post` 即可。
+ POST 请求传递数据的两种格式：
```
form-data?page=1&limit=48

x-www-form-urlencoded {
  page: 1,
  limit: 10
}
```
>**注意**：在 axios 中，post 请求接收的参数必须是 `form-data` 格式
>需要使用 qs 插件的 `-qs.stringify` 方法。
>+ 使用步骤：
>安装 qs 插件：`npm install qs`
>在文件中引入：`import qs from 'qs'`
>在 post 请求中使用：`qs.stringify({ page: 1, limit: 10 })`

+ 以 CNode 社区官方 API 为例：
```
postData(){
  this.$http.get(url, qs.stringify({
      page: 1,
      limit: 10
  })).then((res) => {
      this.items = res.data.data
      console.log(res)
    })
    .catch(function(err){
      console.log(err)
    })
}
```

## Vuex 之 store
Vuex 是用来管理状态，共享数据，在各个组件之间管理外部状态的一个插件。例如用户的已登录状态，需要在各个组件之前进行交互。Vuex 首先会创建一个状态仓库，并将所有组件囊括其中，即这个仓库下的所有组件都可以共享仓库里的状态。
+ 使用步骤：
1\. 安装 vuex：`npm install vuex`
2\. 在 main.js 文件中引入并通过 `use` 方法使用 vuex：
`import Vuex from 'vuex'`， `Vue.use(Vuex)`
3\. 创建状态仓库
4\. 将 `store` 注入到 Vue 实例
5\. 通过 `this.$store.state.xxx` 拿到需要的数据
```
// 创建状态仓库（此处名称store可自定义）
var store = new Vuex.Store({
  state:{
    xxx：yyy
  }
})
```
## Vuex 的相关操作
### 改变状态
除了能够获取状态，那么如何改变状态呢？
我们用 `state` 存放定义的状态，用 `mutations` 来改变状态。
+ `mutations` 用法
```
// 创建状态仓库
var store = new Vuex.Store({
  state: {
    num: 88
  },
  mutations: {
    // 定义状态改变函数
    zzz: function(state){
      state.num++
    }
  }
})

// 调用方式
this.$store.commit('zzz') // zzz 是在 mucations 中定义的方法名
```
+ `actions` 用法：
```
var store = new Vuex.Store({
  state: {
    num: 88
  },
  mutations: {
    zzz: function(state){
      state.num++
    }
  },
  actions:{
    // actions 中只能对 mutation 进行操作
    ccc: function(context){ // context是上下文对象
      context.commit('zzz')
    }
   }
})

// 调用方式
this.$store.dispatch('ccc')
```
**注意**：
`actions` 提交的是 `mutation`，而不是直接变更状态。
`actions` 可以包含异步操作，但是 `mutation` 只能包含同步操作。
+ `getters` 用法：
```
var store = new Vuex.Store({
  state: {
    num: 88
  },
  mutations: {
    zzz: function(state){
      state.num++
    }
  },
  getters: {
    getNum: function(state){
      return state.num>0 ? state.num : 0
    }
  }
})

// 调用方式
this.$store.getters.getNum
```
### vuex 状态管理流程
vuex 状态管理流程为：
>view ——> actions ——> mutations ——> state ——> view

+ 从视图出发，先是 actions，然后是 mutations，然后通过 mutations 来操作 state，最后再回到 view。
+ 直接对状态进行操作的是 mutations，不是 actions。
+ 在这个过程中，actions 不是必须存在的，但如果有异步操作，则必须包含 actions。

