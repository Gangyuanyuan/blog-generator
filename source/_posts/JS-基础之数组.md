---
title: JS 基础之数组
date: 2019-07-12 22:52:03
tags: JS
categories: 前端探索
---

## 数组
### 定义
+ 数组（array）是按次序排列的一组值。每个值的位置都有编号（从 0 开始），整个数组用方括号表示。
```
var arr = ['a', 'b', 'c'];
```
+ 任何类型的数据，都可以放入数组。
+ 如果数组的元素还是数组，就形成了多维数组。

### 数组的本质
本质：数组属于一种特殊的对象。`typeof` 运算符返回数组的类型是 `object`.
特殊性：数组的键名是按次序排列的一组整数（0，1，2…）
+ 数组的**键名**其实也是字符串。可以用数值读取，是因为非字符串的键名会被转为字符串。这点在赋值时也成立，先转成字符串再进行赋值。
```
var arr = ['a', 'b', 'c'];
arr['0'] // 'a'
arr[0] // 'a'
```
+ 因为数值键名不符合标识符规范，不能使用点结构读取，数组成员只能用方括号 `arr[0]` 或 `arr['0']` **读取**。

### length 属性
+ 数组的 `length` 属性，返回数组的成员数量。
+ 只要是数组，就一定有 length 属性。length 是动态的值，等于键名中的最大整数加上 1.
```
var arr = ['a', 'b', 'c'];
length.arry   // 3

arr[9] = 'e';
arr.length // 10
```
+ JavaScript 使用 32 位整数保存数组的元素个数。这意味着，数组成员最多只有 4294967295 个（2^32 - 1）个，因此 length 属性的最大值是 4294967295。
+ length 属性是可写的。若设置一个小于当前成员个数的值，该数组的成员会自动减少到 length 设置的值。
+ 清空数组的一个有效方法，就是将 length 属性设为 0。
+ 若设置 length 大于当前元素个数，则数组的成员数量会增加到这个值，新增的位置都是空位。
```
var arr = [ 'a', 'b', 'c' ];
arr.length // 3

arr.length = 4
arr[3]   //undefined
arr.length = 2;
arr // ["a", "b"]
arr.length = 0;
arr // []
```
+ 由于数组本质上是一种对象，所以可以为数组添加属性，但是这不影响 length 属性的值。
```
var a = [];

a['p'] = 'abc';
a.length // 0
a[2.1] = 'abc';
a.length // 0
```

### in 运算符
+ 检查某个键名是否存在，适用于对象，也适用于数组。
+ 注意，如果数组的某个位置是空位，`in` 运算符返回 `false`。
```
var arr = [];
arr[100] = 'a';

100 in arr // true
1 in arr // false
```

### 遍历数组
+ `for...in` 循环可以遍历数组，但它不仅遍历数字键，还遍历非数字键。所以可以使用 `for` 循环或 `while` 循环遍历数组。
+ 数组的 `forEach` 方法也可以用来遍历数组：
```
var colors = ['red', 'green', 'blue'];
colors.forEach(function (color) {
  console.log(color);
});
// red
// green
// blue
```

### 数组的空位
+ 当数组的某个位置是空元素，即两个逗号之间没有任何值，我们称该数组存在空位（hole）。
+ 数组的空位是可以读取的，返回 undefined。
+ 用 `delete` 命令删除一个数组成员，会形成空位，且不影响 `length` 属性。
```
var a = [1, , 1];
a.length // 3
a[1]   // undefined

delete a[2];
a.length // 3
```
+ 空位：表示数组没有这个元素，遍历数组时空位会跳过
undefined：表示数组有这个元素，值是 undefined，遍历不会跳过

### 类似数组的对象
对象的所有键名都是正整数或零，并且有 length 属性，语法上称为“类似数组的对象”（array-like object）：
```
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};
```
+ 这种 length 属性不是动态值，不会随着成员的变化而变化。
+ 类似数组的对象（array-like object）并不是数组，`instanceof` 运算符返回 `false`：
```
arrayLike instanceof Array   // false
```

