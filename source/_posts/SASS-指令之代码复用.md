---
title: SASS 指令之代码复用
date: 2019-11-05 23:02:02
tags: SASS
categories: 前端探索
---

为了有效的维护和开发项目，代码的重复利用就显得尤为重要。在 SASS中，除了 [`@import`](http://vanseodesign.com/css/sass-the-import-directive/) 和 [`@extend`](http://vanseodesign.com/css/sass-the-extend-directive/) 可以使代码重用性更强，还有一些指令也同样能提高代码的重复使用率并简化代码，如 @mixin，%，@function 等。
## @mixin
@mixin指令是一种简化代码的方法，它可以包含任意内容且可以传递参数。将共用属性声明成 `@mixin`，并给它一个名字，然后用 `@include` 引入到需要使用的代码中即可。
1. 常规写法

html
```
<div class="box smallBox"></div>
<div class="box BigBox"></div>
```
scss
```
.box{
    box-shadow: 0 0 5px black;
    margin: 10px;
    &.smallBox{
        width: 100px;
        height: 100px;
    }
    &.bigBox{
        width: 300px;
        height: 300px;
    }
}
```
2. @mixin 写法

html
```
<div class="smallBox"></div>
<div class="BigBox"></div>
```
scss
```
@mixin box{
	box-shadow: 0 0 5px black;
	margin: 10px;
}
.smallBox{
	width: 100px;
	height: 100px;
	@include box;
}
.bigBox{
	width: 300px;
	height: 300px;
	@include box;
}
```
3. @mixin 更高级写法：设置参数
```
@mixin box($width: 100px, $height:100px){
	box-shadow: 0 0 5px black;
	margin: 10px;
	width: $width;
	height: $height;
}
.smallBox{
	@include box(); // 默认宽高:100px
}
.bigBox{
	@include box(300px, 300px);
}
```

## %placeholder （占位符选择器）
%placeholder 写法
```
%box{
	box-shadow: 0 0 5px black;
	margin: 10px;
}
.smallBox{
	width: 300px;
	height: 300px;
	@extend %box;
}
.bigBox{
	width: 100px;
	height: 100px;
	@extend %box;
}
```
### % 与 @mixin 的区别
+ `@mixin` 支持参数，`%` 不支持参数
+ 生成的 css 代码：`@include` 会将 `@mixin` 的共用代码（属性）复制过去，`%placeholder` 是将选择器复制到共用代码之前。尽可能多的使用 `%placeholder`，可以使代码更少。

## @function
@function 可以自定义一个函数，使属性值的计算逻辑变的可复用。
```
%box{
	box-shadow: 0 0 5px black;
	margin: 10px;
}
@function px($value){
	@return $value/2 + px;
}
.smallBox{
	width: px(200);
	height: px(200);
	@extend %box;
}
.bigBox{
	width: px(600);
	height: px(600);
	@extend %box;
}
```
