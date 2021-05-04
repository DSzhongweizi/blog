---
title: 深入理解js
cover: false
categories:
  - 前端
  - JS
  - JS进阶
abbrlink: 8dc3e3
date: 2020-09-05 14:14:22
tags:
---
## 全局变量和全局对象
js通过函数管理作用域，函数内部的声明的变量属于局部变量，未声明的变量以及函数外部的变量属于全局变量。

js环境存在一个全局对象，在函数外可以通过`this`访问到，所有的全局变量都是全局对象的属性，全局对象存在一个指向自己的属性值`window`，即`this.window === this`，自然所有的全局变量也是`window`的属性。
```js
a = 1;
this.a === this.window.a === window.a === a;//true
```

### 全局变量的问题
污染全局命名空间——所有的js应用程序和web页面上的所有代码都共享同一个全局变量，后面的赋值会覆盖前面的，导致期望以外的结果
> 这些js代码可能并不全是开发者自己写的，比如第三方组件库，广告脚本等等

### 糟糕的隐式全局变量
全局变量可能被我们不自觉的引入，比如下面的情况：
- 函数中使用任务链进行部分var声明：`var a = b = 0`，自右向左的赋值导致了`b`变量是全局变量

### 全局变量vs隐式全局变量
它们都是全局变量，唯一区别是——隐式全局变量可以通过`delete`操作符删除
### 单var形式
```js
function func() {
	var a = 1,
		b = 2,
		sum = a + b,
		myobject = {},
		i,
		j;
	// function body...
}
```
这是一种同时声明多个变量极好的方法——可以防止逻辑错误，增加可读性，确定变量用途

### var散布问题
函数内部的变量存在一个预（置顶）解析的过程，这会导致声明赋值过的全局变量被重新声明赋值的局部变量覆盖。
```js
myname = "global"; // 全局变量
function func() {
	alert(myname); // "undefined"
	var myname = "local";
	alert(myname); // "local"
} 
func();
```
变量声明被提前了！并被初始化为`undefined`
```js
myname = "global"; // global variable
function func() {
	var myname; // 等同于 -> var myname = undefined;
	alert(myname); // "undefined"
	myname = "local";
	alert(myname); // "local"
}
func();
```
