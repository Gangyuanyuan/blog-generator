---
title: HTML 常用标签
date: 2019-04-22 10:44:20
tags: HTML标签
categories: 前端探索
---

## 常用标签
### iframe 标签
嵌套页面。
需要新开一个窗口，速度比较慢。
+ iframe 直接使用
```
<iframe src="https://www.baidu.com" frameborder="0"></iframe>
```
直接打开新页面，内容为百度。
+ iframe 与 a 标签配合使用（name属性）
```
<iframe src="#" frameborder="0" name="xxx"></iframe>
<a target="xxx" href="http://www.qq.com">腾讯</a>
<a target="xxx" href="htttp://www.baidu.com">百度</a>
```
此 a 标签会在 name 为"xxx"的窗口打开。
+ 不写`frameborder="0"`的话，iframe 会出现一个默认的 border 为 1，所以是为了消除 border。

### a 标签
跳转页面（HTTP GET 请求）
+ **target 属性**
```
<a href="http://qq.com" target="_blank">blank-QQ</a>
<a href="http://qq.com" target="_self">self-QQ</a>
<a href="http://qq.com" target="_parent">parent-QQ</a>
<a href="http://qq.com" target="_top">top-QQ</a>
```
>_blank----在空页面打开
>_self----在当前窗口打开
>_parent----在上级窗口打开（父页面）
>_top----在最顶级窗口打开（祖宗页面）

+ **download 属性**
```
<a href="http://qq.com" download>下载</a>
```
关于下载：
（1） 由 http 响应决定，若响应的的 content-type 写为`content-type: application/octet-stream`，浏览器会以下载的形式接受这个请求，而不是在页面上展示。
（2） 若写为`content-type: text/html`，只能在 a 标签上写个 download，强制下载。

+ **href 属性**
href 属性的几种写法：
```
1. <a href="http://qq.com" >QQ</a>
   <a href="https://qq.com" >QQ</a> 
2. <a href="//qq.com" >QQ</a>              //无协议的绝对地址
3. <a href="./xxx.html" >xxx</a>             //相对路径
   <a href="?name=qqq" >xxx</a>      //?name=qqq直接加在当前页面后面
   <a href="#ssss" >xxx</a>             //同上，但不发起请求
4. <a href="javascript:alert(1);">xxx</a>    //伪协议（看似与http同等的协议）
   <a href="javascript:;">xxx</a>          //点击后什么都不做的a链接
```
（1）写代码时一般指定为 http 协议，不用 file 协议。
（2）`<a href="//qq.com" >QQ</a>` 表示“无协议绝对路径”，即当前文件是什么协议，它就是什么协议（一般为 file 协议）
（3）`<a href="qq.com" >QQ</a>` 打不开，因为qq.com是一个相对地址，相当于一个文件
（4）`<a href="#ssss" >xxx</a>` 直接跳转到 xxx.html/#ssss，但因为是锚点，所以不发起请求（只有锚点不发起请求，锚点作用是实现页面内跳转）
（5）`<a href="javascript:;" >xxx</a>` 伪协议的应用：点击之后不需要任何动作的 a 标签
（6）`<a href="">link</a>` 当前页面刷新
（7）`<a href="#">link</a>` 页面锚点变成“#”或页面滚动到顶部（“#” 包含了一个位置信息，默认的锚点是 #top）
+ a 标签属性见 MDN：[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a "null")

### form 标签
跳转页面（HTTP POST 请求）
+ form 标签的作用是用于将其他表单标签“包”起来，以便作为一个整体，可以提交数据到服务器。
```
<form action="index2.html" method="post" target="_blank">
    <input type="text" name="username">
    <input type="text" name="password">
    <input type="submit" value="提交">
</form>
```
+ form 发起的是 post 请求。
+ `name`：给该表单命名，用于JS技术使用；
（1）name 最终会被带到 post 请求的第四部分，成为它的 key；
（2）如果 form 标签里的 input 不加 name 属性，那么在表单提交时，input 的值就不会出现在请求里。
+ `action="URL"` ：指定 form 表单向何处发送数据
+ `method="get / post"`：以何种方式向服务器发送数据
method 取 get 会把参数默认放到查询参数里面，则不会出现第四部分；取 post 会把参数默认放在第四部分，不会出现查询参数。我们可以通过给 action 加参数，让 post 也有查询参数，但没有任何方法让 get 请求拥有第四部分。
+ `enctype="string"`：规定表单数据以什么形式进行编码。
+ form 标签也有 `target`，且规则和 a 标签一样。
+ form 属性见 MDN：[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form "null")

### input / button 标签
区别：是否为“ 空标签”；
input 没有子元素，button 有 span 等子元素。

+ 写 button 按钮时若不写 type，它会自动升级为提交按钮；
若写了 type，按钮则按 type 指定类型显示。
```
<form action="index2.html" method="post" target="_blank">
    <button>button</button>                 // 自动升级为提交按钮
    <button type="button">button</button>          // 普通button按钮
</form>
```
+ 写 input 时，type 指定为什么类型，就是什么类型。
input 标签有很多种 type。
```
<form action="index2.html" method="post" target="_blank">
    <input type="button" value="button">       // 普通button按钮
    <input type="submit" value="submit">       // 提交按钮
</form>
```
（1）submit 是唯一能确定 form 表单能不能点击提交的按钮。
（2）有提交键时按回车即可跳转。

+ **checkbox：（多选框）**
可以同时勾选多个框；name 相同，表明这是同一个事物的选项。

