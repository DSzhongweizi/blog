---
title: for...of与for...in的区别
cover: false
top_img: linear-gradient( 135deg, #E2B0FF 10%, #9F44D3 100%)
categories:
  - 前端
  - JS
  - 区别系列
tags:
  - for
abbrlink: e6e6de21
date: 2021-03-16 12:06:53
updated:
---

## for...of与for...in的区别
无论是for...in还是for...of语句之间的主要区别，在于它们的迭代方式：
- for...in 语句以任意顺序迭代对象的可枚举属性。
- for...of 语句遍历可迭代对象定义要迭代的数据。

以上定义需要注意几点：
1. 区别可枚举属性和可迭代对象
    - 可枚举属性：指那些内部 “可枚举” 标志设置为 true 的属性，对于通过直接的赋值和属性初始化的属性，该标识值默认为即为 true，对于通过 Object.defineProperty 等定义的属性，该标识值默认为 false。
    - 可迭代对象：一个（原型链上）拥有 `Symbol.iterator` 属性的对象，被认为是可迭代对象，String、Array、TypedArray、Map 和 Set 都是内置可迭代对象，**Object则不是。**
2. `for...in`任意顺序迭代属性（key），`for...of`迭代数据(value)

看下面代码：
```js
Object.prototype.objCustom = function() {};
Array.prototype.arrCustom = function() {};

let iterable = [3, 5, 7];
iterable.foo = 'hello';
console.log(iterable)   // [3, 5, 7, foo: "hello"]

// 继承的可枚举属性也被for...in迭代
for (let i in iterable) {
  console.log(i); // 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i in iterable) {
  if (iterable.hasOwnProperty(i)) {
    console.log(i); // 0, 1, 2, "foo"
  }
}
// for...of循环调用遍历器接口，数组的遍历器接口只返回具有数字索引的属性，"foo"因此被排除
for (let i of iterable) {
  console.log(i); // 3, 5, 7
}
```