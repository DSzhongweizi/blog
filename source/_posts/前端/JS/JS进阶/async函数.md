---
title: async函数
cover: false
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - async函数
  - 异步编程
abbrlink: 4f346c5
date: 2020-09-11 21:03:26
---
async函数是 Generator 函数的语法糖，使得异步操作变得更加方便。
> async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已。

async函数对 Generator 函数的改进，体现在以下四点：
- 内置执行器：Generator 函数的执行必须靠执行器，所以才有了co模块，而async函数自带执行器。也就是说，async函数的执行，与普通函数一模一样，只要一行。
- 更好的语义：async和await，比起星号和yield，语义更清楚了。async表示函数里有异步操作，await表示紧跟在后面的表达式需要等待结果。
- 更广的适用性：co模块约定，yield命令后面只能是 Thunk 函数或 Promise 对象，而async函数的await命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。
- 返回值是 Promise：async函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了，你可以用then方法指定下一步的操作。进一步说，async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await命令就是内部then命令的语法糖。

## 基本用法
async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

### then方法
- 参数：async函数内部return语句返回的值，会成为then方法回调函数的参数。
- 执行时机：async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。

### catch方法
- await命令后面的 Promise 对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到
- reject状态如果没有被catch方法处理，那么整个async函数都会中断执行。

### 注意
- await命令后面的Promise对象，运行结果可能是rejected，所以最好把await命令放在try...catch代码块中进行处理。
- 多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发（作为变量的赋值）。
- await命令只能用在async函数之中，如果用在普通函数，就会报错。
