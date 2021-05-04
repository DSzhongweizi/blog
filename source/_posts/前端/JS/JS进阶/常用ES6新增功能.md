---
title: 常用ES6新增功能
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - ES6
cover: false
abbrlink: 196a7a77
date: 2020-07-08 13:03:31
---
> 内容整理自JS数据结构与算法该书内容的第一章
# ES6
ES6的一些新功能
>使用let和const声明变量、模板字面量、解构、展开运算符、箭头函数、类、模板。
## let和const
> 两个的作用域都是块级的
### let
ES5可以用var关键字重复声明同一个变量，但ES6的let并不能。
```js
var a = 1;
var a = 2; //正常

let b = 1;
let b = 2; //报错
```
### const
const声明的变量是只读的，对于对象变量的引用也是只读的，不过对引用的属性是可以改写的（不建议修改）。
```js
const book = {
  name: "JS学习"
}
book = {} //报错
book.name = "CSS学习" //正常
```
## 模板字面量
利用模板字面量，我们创建字符串可以不在用拼接值。
```js
const book = {
  name: "JS学习"
}
let str1 = "我爱" + book.name ; //过去使用+符号拼接
let str2 = `我爱${book.name}`; //现在用``和${}拼接，这在多行拼接可以省略"\n"
```

## 箭头函数
极大的简化了函数的语法

一般写法
```js
var sum = function sum(a, b) {
  return a + b;
}
```
箭头函数
```js
var sum = (a, b) => a + b; //省略function和return...
```
> 1个参数可以不用括号，0个参数需要用空括号`()`代替，多行语句需要用大括号`{}`包裹，返回值需要`return`显式返回。

## 函数参数默认值
ES6中，函数的参数可以定义默认值
```js
function sum(a,b = 1) { 
  return a + b;
}
sum (1) //2
sum (1,2) //3
```
### arguments 对象
js函数的内置对象，是一个类数组对象，具有数据的length属性，可以用来获取不明确的参数值。

## 声明展开
ES5中，可以用apply()把数组转换为参数，ES6中，可以用展开运算符`...`代替
```js
let args = [1, 2, 3];
sum.apply(undefined, args); //ES5
sum(...args) //ES6
```
## 增强对象属性
### 数组解构
一次初始化多个变量
```js
let [x, y] = ['a', 'b'];
let obj  = {x, y}; //对象解构
```
交换数据```
```js
[x, y] = [y, x]
```
### 简写方法名
在对象中像属性一样声明函数
```js
const book = {
  name: "JS学习",
  read() {    //函数声明
   console.log('Reading'); 
  }
}
```
## 使用类面向对象编程
过去面向对象一般写法
```js
function Book(name, price){
  this.name = name;
  this.price = price;
}
Book.prototype.read = function () {   //基于原型的语法糖
  console.log('Reading');
}
```
使用class简化
```js
class Book{
  constructor(name, price) {    //构造函数
    this.name = name;
    this.price = price;
  }
  read () {   /普通函数
    console.log('Reading');
  }
}
```
### 继承
class类的继承写法,和Java，C++如出一辙。
```js
class ITBook extends Book {
  constructor(name, price, type) {    //构造函数
    super(name, price);   //继承父类构造方法
    this.type = type;
  }
  readIT () {
    console.log(this.type);
  }
}
```
### 使用属性存取器
形式上是类似其他语言一样封装私有变量，但效果上实际并不是。
```js
class Book {
  constructor(name) {   //构造函数
    this._name = name;    //私有变量属性名前加_
  }
  get name() {      //get函数
    return this._name;
  }
  set name(value) {   //set函数
    this._name = value;
  }
}
let book = new Book('JS学习');
console.log(book._name);
book.name = 'CSS学习';
console.log(book._name);
```
## 模块
ES6引入的模块，可以让我们像Node.js的require模块（CommonJS模块）进行模块化开发
### 导出模块
```js
const circleArea = r => 3.14 * (r ** 2);
const squareArea = s = s * s;
export { circleArea, squareArea }; //导出模块
```
> 可以直接在需要导出的模块前面加`export`关键字，比如`export const squareArea = s = s * s;`
### 引入模块
```js
import { circleArea, squareArea } from '模块相对文件路径';
```
> 当导入仅一个模块成员时，可以省略`{}`
### 模块重命名
导入导出的时候都可以重命名
```js
export { circleArea as circle, squareArea as square}; //导出重命名
import { circleArea as circle, squareArea as square} from '模块相对文件路径'; //导入重命名
import * as area from '模块相对文件路径'; //整个模块当作一个变量导入重命名
```

## 乘方运算符
过去指数运算
```js
const area = 3.14 * Math.pow(r, 2);
```
ES6引入`**`指数运算符
```js
const area = 3.14 * (r ** 2);
```
## 列表迭代器
## 类型数组
## Set/WeakSet
## Map/WeakMap
## 尾调用/尾逗号
## for...of
## Symbol
## 字符串补全

# TypeScript
TS是一个开源的，渐进式包含类型的JS超集
> 待补