## Array 对象
### 构造函数
`Array` 是 JavaScript 的全局对象，同时是构造数组的一个构造函数。
1. 基本用法，以下三种写法都可以：
```
let f = new Array('a,' 'b')
let f = Array('a,' 'b')
let f = ['a', 'b']  // 常用方法
f // ["a", "b"]
```
对于**基本类型** Number、String 和 Boolean，构造函数时不加 new 创建的的是对应的基本类型，加 new 创建的是对象；对于**复杂类型** Object（包括 Array 和 Function），加与不加 new 创建的都是对象。
2. 创建空数组：
```
var a = new Array(3)
a  // [empty * 3]
a.length   // 3
```
Array 构造函数的参数 3，表示生成一个长度为 3 的数组，每个位置都是空值：![创建数组](https://upload-images.jianshu.io/upload_images/13038962-d8218b80895bf810.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 不一致性
```
var a = new Array(3, 3)
// [3, 3]
```
上面的例子，第一个参数不再指数组的长度，说明 Array 作为构造函数具有不一致性。内存图解：![不一致性](https://upload-images.jianshu.io/upload_images/13038962-0b29001a007d2ff7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### JS 中数组的本质
1. 我们对数组的理解：数组就是数据的有序集合。
JS 对数组的理解：数组就是原型链中有 `Array.prototype` 的对象。
+ Array 既有 `valueOf()`，`toString()` 等所有对象都拥有的静态方法，还有 `push()`、`pop()`、`shift()`、`join()` 等 Array 独有的方法（`Array.prototype`）。
2. Array 与 Object 的区别
```
var a = [1,2,3]
var obj = {0:1, 1:2, 2:3, length:3}
```
二者的内存是没有区别的，它们的区别是原型不同。内存图解：![Array 与 Object 的区别](https://upload-images.jianshu.io/upload_images/13038962-4fb7c20163f74f25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 伪数组
+ 定义：一个对象，有 `0,1,2,3,4,5...n,length` 这些 `key`，但原型链中没有 `Array.prototype`，这样的对象就是伪数组。
+ 目前知道的伪数组有
`arguments` 对象
`document.querySelectAll('div')` 返回的对象
![伪数组 arguments](https://upload-images.jianshu.io/upload_images/13038962-8a67364f00280a92.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 数组的 API
API：浏览器提供的接口。
1. **Array.prototype.forEach**：遍历
+ `forEach()` 方法对数组的所有成员依次执行参数函数。`forEach()` 方法的参数是一个函数，该函数接受三个参数：当前值 `value`、当前位置 `key`、整个数组。
+ 它不返回值，只用来操作数据。
```
arr.forEach( function(){} ) 
```
![forEach()](https://upload-images.jianshu.io/upload_images/13038962-068daa60296b1623.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ `for i` 循环和 `Array.prototype.forEach` 都可以遍历数组，二者区别为：
 `for` 是关键字，不是函数；`Array.prototype.forEach` 是一个函数。
 `for` 循环可以 `break` 和 `continue`；`Array.prototype.forEach` 不支持 `break` 和 `continue`.

2. **Array.prototype.sort**：排序
`sort()` 方法的内置排序一般是快排。排序后，**原数组被改变**。
默认按照字典顺序排序，若想自定义排序，可传入一个函数作为参数。
![sort()](https://upload-images.jianshu.io/upload_images/13038962-a411ce1ed3ff4281.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 返回 （x-y）和返回（y-x）的排序结果是不同的，一个从小到大排序，一个从大到小排序。

3. **Array.prototype.join**：连接数组成员为字符串
`join()` 方法以指定参数作为分隔符，将所有数组成员连接成一个字符串。
如果不提供参数，默认用逗号分隔。
![join()](https://upload-images.jianshu.io/upload_images/13038962-c585f8673926bc8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4. **Array.prototype.concat**：合并多个数组
`concat()` 方法用于多个数组的合并。将新数组的成员添加到原数组成员的后部，然后返回一个新数组，原数组不变。
![concat()](https://upload-images.jianshu.io/upload_images/13038962-995d8b58b1838757.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 特殊用法：用来复制一个数组。把当前数组 `concat` 一个空数组`[]`，返回一个新的数组。

5. **Array.prototype.map**：映射
+ `map()` 方法将数组的所有成员依次传入参数函数（与 `forEach` 相同），然后把每一次的执行结果组成一个新数组返回（与 `forEach` 不同）。
+ `map()`方法也接受一个函数作为参数，该函数调用时，`map()` 方法向它传入三个参数：当前值 `value`、当前位置 `key` 和数组本身。
![map()](https://upload-images.jianshu.io/upload_images/13038962-d0258d2dc3b5f909.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6. **Array.prototype.filter**：过滤
+ `filter()` 方法用于过滤数组成员，满足条件的成员组成一个新数组返回。
+ 它的参数是一个函数，所有数组成员依次执行该函数，返回结果为 `true` 的成员组成一个新数组返回。该方法不会改变原数组。
![filter()](https://upload-images.jianshu.io/upload_images/13038962-88777974fa7befe1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ **链式操作**，同时使用 `filter` 和 `map` 方法：
![链式操作](https://upload-images.jianshu.io/upload_images/13038962-d12d37ad506decda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7. **Array.prototype.reduce**
`reduce()` 方法从左到右依次处理数组的每个成员（从第一个成员到最后一个成员），最终累计为一个值。
![reduce()](https://upload-images.jianshu.io/upload_images/13038962-dc9de79a77a031f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)上面代码中，`reduce` 方法求出数组所有成员的和。第一个参数是一个函数，sum 为之前求和的结果，n 为当前数字，最后一个参数 0 是初始值。
+ `map` 和 `filter` 用 `reduce` 表示：![map 和 filter 用 reduce 表示](https://upload-images.jianshu.io/upload_images/13038962-0bfba19a493bd7a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

8. **Array.prototype.reverse**：颠倒
`reverse()` 方法用于颠倒排列数组元素，返回改变后的数组。该方法会**改变原数组**。
![reverse()](https://upload-images.jianshu.io/upload_images/13038962-7d3523737803ebdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

9. **Array.prototype.slice**：提取
+ `slice()` 方法用于提取目标数组的一部分，返回一个新数组，原数组不变。
```
arr.slice(start, end)
```
+ 它的第一个参数为起始位置（从 0 开始），第二个参数为终止位置（不包含该位置的元素本身）。如果省略第二个参数，则一直返回到原数组的最后一个成员。![slice()](https://upload-images.jianshu.io/upload_images/13038962-a06da8b7ce43bdcc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 特殊用法：最后一个例子 `slice` 没有参数，实际上等于返回一个原数组的拷贝。

10. **Array.prototype.splice**：删除
+ `splice()` 方法用于删除原数组的一部分成员，并可以在删除的位置添加新的数组成员，返回值是被删除的元素。该方法会**改变原数组**。
```
arr.splice(start, count, addElement1, addElement2, ...)
```
+ `splice` 的第一个参数是删除的起始位置（从0开始），第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。
![splice()](https://upload-images.jianshu.io/upload_images/13038962-a1ca36d0817c405e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
+ 如果只提供第一个参数，等同于将原数组在指定位置拆分成两个数组：![splice() 一个参数](https://upload-images.jianshu.io/upload_images/13038962-2f4006f5e5f99d3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

11. 例题：var a = [1,2,3,4,5,6,7,8,9]，计算所有奇数的和。
```
var a = [1,2,3,4,5,6,7,8,9]
a.reduce(function(arr, n){
	if(n % 2 !== 0){
		arr.push(n)
	}
	return arr
},[]).reduce(function(sum, n){
	return sum + n
},0)

// 或者
a.reduce( (sum,n)=> {
    return n%2===1 ? sum + n : sum
}, 0)
```
