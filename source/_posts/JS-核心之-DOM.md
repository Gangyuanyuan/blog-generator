---
title: JS 核心之 DOM
date: 2019-07-16 13:02:44
tags: JS
categories: 前端探索
---

## 理解 DOM
### DOM 定义
DOM 的全称是" Document Object Model "，中文意思为“文档对象模型”。
>+ Document： 表示 XML 或 HTML 文档（HTML 是 XML 的衍生品）
>+ Object：把文档变成对象
>+ Model：把 DOM 抽象理解成一棵 **树模型**
### 节点
+ 浏览器会根据 DOM 模型，将结构化文档（如 HTML 和 XML）解析成一系列的节点，再由这些节点组成一个树状结构（DOM Tree）。DOM 的**最小组成单位**叫作**节点**（node），文档的树形结构由不同类型的节点组成。
+ 节点有**七种类型**：
>**Document**（html）、**Element**（元素）、**Text**（文本）
Comment（注释）、Attribute（属性）
DocumentType（doctype 标签）、DocumentFragment（文档片段）

就像 JS 中所有的对象都继承自 `Object` 一样，浏览器提供一个原生的节点对象 `Node`（Node 是一个函数），**DOM 的所有节点都继承自 Node**，Node 又继承自 Object，因此它们具有一些共同的属性和方法。
### 节点树
一个文档的所有节点，按照所在的层级，可以抽象成一种树状结构。这种树状结构就是 DOM 树。
DOM 树形结构：![DOM 树形结构](https://upload-images.jianshu.io/upload_images/13038962-8a1cf5f586166d30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

顶层的**根节点 html** 属于 `Document` 节点，代表整个文档；
第二层级和第三层级的属于 `Element` 节点，即网页的各种 HTML 标签；
第四层级的属于 `Text` 节点，即标签之间或标签包含的文本。

### 深刻理解 DOM
将构造这些节点的构造函数也用树形结构表示：![DOM 构造关系](https://upload-images.jianshu.io/upload_images/13038962-7790022eb02e4596.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

页面中的节点，通过上述不同的构造函数，构造出相应的对象。
由此可以看出 **DOM 的主要作用**：DOM 是 JavaScript 操作网页的接口，它的作用是将网页转为一个 JavaScript 对象，从而可以用脚本进行各种操作（如增删改查）。
DOM 有自己的国际标准，目前通用版本是 DOM 3。

## Node 接口
### 属性
Node 属性有很多，用到时可以查询文档。
+ `childNodes`：返回一个类似数组的对象（NodeList 集合），成员包括当前节点的所有子节点。（**注意**：回车也会被认为是 Text 节点）
+ `firstChild`：返回当前节点的第一个子节点，没有则返回 null。
+ `lastChild`：返回当前节点的最后一个子节点，没有则返回 null。
+ `nextSibling`：返回紧跟在当前节点后面的第一个同级节点。没有则返回 null。（**注意**：可能会获取到“空格”或“回车”这样的文本节点）
+ `previousSibling`：返回当前节点前面的、距离最近的一个同级节点。没有则返回 null。
+ `nodeName`：返回节点的名称。（**注意**：获取到的节点名称全都是**大写**，'svg' 除外）
+ `nodeType`：返回一个整数值，表示节点的类型。

| 节点类型 | 值 | 对应常量 |
| --- | --- | --- |
| 元素节点（element） | 1 | Node.ELEMENT_NODE |
| 文本节点（text） | 3 | Node.TEXT_NODE |
| 注释节点（Comment） | 8 | Node.COMMENT_NODE |
| 文档节点（document） | 9 | Node.DOCUMENT_NODE |
| 文档类型节点（DocumentType） | 10 | Node.DOCUMENT_TYPE_NODE |
| 文档片断节点（DocumentFragment） | 11 | Node.DOCUMENT_FRAGMENT_NODE |
例：![节点类型](https://upload-images.jianshu.io/upload_images/13038962-a39f20c412695e18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ `nodeValue`：返回一个字符串，表示当前节点本身的文本值。
+ `ownerDocument`：返回当前节点所在的顶层文档对象，即 document 对象。
+ `parentNode`：返回当前节点的父节点。
+ `parentElement`：返回当前节点的父元素节点（Element 节点）。
+ `innerText`：表示一个节点及其后代的“渲染”文本内容
+ `textContent`：表示一个节点及其后代的文本内容。
>**注意**：`textContent` 与 `innerText` 的区别：
>+ `textContent`会获取所有元素的内容，包括`<script>`和`<style>`元素，而 `innerText` 不会；
>+ `innerText`受 CSS 样式的影响，且不会返回隐藏元素的文本，而`textContent`会；
>+ `innerText`受 CSS 样式的影响会触发重排（reflow），但`textContent`不会；
>+ 在 Internet Explorer (<= IE11 的版本) 中对`innerText`进行修改， 不仅会移除当前元素的子节点，而且还会永久性地破坏所有后代文本节点（所以不可能再次将节点再次插入到任何其他元素或同一元素中）。
+ `baseURL`：返回一个字符串，表示当前网页的绝对路径。浏览器根据这个属性，计算网页上的相对路径的 URL。

### 方法
如果一个属性是函数，那么这个属性也叫做**方法**；即方法是函数属性。
+ `appendChild()`：接受一个节点对象作为参数，将其作为最后一个子节点，插入当前节点。
+ `cloneNode(deep)`：克隆一个节点。它接受一个布尔值作为参数，表示是否同时克隆子节点。参数默认为 false.
```
cloneNode(true) // 深拷贝，克隆此节点及它的子节点（递归克隆）。
cloneNode(false) // 浅拷贝，只克隆此节点本身。
```
+ `contains()`：返回一个布尔值，表示参数节点是否为当前节点，或为当前节点的子节点或后代节点。
+ `hasChildNodes()`：返回一个布尔值，表示当前节点是否有子节点。
+ `insertBefore()`：将某个节点插入父节点内部的指定节点的前面。
```
insertBefore(newNode, referenceNode) // （新节点，父节点内部的一个子节点）
```
+ `isEqualNode()`：返回一个布尔值，用于检查两个节点是否相等。
+ `isSameNode()`：返回一个布尔值，表示两个节点是否为同一个节点（即 `node1 === node2`）。
```
<body>
  <div class="red">div</div>
  <div class="red">div</div>
</body>

var div1 = document.body.children[0]
var div2 = document.body.children[1]
div1.isEqualNode(div2) // true
div1.isSameNode(div2) // false
```
+ `removeChild()`：接受一个子节点作为参数，用于从当前节点移除该子节点。（移除后该子节点依然在内存中，但不再出现在页面中）
+ `replaceChild()`：将一个新的节点，替换当前节点的某一个子节点。
+ `normalize()`：将当前节点和它的后代节点“规范化”。它会去除空的文本节点，并将相邻的文本节点合并成一个。
```
var wrapper = document.createElement("div");
wrapper.appendChild(document.createTextNode("part 1 "));
wrapper.appendChild(document.createTextNode("part 2 "));
wrapper.childNodes.length // 2
wrapper.childNodes[0].textContent // "part 1 "
wrapper.childNodes[1].textContent // "part 2 "

wrapper.normalize()
wrapper.childNodes.length // 1
wrapper.childNodes[0].textContent // "part 1 part 2 "
```
## Document 接口
document对象是文档的根节点，window.document属性就指向这个对象。
### 属性
+ `head`、`body`、`title`、`children`、`doctype`
+ `anchors`： 返回当前文档中的所有锚点元素（只返回拥有 name 属性的 a 元素，而不是拥有 id 属性的 a 元素，已弃用）。
+ `characterSet`：返回当前文档的字符编码。
+ `childElementCount`：返回一个无符号长整型数字，表示给定元素的子元素个数。
+ `documentElement`：返回文档对象（document）的根元素的只读属性（如 HTML 文档的 <html> 元素）。
+ `domain`：获取域名。
+ `hidden`：返回一个布尔值，表示该文档是否被隐藏。
+ `images`：获取页面中所有的 `img` 标签。
+ `links`：获取页面中所有的 `a` 标签。
+ `location`：返回一个包含有关文档当前位置信息的 `Location` 对象。
+ `onclick`、`onerror`、`onchange`等：一系列监听事件。
+ `plugins`：用户是否开启了插件。
+ `readyState`：返回文档的加载状态：
>`loading`：document 仍在加载；
`interactive`：文档已经完成加载并被解析，但是诸如图像，样式表和框架之类的子资源仍在加载；
`complate`：文档和所有子资源已完成加载。`load` 事件即将被触发。
+ `referrer`：（引荐者）返回“链接”到此页的URI。
+ `scripts`：获取当前文档中所有的 `<script>` 标签。
+ `scrollingElement`：返回对滚动文档的元素的引用。
+ `styleSheets`：获取当前文档中多有的 CSS 样式表。
+ `fullscreen`：返回一个布尔值，表明窗口是否处于全屏模式下。
+ `visibilityState`：返回 document 的可见性，即查看页面是否被显示。它有四种状态：
>`visible`：此时页面内容至少是部分可见（即文档在前景标签页中且窗口没有最小化）。
`hidden`： 此时页面对用户不可见（即文档处于背景标签页或者窗口处于最小化状态，或者操作系统处于锁屏状态）。
`prerender`：此时页面正在渲染中, 因此是不可见的（文档只能从此状态开始，永远不能从其他值变为此状态）。
`unloaded`：页面从内存中卸载清除。

### 方法
+ `close()`：关闭向当前文档的数据写入。
（**注意**：页面加载完毕会自动调用 `window.close`，即不能再写入内容了）
+ `open()`：打开一个要写入的文档。
+ `writer()`：将一个文本字符串写入由 `document.open()` 打开的文档流。
（**注意**： 因为 `document.write` 需要向文档流中写入内容，因此在关闭已加载的文档上调用 `document.write` 会自动调用 `document.open`，**这将清空该文档之前写入的内容**）
+ `writeln()`：向文档中写入一串文本，并紧跟着一个换行符。
+ `createDocumentFragment()`：创建一个新的空白的文档片段( `DocumentFragment`）。
+ `createElement()`：创建由 tagName 指定的 HTML 元素。
+ `createTextNode()`：创建一个新的文本节点。
+ `execCommand()`：当一个HTML文档切换到设计模式时，允许运行命令来操纵可编辑内容区域的元素。（**注意**：可用 'copy' 拷贝当前内容到剪贴板）
+ `exitFullscreen()`：让当前文档退出全屏模式。（**注意**：调用这个方法会让文档回退到调用`Element.requestFullscreen()`方法进入全屏模式之前的状态，若之前其它元素的状态也处在全屏模式，则之前的状态不会改变）
+ `getElementById()`：返回匹配指定 id 的一个元素。
+ `getElementsByClassName()`：返回一个HTML集合`HTMLCollection`(伪数组)，包含匹配指定类名的所有元素。
+ `getElementsByName()`：返回一个`HTMLCollection`，包含匹配指定`name`的所有元素。
+ `getElementsByTagName()`：返回一个`HTMLCollection`，包含匹配指定标签名的所有元素。
+ `getSelection()`：返回一个 `Selection`对象，表示文档中当前被选择的文本。
+ `hasFocus()`：表明当前文档或当前文档内的节点是否获得了焦点。
+ `querySelector()`：返回与指定的选择器组相匹配的**第一个元素**。
+ `querySelectorAll()`：返回一个包含与指定的选择器组匹配的**所有元素**的`NodeList`（伪数组）。

**通过 DOM API 获取到的 elements 都是伪数组。**

## Element 接口
`Element` 对象对应网页的 HTML 元素。每一个 HTML 元素在 DOM 树上都会转化成一个 `Element`节点对象。
### 属性
+ `attributes`：返回一个与该元素相关的所有属性的集合。
+ `classList`：返回该元素包含的 class 属性的集合。
+ `className`：获取或设置指定元素的 class 属性的值。
+ `clientHeight`：获取元素内部的高度，包含内边距，但不包括水平滚动条、边框和外边距。（`clientHeight = CSS height + CSS padding - 水平滚动条高度`）
+ `clientTop`：返回该元素距离它上边界的高度。
+ `clientLeft`：返回该元素距离它左边界的宽度。
+ `clientWidth`：返回该元素它内部的宽度，包括内边距，但不包括垂直滚动条、边框和外边距。（该属性值会被四舍五入为一个整数，若需要一个小数可使用`element.getBoundingClientRect()`）
+ `innerHTML`：设置或获取 HTML 语法表示的元素的后代（最好不要用`innerHTML`，它会把用户写的标签当作开发者写的标签去渲染）。
+ `tagName`：返回当前元素的标签名。

### 方法
+ `getAttribute()`：获取元素属性的当前值。
+ `animate()`：创建一个新的动画。
+ `querySelector()`：返回与指定的选择器组匹配的元素的后代的第一个元素。
+ `querySelectorAll()`：返回一个包含与指定的选择器组匹配的**所有元素**的 `NodeList`（伪数组）。
+ `remove()`：把对象从它所属的 DOM 树中删除。
+ `removeAttributes()`：从指定的元素中删除一个属性。
+ `setAttribute()`：设置指定元素上的一个属性值。若属性已存在，则更新该值；若不存在，则添加这个新的属性。

## 节点集合
DOM 提供两种节点集合，用于容纳多个节点：`NodeList` 和`HTMLCollection`。这两种集合都属于接口规范。许多 DOM 属性和方法，返回的结果是 `NodeList` 实例或 `HTMLCollection` 实例。

### NodeList 接口
1. NodeList 实例是一个类似数组的对象，它的成员是节点对象。
NodeList 是**伪数组**，可以使用 `length` 属性、 `forEach()` 方法和 `for` 循环。
```
document.body.childNodes instanceof NodeList // true
```
2. NodeList 实例可能是动态集合，也可能是静态集合。所谓动态集合就是一个活的集合，DOM 删除或新增一个相关节点，都会立刻反映在 NodeList 实例。目前，只有 Node.childNodes 返回的是一个动态集合，其他的 NodeList 都是静态集合。
3. NodeList 只能使用数字索引引用。

### HTMLCollection 接口
1. HTMLCollection 是一个节点对象的集合，只能包含元素节点（element），不能包含其他类型的节点。HTMLCollection 也是**伪数组**，它没有 `forEach` 方法，只能使用 `for` 循环遍历。
2. HTMLCollection 实例都是**动态集合**，节点的变化会实时反映在集合中。
3. HTMLCollection 实例可以用 `id` 属性或 `name` 属性引用该节点元素。

## 其他接口
节点对象除了继承 Node 接口以外，还会继承其他接口。
### ParentNode 接口
1. ParentNode 接口表示当前节点是一个父节点。如果当前节点是父节点，就会继承 ParentNode 接口。
2. 由于只有元素节点（`element`）、文档节点（`document`）和文档片段节点（`documentFragment`）拥有子节点，因此只有这三类节点会继承 ParentNode 接口。
3. 相关属性和方法：
+ `ParentNode.children`：返回一个 HTMLCollection 实例，成员是当前节点的所有元素子节点。（**注意**：children属性只包括元素子节点，不包括其他类型的子节点）
+ `ParentNode.firstElementChild`：返回当前节点的第一个元素子节点。
+ `ParentNode.lastElementChild`：返回当前节点的最后一个元素子节点。
+ `ParentNode.childElementCoun`：返回一个整数，表示当前节点的所有元素子节点的数目。
+ `ParentNode.append()`：为当前节点追加一个或多个子节点，位置是最后一个元素子节点的后面。
+ `ParentNode.prepend()`：为当前节点追加一个或多个子节点，位置是第一个元素子节点的前面。

### ChildNode 接口
1. ChildNode 接口表示当前节点是一个子节点。如果一个节点有父节点，那么该节点就是一个子节点，就会继承 ChildNode 接口。
2. Element 节点、DocumentType 节点和 CharacterData 接口，部署了ChildNode 接口，凡是这三类节点（接口），都可以使用以下相关方法。
3. 相关方法：
+ `ChildNode.remove()`：从父节点移除当前节点。
+ `ChildNode.before()`：在当前节点的前面，插入一个或多个同级节点（可以插入元素节点或文本节点）。两者拥有相同的父节点。
+ `ChildNode.after()`：在当前节点的后面，插入一个或多个同级节点（可以插入元素节点或文本节点）。
+ `ChildNode.replaceWith()`：使用参数节点，替换当前节点（参数可以是元素节点或文本节点）。

## 其他知识点
1. + `<div id="x"></div>`，x 的值为：这个 div 对应的 Element 对象。
    + `<div id="parent"></div>`，parent 的值为：如果有父窗口，就是父窗口；如果没有，就是当前窗口。
    + `<div id="parent1"><div id="child"></div></div>`，parent1.childNodes 的值为：{0::child, length:1} 伪数组。

2. + `parent.ChildNodes` 属性返回的 NodeList 是动态集合。DOM 删除或新增一个相关节点，都会立刻反映在 NodeList 接口之中。
    + `document.querySelectorAll()` 方法返回的 NodeList 是静态集合。DOM 内部的变化，不会实时地反映在该方法的返回结果之中。