（1）label 的 for 和 input 的 id 是对应的，要成对出现
```
<form action="index2.html" method="post" target="_blank">
    <input type="checkbox">爱我              //点“爱我”无反应
    <input type="checkbox" id="xxx"><label for="xxx">爱我</label>  //点“爱我”也可勾选
</form>
```
（2）也可用 label 标签包裹住 input（简单，比较常用）
```
<form action="index2.html" method="post" target="_blank">
    喜欢的水果
    <label><input type="checkbox" name="fruit" value="orange">橘子</label>
    <label><input type="checkbox" name="fruit" value="banana">香蕉</label>
    //点“橘子”和“香蕉”也可勾选选框
</form>
```

+ **radio：（单选框）**
name 相同时，只能勾选其中一个框。
```
<form action="index2.html" method="post" target="_blank">
   爱我
    <label><input type="radio" name="loveme" value="yes"></label>
    <label><input type="radio" name="loveme" value="no"></label>
</form>
```
+ **password：密码输入框**
虽然输入时看不到密码，但实际还是明文传输的。

+ input 属性见：[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input "null")
+ button 属性见：[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button "null")

### select 标签
下拉选择框。
```
<form>
	<select name="group">
		<option value="">-</option>        //值为空
		<option value="1">第一组</option>  
		<option value="2">第二组</option>
		<option value="3" disabled>第三组</option>    //不能选
		<option value="4" selected>第四组</option>    //默认选项
	</select>
</form>
```
+ select 加 multiple 属性，可按着 shift 或 ctrl 实现多选。
```
<select name="group" multibple>
```

### textarea 标签
多行文本输入框。
```
<textarea style="resize:none; width:100px; height:50px;" name="爱好"></textarea>
```
+ 文本框可以随意拉动大小，防止出现 bug，常用 css 固定大小（宽、高也可以用行、列替代）。

### table 标签
以表格的形式展示数据。
```
<table border=1>    //表格加边框
	<colgroup>           //colgroup里面有col属性才有意义
		<col width=100>    //第一列宽度（px)
		<col width=200>    //bgcolor已不常用，现一般用css控制
		<col width=100>
		<col width=100>
	</colgroup>
	<thead>
		<tr>
			<th>项目</th><th>姓名</th><th>班级</th><th>分数</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th>1</th><td>小明</td><td>一班</td><td>94</td>
		</tr>
		<tr>
			<th>2</th><td>小红</td><td>二班</td><td>96</td>
		</tr>
	</tbody>
	<tfoot>
		<tr>
			<th>平均分</th><td></td><td></td><td>95</td>
		</tr>
		<tr>
			<th>总分</th><td></td><td></td><td>190</td>
		</tr>
	</tfoot>
</table>
```
+ `thead`：table head 、`tbody`：table body 、`tfoot`：table foot
`tr`：table row（行）、`td`：table data（数据）、`th`：table header（标题）
+ thead、tbody、tfoot 的内容与三个标签排放顺序无关，不影响内容显示。
+ 不写 tbody，系统会自动补上；
不写 thead 和 tfoot，就没有表头和表尾，内容统统放在 tbody 里。
+ table 的 border 默认是有空间的，可以在css里把它合并起来：
```
    <style>
		table{
			border-collapse: collapse;
		}
	</style>
```
+ table 属性见：[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table "null")

### 表单元素总结
单行文本框：`<input type="text">`，默认值是type="text"
密码框：`<input type="password">`
单选按钮：`<input type="radio">`
多选按钮：`<input type="checkbox">`
隐藏框：`<input type="hidden">`
文件上传：`<input type="file">`
下拉框：`<select></select>`
多行文本：`<textarea></textarea>`
标签：`<label></label>`
元素集：`<fieldset></fieldset>`
提交按钮：`<input type="submit">`
普通按钮：`<input type="button">`
重置按钮：`<input type="reset">`

### noscript 标签
如果用户浏览器不支持 script，则会显示 `noscript` 中的内容。

## 空标签
**空标签**：即空元素，是指有内容的元素，即没有子元素（包括文本）的元素。
+ 空标签是不闭合的标签，不成对出现
+ 在 HTML 中，通常在一个空元素上使用一个闭标签是无效的。例如，`<input type="text"></input>` 的闭标签是无效的。
+ 常见空标签：
```
<area>
<base>
<br>
<col>
<colgroup> when the span is present.
<command>
<embed>
<hr>
<img>
<input>
<keygen>
<link>
<meta>
<param>
<source>
<track>
<wbr>
```
## 替换元素
### 可替换元素
可替换元素：替换元素是浏览器根据其标签的元素与属性来判断显示具体的内容。替换元素一般没有实际内容。
+ 比如`<input/>`，type="text" 时是一个文本输入框；取作其他的时候，浏览器显示就不一样了。
+ 可替换元素的展现不是由 CSS 来控制的。这些元素是一类外观渲染独立于 CSS 的外部对象。典型的可替换元素有`<img>`、`<object>`、`<video>`和表单元素，如`<textarea>`、`<input>`。某些元素只在一些特殊情况下表现为可替换元素，例如`<audio>`、`<canvas>`。通过 CSS `<content>`属性来插入的对象被称作匿名可替换元素。

### 非替换元素
非替换元素：HTML的大多数元素是不可替换元素，他们将内容直接告诉浏览器，将其显示出来。
+ 比如`<p>我爱学习<p/>`，浏览器将这段话直接显示出来。