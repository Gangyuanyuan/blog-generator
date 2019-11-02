---
title: Babel 的安装及使用方法
date: 2019-11-02 22:48:58
tags: Webpack
categories: 前端探索
---

Babel 是一个 JavaScript 编译器，它可以将 ECMAScript 2015+ 版本的 JavaScript 代码编译为能在旧版浏览器上工作的代码，包含新语法的转换和缺失特性的修补。

## 安装 Babel
### 局部安装 Babel
局部安装的默认路径为 `./node_modules/.bin/babel`。
1. 进入项目：
```
cd xxx
```
2. 初始化，生成 package.json 文件（遇到需要填写的问题直接回车）：
```
npm init
```
3. 安装所需的包（Babel 7）：
```
npm install --save-dev @babel/core @babel/cli @babel/preset-env
npm install --save @babel/polyfill
```
+ 可以用以下命令查询所安装的版本号：
```
./node_modules/.bin/babel -V
```
### 模块释义
+ `@babel/core`：Babel 的核心库，包含 Babel 的核心功能
+ `@babel/cli`：命令行工具，使用它从终端运行 Babel
+ `@babel/env preset`：一组预设的插件组，只对我们所使用的并且目标浏览器中缺失的功能进行代码转换和加载 polyfill
+ `@babel/polyfill`：用来模拟所有新的 JavaScript 功能

## 配置及使用
1. 在项目的根目录下创建一个命名为 babel.config.js 的配置文件，其内容为
```
const presets = [
  [
    "@babel/env",
    {
      targets: {
        edge: "17", firefox: "60", chrome: "67", safari: "11.1",
      },
      useBuiltIns: "usage",
    },
  ],
];

module.exports = { presets };
```
以上是直接指定支持的浏览器版本号，支持多种配置方式，可自行调整。
2. 将 js 目录下的所有代码编译到 dist 目录：
```
./node_modules/.bin/babel js --out-dir dist
```
3. 由于我们配置了 `useBuiltIns: usage`，所以执行编译时会出现警告，根据提示再安装配置一下 core-js：
+ 安装 core-js 2：
```
npm install --save core-js@2
```
+ 修改 babel.config.js 配置文件：
```
const presets = [
  [
    "@babel/env",
    {
      targets: {
        edge: "17", firefox: "60", chrome: "67", safari: "11.1",
      },
      useBuiltIns: "usage",
      corejs: 2
    },
  ],
];

module.exports = { presets };
```
这时再执行编译命令，警告就会消除。
4. 也可以用 `watch` 选项开启自动编译（需要一直开启），js 文件一旦发生变动就会同步更新到 dist 目录：
```
./node_modules/.bin/babel js --out-dir dist --watch
```
5. `./node_modules/.bin/babel` 命令也可以用 `npx babel` 来代替（npm@5.2.0 所自带的 npm 包运行器，它会自动寻找 babel 的局部安装路径）。
