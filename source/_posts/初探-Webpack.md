---
title: 初探 Webpack
date: 2019-11-03 10:14:13
tags: Webpack
categories: 前端探索
---

## 关于 Webpack
### Webpack 是什么
Webpack 是一个现代 JavaScript 应用程序的模块打包器（module bundler），它把一切的内容包括 script 文件和静态文件都视为模块。Webpack 处理应用程序时，会递归地分析模块之间的依赖关系，然后使用各种 Loaders 处理它们，将所有这些模块打包成一个或多个 bundle，功能十分强大。
### Webpack 作用
前端模块化有利于代码的重用和维护，但又会引起模块与模块之间的依赖关系；而且服务器需要频繁地读取各个模块的 js 文件，导致加载速度相对较慢。而 Webpack 可以减少浏览器向服务器请求的次数，节约服务器的带宽资源，起到性能优化的作用。

## Webpack 安装及配置 
### Webpack
1. 进入项目
```
mkdir webpack-demo
cd webpack-demo
```
2. 初始化并创建 pakage.json 文件
```
npm init -y
```
`-y` ：yes，可以跳过中间的问题，直接在根目录生成 pakage.json；也可以不加 `-y` 一步步自行配置。
3. 安装 Webpack 3
```
npm install --save-dev webpack@3
```
安装完毕在 package.json 的 `devDependencies` 依赖中会出现  `"webpack": "^3.12.0"`。
4. 创建 src/index.js 文件
```
mkdir src
touch src/index.js
```
5. 创建 webpack.config.js，并进行简单的配置：
```
const path = require('path')
module.exports = {
  entry:  './src/index.js', // 入口文件配置
  output: { // 出口文件配置
    filename: "bundle.js",
    path: path.resolve(__dirname, 'dist') // 输出文件夹，须使用绝对地址
  }
}
```
6. 这时我们可以开始运行 webpack 了：
```
./node_modules/.bin/webpack
// 或者
npx webpack
```
这时会出现以下页面：![](https://upload-images.jianshu.io/upload_images/13038962-802e2fa8d2aa8bb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)说明 src/index.js 文件已被编译为 dist/bundle.js，但此时 Webpack 功能还不完善，只是对文件内容实现了简单的拷贝功能。此时 webpack-demo 的目录结构为：![](https://upload-images.jianshu.io/upload_images/13038962-55d04a173698dde8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### babel-loader
babel-loader 具有很强大功能，它允许使用 Babel 和 Webpack 转换 JavaScript 文件。Babel 的安装及使用可以参考我另一篇[文章](https://www.jianshu.com/p/789796367d47)。
#### 安装及配置 babel-loader
1. 安装 babel-loader：
```
npm install --save-dev babel-loader@8 @babel/core @babel/preset-env
```
2. 将以下配置写入 webpack-config.js 中：
```
module.exports = {
  // ......
  module: {
    rules: [
      {
        test: /\.js$/, // 以 .js 结尾的文件
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader', // 就使用 babel-loader 编译
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  }
}
```

#### 编译 js 文件
1. 创建下列 js 文件并写入代码
+ src/js/module1.js
```
function fn(){
	console.log(1)
}
// 如果此文件被引用，则默认把 fn 传过去
export default fn 
```
+ src/js/module2.js
```
function fn(){
	console.log(2)
}
// 如果此文件被引用，则默认把 fn 传过去
export default fn 
```
+ src/js/app.js
```
import x from './module-1'
import y from './module-2'
// x 和 y 就是 modelue-1 和 modelue-2 默认导出的 fn
x() 
y()
```
app.js 引入并使用了 module-1.js 和 module-2.js 这两个模块，我们只需要将 app.js 编译为 IE 可以运行的代码。
2. 重新配置 webpack-config.js 的入口文件和出口文件：
```
module.exports = {
  entry:  './src/js/app.js', // 入口文件配置
  output: { // 出口文件配置
    filename: "bundle.js",
    path: path.resolve(__dirname, 'dist/js/') 
  }
}
```
3. 再次运行 webpack，出现以下页面：![](https://upload-images.jianshu.io/upload_images/13038962-bc74605d97f289af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)这时 src/js 目录下的三个 js 文件被编译为 dist/js 目录下的一个bundle.js 文件了。

### sass-loader
sass-loader 用来加载 SASS/SCSS 文件并将其编译为 CSS 文件。
#### 安装及配置 sass-loader
1. 安装 sass-loader
```
npm install --save-dev sass-loader@6 node-sass
npm install style-loader@0  css-loader@0
```
2. 将以下配置写入 webpack-config.js 中：
```
module.exports = {
  // ......
  module: {
    rules: [
      { ...... },
      {
        test: /\.scss$/,  // 以 .scss 结尾的文件
        use: [{ // 顺序自下而上
          loader: "style-loader" // 将 JS 字符串生成为 style 节点
        }, {
          loader: "css-loader" // 将 CSS 转化成 CommonJS 模块
        }, {
          loader: "sass-loader" // 将 Sass 编译成 CSS
        }]
      }
    ]
  }
}
```

#### 编译 CSS 文件
1. 创建和修改下列文件
+ 创建 src/index.html 文件，内容如下，并拷贝到 dist 目录下：
```
<!DOCTYPE html>
<head></head>
<body>
  <div class="orange">
    <div class="blue">
      <div class="yellow"></div>
    </div>
  </div>
  <script src="./js/bundle.js"></script>
</body>
</html>
```
+ 创建 src/css/main.scss 文件，内容如下：
```
.orange{ width: 90px; height: 90px; background: orange;
  > .blue{ width: 60px; height: 60px; background: blue;
    > .yellow{ 
      width: 30px; height: 30px; background: yellow;
    }
  }
}
```
+ 在 src/js/app.js 中添加下面代码：
```
import '../css/main.scss'
```
2. 运行 webpack，出现以下页面：![](https://upload-images.jianshu.io/upload_images/13038962-81f258318398e5be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时 src/css/main.scss 也被以 CSS 字符串形式存编译加载到 dist/js/bundle.js 里了，bundle.js 运行时会将 CSS 字符串放入页面的 style 标签里，打开开发者工具可以查看：![](https://upload-images.jianshu.io/upload_images/13038962-8ee0739bd328cf22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### postcss-loader
postcss-loader 可以自动为 CSS 代码添加浏览器私有前缀，以更好地兼容各个版本的浏览器。
1. 安装 postcss-loader
```
npm i -D postcss-loader
npm install postcss-import postcss-cssnext
```
2. 创建 postcss.config.js 文件，并进行配置：
```
module.exports = {
  // parser: 'sugarss',
  plugins: {
    'postcss-import': {},
    'postcss-cssnext': {},
    'cssnano': {}
  }
}
```
3. 为 webpack.config.js 文件添加配置：
```
module.exports = {
  // ......
  module: {
    rules: [
      { ...... },
      {
        test: /\.scss$/,  // 以 .scss 结尾的文件
        use: [{ // 顺序自下而上
          loader: "style-loader" // 将 JS 字符串生成为 style 节点
        }, {
          loader: "css-loader", // 将 CSS 转化成 CommonJS 模块
          options: { importLoaders: 1 }
        }, {
          loader: "postcss-loader" // 为 CSS 添加前缀
        }, {
          loader: "sass-loader" // 将 Sass 编译成 CSS
        }]
      }
    ]
  }
}
```
4. 为 src/css/main.scss 文件中增加需要加前缀的代码，如：
```
body{
  display: flex; // 存在兼容性问题，如 IE 需要加前缀
}
```
5. 运行 webpack，刚才 scss 文件中添加的代码编译为 css 时添加了前缀：
```
body{
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
}
```
