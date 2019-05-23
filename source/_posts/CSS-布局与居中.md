---
title: CSS 布局与居中
date: 2019-04-27 09:04:59
tags: CSS
categories: 前端探索
---
## 常见布局
### 浮动布局
可以通过盒模型的 `float` 属性实现浮动布局，使元素脱离文档流，浮动起来。
（1）使所有子元素浮动起来：`float: left;` 或 `float: right;`
（2）给父元素添加 `clearfix` 类清除浮动（加在类名后面，与类名用空格隔开），例：
```
<div class="picture clearfix">
  ······
<div>
```
（3）在 css 文件中添加以下代码：
```
.clearfix::after{           
  content: '';             
  display: block;
  clear: both;
}
```
### 定位布局
通过 `position` 属性来进行定位，确定盒模型的位置。
+ **relative 定位**：相对定位
相对元素的定位是相对其正常位置。
```
h2.pos-left{
  position: relative;
  left: -20px;
}
```
相对定位元素经常被用来作为绝对定位元素的容器块。
+ **absolute 定位**：绝对定位
绝对定位的元素的位置相对于最近的已定位父元素，若没有已定位的父元素，那么它的位置相对于<html>.
```
h2{
  position: absolute;
  left: 100px;
  top: 150px; 
}
```
（1）**脱离文档流**：使元素的位置与文档流无关，因此不占据空间。
（2）`absolute` 定位的元素和其它元素重叠。

## 两种布局的应用
### 左右布局
+ `float`+`margin`
一个父 div 包裹两个子 div，并用 `float` 属性使两个子 div 浮动起来；子 div 宽度可以设置为固定值如 `width: 100px;`，也可以写为父 div 宽度的百分比形式如 `width: 50%;`。可以给两个子 div 设置一定的 `margin` 值，确定其间距。

+ `position` 绝对定位
父 div 用 `position: relative;` 相对定位，两个子 div 用 `position: absolute;` 使其对父元素绝对定位，并用 `left` 或 `right` 调整其左右位置。

### 左中右布局
+ `float`+`margin`
一个父 div 包裹三个子 div，并用 `float` 属性使三个子 div 都浮动起来；可以设置其中两个子 div 的宽度，使第三个宽度自适应；也可以根据需要分别设置三个子 div 的宽度。再用 `margin` 调整位置。

+ `position` 绝对定位
与左右布局类似，父 div 用 `position: relative;` 相对定位，三个子 div 用 `position: absolute;` 绝对定位，并根据需要用 `left` 或 `right` 调整其左右位置。

## 设置居中
### 垂直居中
 + 内联元素垂直居中：使 `height` 和 `line-height` 高度相同：
```
.a{
    height: 50px;
    line-height: 50px;
}
```

+ 块状元素垂直居中：用 `position` 定位实现：
```
.parent{
    height: 200px;
    position: relative;
}
.a{
    height: 100px;
    margin-top: -50px;    
    position: absolute;
    top: 50%;
}
```

### 水平居中
+ 内联元素水平居中：通过父元素实现：
```
.parent{
    text-align: center;
}
```

+ 块状元素水平居中：宽度确定，用 `margin` 属性：
```
a.{
    margin: 0 auto;
}
```

+ 宽度未知或多个块状元素水平居中：
```
.parent{
    text-align: center;
}
.a{
    display: inline-block;
}
```

## 其他
1. 若外面父 div 高度是固定的，里面的子 div 高度可写为 `height:100%;`
2. CSS 中相同的代码段，可以通过 "`inherit`" 继承关系，使其继承父元素的定义：
```
div{
	color: red; 
}
div a{
    color: inherit; 
}
```

3. `content-box` 与 `border-box` 的区别：
+ `content-box` 的 `width` 不包括 `padding` 和 `border`.
+ `border-box` 的 `width` 包括 `padding` 和 `border`.

4. 给 div 加 `padding` 会使 div 变大，所以应该把 div 里面的内容另外打包成一个新的 div，然后给它加 `padding`.

5. 若 `li` 标签不能包裹住 `a` 标签，给 `a` 标签一个 `display:block` 就可以消除 bug。

6. 块状元素显示为内联元素时，最好用 `vertical-align` 防止其产生 bug：
```
.a{
    display: inline-block;  
    vertical-align: top; 
}  
```

