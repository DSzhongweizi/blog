---
title: Generator函数
cover: false
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - Generator函数
  - 异步编程
abbrlink: 4dc42fb7
date: 2020-07-11 21:00:07
---
参考[Generator](https://es6.ruanyifeng.com/#docs/generator)

## 基本概念
Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同。
### 语法上
Generator 函数是一个状态机，封装了多个内部状态。执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。

返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
### 形式上
Generator 函数是一个普通函数，但是有两个特征。
1. function关键字与函数名之间有一个星号；
2. 函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）。

```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```
> 该函数有三个状态：hello，world 和 return 语句（结束执行）。

调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，一个遍历器对象（Iterator Object）。
> 调用方法和普通函数无异，主要是调用了并不立即执行，需要调用遍历器对象的next方法，使得指针移向下一个状态。

## yield 表达式

由于 Generator 函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。

Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行。
> 该标志的存在，实现了一个函数可以多次返回不同的值。

### yield 和 return
这两个有很多相似的地方，但也有区别。
- 相似：两个表达式都可以表示一种状态，都能返回一个值。
- 不同：yield表达式在一个函数里，同一个条件下也可以有很多个，并且都可以执行，返回多个值，也可以说 Generator 生成了一系列的值，这也就是它的名称的来历

return表达式在一个函数里的同一个条件下只能有一个，对于整个函数而言，不同的条件下也最多执行一次，且返回一个值。

### next方法
yield表达式本身没有返回值，或者说总是返回undefined。

next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。
> 由于next方法的参数表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的。


### 注意
- yield表达式只能用在 Generator 函数里面，用在其他地方都会报错。
- yield表达式如果用在另一个表达式之中，必须放在圆括号里面。
- yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号。

## 与 Iterator 接口的关系
任意一个对象的`Symbol.iterator`方法，等于该对象的遍历器生成函数，调用该函数会返回该对象的一个遍历器对象。

由于 Generator 函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的Symbol.iterator属性，从而使得该对象具有 Iterator 接口。
```js
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]  Iterator 对象可以被...运算符遍历
```

## `Generator.prototype.throw()`
Generator 函数返回的遍历器对象，都有一个throw方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。
> 当然Generator 函数体内抛出的错误，也可以被函数体外的catch捕获。
```js
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b   
```
> 第二次错误，由于 Generator 函数内部的catch语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的catch语句捕获。

- 如果 Generator 函数内部没有部署try...catch代码块，那么throw方法抛出的错误，将被外部try...catch代码块捕获。
- 如果 Generator 函数内部和外部，都没有部署try...catch代码块，那么程序将报错，直接中断执行。
- throw方法抛出的错误要被内部捕获，前提是必须至少执行过一次next方法，用来启动执行 Generator 函数的内部代码。
- throw方法被捕获以后，会附带执行下一条yield表达式，执行一次next方法。
- 多个yield表达式，可以只用一个try...catch代码块来捕获错误，回调函数的写法就要多个。
- 一旦 Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。

## `Generator.prototype.return()`
Generator 函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且终结遍历 Generator 函数。
```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next()        // { value: undefined, done: true }
```
> 遍历器对象g调用return方法后，返回值的value属性就是return方法的参数foo。并且，Generator 函数的遍历就终止了，返回值的done属性为true，以后再调用next方法，done属性总是返回true。

如果 Generator 函数内部有try...finally代码块，且正在执行try代码块，那么return方法会导致立刻进入finally代码块，执行完以后，整个函数才会结束。
```js
function* numbers () {
  yield 1;
  try {
    yield 2;
    yield 3;
  } finally {
    yield 4;
    yield 5;
  }
  yield 6;
}
var g = numbers();
g.next() // { value: 1, done: false }
g.next() // { value: 2, done: false }
g.return(7) // { value: 4, done: false }
g.next() // { value: 5, done: false }
g.next() // { value: 7, done: true }
```
调用return()方法后，就开始执行finally代码块，不执行try里面剩下的代码了，然后等到finally代码块执行完，再返回return()方法指定的返回值。

## next()、throw()、return() 的共同点
它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换yield表达式。

next()是将yield表达式替换成一个值，如果next方法没有参数，就相当于替换成undefined。
```js
const g = function* (x, y) {
  let result = yield x + y;
  return result;
};

const gen = g(1, 2);
gen.next(); // Object {value: 3, done: false}

gen.next(1); // Object {value: 1, done: true}
// 相当于将 let result = yield x + y
// 替换成 let result = 1;
```

throw()是将yield表达式替换成一个throw语句。
```js
gen.throw(new Error('出错了')); // Uncaught Error: 出错了
// 相当于将 let result = yield x + y
// 替换成 let result = throw(new Error('出错了'));
```

return()是将yield表达式替换成一个return语句。
```js
gen.return(2); // Object {value: 2, done: true}
// 相当于将 let result = yield x + y
// 替换成 let result = return 2;
```

## yield* 表达式
如果在 Generator 函数内部，调用另一个 Generator 函数，需要在前者的函数体内部，自己手动完成遍历，嵌套多了很麻烦。

`yield*`表达式是一种替代方案，如果yield表达式后面跟的是一个遍历器对象，需要在yield表达式后面加上星号，表明它返回的是一个遍历器对象，这被称为`yield*`表达式。
```js
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  for (let v of foo()) {
    yield v;
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```