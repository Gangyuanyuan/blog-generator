---
title: 关于 CSS
date: 2019-04-26 08:40:58
tags: CSS
categories: 前端探索
---

## 概论
### CSS：层叠样式表（Cascading Style Sheets）
CSS2.1 目前是世界上支持内容最广泛的 CSS（最通用），IE 从 5.5 开始是支持 CSS2.1 的；我们现在学前段从 IE8 开始兼容，低于 IE8 就不看不管不测试；其他主流浏览器都是支持 CSS3 的。

 ### CSS引入的四种方式：
+ 内联样式：即 `style` 属性，把样式直接写在标签上。
+ `style` 标签：把 `style` 标签放在 `head` 里。
+ `link` 标签引入外部文件：`<link rel="stylesheet" href="./a.css">`.
+ 在 CSS 文件里再引入一个 CSS 文件：`@import url(./b.css);`.

## 知识点 
### div 高度
div 的高度由其 内部文档流元素 的高度总和决定。

**文档流**：即文档内元素的流动方向
内联元素（如 `span`）从左到右流动，如果流动遇到阻碍，就换行继续流动。
块状元素（如 `div`）从上往下流动，每个块都独占一行。
+ 如果一个 `span` 有 `border`，在流动过程中被断行截断了，`border` 也呈被分开状态，不会出现两个 `border`.
+ 英文单词默认不会换行，可用`word-break`控制换行。
`word-break:break all` 在任意位置打断
`word-break:break word` 只在单词连接处打断（默认状态）
+ 块状元素并排展示的方法：
  （1）用 `float:left`
  （2）用 `display:inline-block`

### span 高度
`span` 高度由字体，以及设计师设计的与字体样式相关的参数决定。

+ 文字是基线对齐，不是中线对齐。
+ 除了字体本身高度外，不同样式的字体还会有一个“建议行高”，浏览器默认取这个建议行高。
+ 可以用 `line-height` 指定内联元素的高度。

### 文档流（Normal Flow）
+ **文档流**：将窗体自上而下分成一行一行，并在每行中按从左至右的顺序依次排放元素，即为文档流，也称为普通流。
+ **脱离文档流**：元素脱离文档流之后，将不再在文档流中占据空间，而是处于浮动状态。
+ **脱离文档流的三种方法**：
    （1）`float: left;`
    （2）`position: fixed;`
    （3）`position: absolute;`

### CSS position 定位
+ **fixed 定位**
元素的位置相对于浏览器窗口是固定位置，即使窗口是滚动的它也不会移动。
```
.topNavBar{
  position: fixed;
  top: 0;
  left: 0;
}
```
（1）**脱离文档流**：使元素的位置与文档流无关，即不再在文档流中占据空间。
（2）`fixed` 定位的元素和其它元素重叠（会影响父元素高度）。
（3）用 `fixed` 定位之后，扩张的文字会缩回去，可用 `width:100%` 修正。（`width` 标签也容易出现 bug，能不用就不用）

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
绝对定位的元素的位置相对于最近的已定位父元素，若没有已定位的父元素，那么它的位置相对于 `<html>`.
```
h2{
  position: absolute;
  left: 100px;
  top: 150px; 
}
```
（1）**脱离文档流**：使元素的位置与文档流无关，因此不占据空间。
（2）`absolute` 定位的元素和其它元素重叠。
+ **static 定位**：静态定位
+ **sticky 定位**：粘性定位







