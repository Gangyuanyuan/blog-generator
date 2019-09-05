---
title: 使用 vue-cli 脚手架搭建项目
date: 2019-09-05 23:43:55
tags: Vue
categories: 前端探索
---

## 安装 Node.js
首先需要在电脑上安装最新稳定版本的 Node.js （[官网下载](https://nodejs.org/zh-cn/download/>)），安装 Node.js 时会随之安装一个包管理工具 `npm`，可以在命令行工具中用命令 `npm -v` 查看已安装版本。
安装完 Node.js 之后，可以安装淘宝 npm 镜像，因为我们使用国外的 npm 可能会受到网速的限制。
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
安装之后命令中的 `npm` 都用 `cnpm` 来代替。
## Vue-cli 脚手架搭建
+ 全局安装 vue­-cli（-g --- global）
```
npm install -g vue-cli
```
+ 进入目录（如 vueWebpackApp），初始化项目（项目名称自定义）
```
vue init webpack my-project
```
此时会出现以下选项：
```
? Project name vueapp          // 输入项目名称
? Project description demo     // 输入项目描述
? Author GY                    // 输入作者
? Vue build                    // 选择构建模式，直接回车选择第一条
> Runtime + Compiler: recommended for most users
  Runtime-only: about 6KB lighter min+gzip, but templates (or any Vue-specific
HTML) are ONLY allowed in .vue files - render functions are required elsewhere
? Install vue-router? No                 // 是否安装 vue-router
? Use ESLint to lint your code? No       // 是否安装 ESLint 代码检查工具
? Setup unit tests? No                   // 是否设置单元测试
? Setup e2e tests with Nightwatch? No    // 是否设置端到端测试
?Should we run `npm install` for you after the project has been created? // 直接回车
> Yes, use NPM
  Yes, use Yarn
  No, I will handle that myself
```
+ 进入项目
```
cd my-project
```
+ 安装依赖
```
npm install
```
+ 启动项目
```
npm run dev
```
完成后会出现：
```
Your application is running here: http://localhost:8080
```
打开这个地址 `http://localhost:8080`，你会发现通过 vue-cli 脚手架工具结合 webpack 搭建的项目已经完成了。![vue-cli 搭建项目](https://upload-images.jianshu.io/upload_images/13038962-ee48ed581fb7830f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 目录结构分析
+ build // **项目构建**（webpack）相关代码（9个）
── build.js // 生产环境构建代码
── check-­versions.js // 检查 node&npm 等版本
── dev­client.js // 热加载相关
── dev­server.js // 构建本地服务器
── utils.js // 构建配置公用工具
── vue-­loader.conf.js // vue 加载器
── webpack.base.conf.js // webpack 基础环境配置
── webpack.dev.conf.js // webpack 开发环境配置
── webpack.prod.conf.js // webpack 生产环境配置
+ config // 项目开发**环境配置**相关代码（3个）
── dev.env.js // 开发环境变量
── index.js // 项目的一些配置变量
── prod.env.js // 生产环境变量
+ node_modules // **项目依赖**的模块
+ src // 源码目录
── assets // 资源目录
── ── logo.png
── components // Vue 公共组件
── ── HelloWolrd.vue
── router // 前端路由
── ── index.js // 路由配置文件
── App.vue // 页面入口文件（根组件）
── main.js // 程序入口文件（入口 js 文件）
+ static // 静态文件，比如一些图片、json 数据等
── .gitkeep
+ 其余文件
── .babelrc // ES6 语法编译配置
── .editorconfig // 定义代码格式
── .gitignore // git 上传需要忽略的文件格式
── .postcssrc.js // 处理 css 文件
── index.html // 项目入口页面
── package.json // 项目基本信息
── README.md // 项目说明
