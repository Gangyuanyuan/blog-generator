---
title: 解决Hexo标签与MD文件冲突问题
date: 2019-08-12 13:42:37
tags: Git
categories: 前端探索
---

今天使用 Github+Hexo 搭建的博客写文章时遇到一个 Bug，查阅资料解决之后，发现很多人都遇到过由于 nunjucks 模板标签导致 MD 文件解析报错的问题，于是记录一下这个问题的解决方法。

## 报错及原因
我在使用 `hexo generate` 生成文章时，出现了如下报错：
```
$ hexo generate
INFO  Start processing
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 13, Column 197]
  unexpected token: }}
  at ......
```

出现上述情况的原因是 Markdown 文件中的 `{{ }}` 标签与 nunjucks 模板引擎的标签发生了冲突。`{{ }}`，`{% %}` 等这些标签都是模板引擎的占位标签，如果 MarkDown 文件中包含这些标签，那么解析时就会把 MD 文件中的标签动态解析了，于是导致 MD 文件解析时报错。

## 解决方法
### 方法一：修改 Markdown 文件
将含有双大括号的内容首尾添加如下标签进行处理：
```
{% raw %}
{{ 双大括号内包裹的内容 }}
{% endraw %}
```
用这个标签虽然可以解决问题，但之后再遇到类似的情况每次都需要对 MarkDown 文件进行修改。下面介绍一种更便捷的方法。

### 方法二：修改 nunjucks 模块源代码
1. 我们还可以直接在 nunjucks 模块上修改源代码，更改有冲突的渲染标签。
+ 首先打开这个文件，路径如下：
```
node_modules/nunjucks/src/lexer.js
```
+ 找到下述两行代码：
```
var VARIABLE_START = '{{';
var VARIABLE_END = '}}';
```
+ 将其修改为：
```
var VARIABLE_START = '{$';
var VARIABLE_END = '$}';
```
将有冲突的模板引擎的占位符更改为其他字符，进行模板解析时就不会与 MarkDown 的内容发生冲突了，且这种方法对所有 MarkDown 文件都是有效的，一劳永逸。
类似的，如果出现 `{% %}` 等符号的解析错误时，也可以根据情况将其更改为其他占位符（自定义）。

2. 插件同步修改
若博客使用 `hexo-generator-search` 或 `hexo-generator-feed` 等其他依赖于 `nunjucks` 模板的插件，那么这些插件的模板处理标签也需要进行同步修改。以搜索插件为例，打开如下路径的文件：
```
node_modules/hexo-generator-search/templates/search.xml
```
根据之前修改的 `nunjucks` 的内容，将此文件的 `{{ }}` 也分别更改为 `{$` 和 `$}` 即可。

**注意**：
如果在项目下执行 `npm install` 更新 `nunjucks` 模板时，那么之前更改的内容会被还原，需要重新对有冲突的符号进行更改。

