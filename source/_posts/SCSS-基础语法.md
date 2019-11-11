---
title: SCSS 基础语法
date: 2019-11-11 10:08:21
tags: SASS
categories: 前端探索
---

## 选择器扩展
### BEM  命名法
BEM 全称为 Block Element Modifier，是一种 CSS 命名方法。
比如要给一个模块取名字，步骤如下：
1\. 先确定模块的名字，如 `user-card`
2\. 这一模块的子代或后代元素都围绕这个名字来命名，用两个下划线连接。如模块里面的头像用 `.user-card__picture`，名字用 `.user-card__name`，简介用 `.user-card__description`。
3\. 当某个元素发生状态的变化，如 hover 或 click 时样式改变，命名时用两个中划线连接。如 `.user-card--active`，`.user-card__picture--active`。

虽然 BEM 命名法逻辑清晰，且各模块名字之间不会相互影响，但名字比较冗长，实际开发中我们可以根据这个思想改的简洁一些，例如将 `.user-card__picture--active` 改写成 `.userCard-picture  .active`（`class="userCard-picture active"`）。

### 嵌套选择器
```
<div class="userCard active">
	<div class="userCard-name"></div>
</div>
```
```
.userCard{
	width: 100px;
	.userCard-name{
		color: block;
	}
}
// 编译后的 CSS
.userCard{
	width: 100px; }
	.userCard-name{
		color: block; }
```
### & 符号
```
.userCard{
	width: 100px;
	&.active{
		background: orange;
	}
	&-name{
		color: block;
	}
}
```
### 嵌套属性
```
.userCard{
	width: 100px;
	font: {
		size: 20px;
		weight: bold;
		family: 'Courier New',Courier,monospace;
	}
}
```

## 注释
CSS 只能 `/*单行注释*/`，SCSS 可以支持 `// 单行注释` 了
如果多行注释的第一个字母是 `!`，那么注释总是会被保留到输出的 CSS 中。

## 变量
变量可以在多个选择器里引用同一个数值，方便批量修改。
```
.userCard{
	width: 100px;
	&-name{
		$width: 120px;
		width: $width;
		height: $width;
	}
}
```
变量是有作用域的。
可以使用 `!global` 强行变为全局变量（不推荐使用）
如果定义一个变量名为 `$main-width`，也可以使用 `$main_width`访问它。

## 运算
1. SCSS 支持数字的加减乘除（除法特殊），取模运算（判断奇偶）
```
div{
    width: 100px + 100px + 100px; // 加
    width: 100px * 100; // 乘
    width: (100px/2); // 除
    width: $width/2; // 除
    width: 100px/2-0; // 除
    width: 101px % 2; // 取模运算，值为1，奇数
    width: 100px % 2; // 取模运算，值为0，偶数
}
```
将 `/` 解析为除法的三种情况：
+ 如果该值，或值的任何部分，存储在一个变量中或通过函数返回。
+ 如果该值是由括号括起来的，除非这些括号是在一个列表（list）外部，并且值是括号内部。
+ 如果该值被用作另一个算术表达式的一部分。
2. 颜色相关运算
```
div{
    $red: #FF0000;
    color: $red + #888888;
}
// 运算之后的值为：
div{
    color:  #FF8888;
}
```
+ 除此之外，SCSS 还提供了很多操作颜色的函数，可以对颜色进行任何操作，如：
```
div{
	$red: #FF0000;
	background: fade-out($red, 0.5); // 更改透明度
	color: change-color($color:$red, $green:255); // 更改颜色
}
```
3. 字符串插值
SCSS 支持在代码里插入字符串或变量（`#{}`），如在某个元素的开头和结尾各加一个符号，并插入变量：
```
div{
	$red: #FF0000;
	&::before{
		content: '* #{$red}';
	}
	&::after{
		content: '?';
	}
}
```
4. CSS 语法也支持运算：
```
div{
    width: calc(200px / 2);
}
```
