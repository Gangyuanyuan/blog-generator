---
title: 自己写一个 jQuery
date: 2019-07-16 13:40:52
tags: jQuery
categories: 前端探索
---

jQuery 是目前使用最广泛的 JavaScript 函数库，它提供的 API 易于使用且功能强大。我们也可以模仿 jQuery 写一个函数库，封装一些实现特定功能的函数。
## 封装两个函数
我们先封装两个常用的函数：
1. 给某个节点添加类名
```
function addClass(node, class1) { 
    node.classList.add(class1)
}
```
假设给节点 nodeX 添加类名 red：`addClass(nodeX, 'red')`
2. 给某个节点设置文本内容
```
function setText(node, text) { 
    node.textContent = text
}
```
假设给节点 nodeX 设置文本内容为 hi：`setText(nodeX, 'hi')`

## 命名空间
“命名空间”是一种设计模式，即把自己写的函数放在一个库里并给它命名，它们就不会与已有的相同标识符发生冲突。例：给上述两个函数命名为 `gydom`：
```
window.gydom = {……}
gydom.addClass = addClass
gydom.setText = setText
// 函数调用
gydom.addClass(nodeX, 'red' )
gydom.setText(nodeX, 'hi')
```

## 改写为 Node.prototype
扩展 Node 接口，直接在 `Node.prototype` 上添加函数，用 `this` 传参，方便调用。
```
Node.prototype.addClass = function(class1) { 
    this.classList.add(class1)
}
Node.prototype.setText = function(text) { 
    this.textContent = text
}
// 调用函数
nodeX.addClass.call(nodeX, 'red') // 等价于 nodeX.addClass('red')
nodeX.setText.call(nodeX, 'hi')  // 等价于 nodeX.getSiblings('hi') 
```
>`nodeX.function.call(nodeX) ` 为显示执行 this
`nodeX.function() ` 为隐式执行 this

`function.call()` 的第一个参数，会被当做 `this` 来执行。新手最好用 `call()` 调用函数，可以加深对 `this` 的理解。

## 改写为 Node2
虽然在 `Node.prototype` 上定义函数方便使用，但是有很大的弊端，不同的人定义的函数会相互覆盖，比较杂乱，所以我们可以重写一个新的接口 Node2，再用它新提供的 API 去操控节点。
```
window.Node2 = function(node){
  return {
    addClass: function(node, class1) { 
      node.classList.add(class1)
    },
    setText: function(node, text) { 
      node.textContent = text
    }
  }
}
// 调用函数
var node2 = Node2(nodeX)
node2.addClass('red')
node2.setText('hi')
```
这种方法叫做“无侵入”。

## 改名为 jQuery
+ 将函数名 Node2 改为 jQuery，函数体不做改动，还是实现同样的效果。
```
window.jQuery= function(node){
  return {
    addClass: function(){……},
    setText: function(){……}
  }
}
// 调用函数
var node2 = jQuery(nodeX)
node2.addClass('red')
node2.setText('hi')
```
jQuery 其实相当于一个构造函数，它接受参数，然后返回一个新的对象，并调用新对象的 API 去操作接受的元素。
+ jQuery 接受的参数不仅可以是节点，还可以是字符串：
```
window.jQuery = function(nodeOrSelector){
  let node
  if(typeof nodeOrSelector === 'string'){ // 判断参数类型
    node = document.querySelector(nodeOrSelector) // 根据参数找对应节点
  }else{
    node = nodeOrSelector // 直接将字符串地址赋值给 node
  }
  return {
    addClass: function(){……}
    setText: function(){……},
  }
}
// 调用
var node2 = jQuery ('#xxx') // 获取页面中 id 为 xxx 的元素
node2.addClass({'red')
node2.setText('hi')
```

## jQuery 操作多个节点
```
// 接受一个 Node 或选择器
window.jQuery = function(nodeOrSelector) {
    // 封装成一个伪数组
    let nodes = {}
    if (typeof nodeOrSelector === 'string') {
      let temp = document.querySelectorAll(nodeOrSelector) // 伪数组
      for (let i = 0; i < temp.length; i++) {
        nodes[i] = temp[i]
      }
      nodes.length = temp.length
    } else if (nodeSelector instanceof Node) {
      nodes = {
        0: nodeOrSelector,
        length: 1
      }
    }
    // API
    nodes.addClass = function(class1) { 
      for (let i = 0; i < nodes.length; i++) {
        nodes[i].classList.add(class1)
      }
    }
    // API 
    nodes.setText = function(text) { 
      for (let i=0; i<nodes.length; i++) {
        nodes[i].textContent = text
      }
    }
    // 返回伪数组 nodes
    return nodes
}
// 调用
var nodes = jQuery('li')  // 页面中的 li 标签
nodes.addClass('red')
nodes.setText('hi')
```

## alias $
我们可以给 jQuery 一个别名 `$`，使调用时更简洁方便：
```
window.$ = jQuery
```
如果一个对象是由 jQuery 构造出来的，最好在对象名字前面加上 `$`，表示它是 jQuery 的对象，避免与 DOM 对象混淆而用错方法。
```
var nodes = jQuery('li')
// 改写为
var $nodes = $('li')
$nodes.addClass('red')
$nodes.setText('hi')
```
这样，封装完的 jQuery 函数就是具有一定功能的 **API**，可以供自己和他人调用了。
