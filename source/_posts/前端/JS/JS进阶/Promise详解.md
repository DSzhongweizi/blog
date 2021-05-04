---
title: Promise详解
cover: false
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - Promise
  - 异步编程
abbrlink: 4b23cf7d
date: 2020-07-11 17:54:53
---

参考[Promise](https://es6.ruanyifeng.com/#docs/promise)

下面是Promise自己学习的一部分总结
> Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。
## Promise 的含义
所谓Promise，简单说就是一个容器对象，这个对象有三种状态：
- pending（进行中）
- fulfilled（已成功）
- rejected（已失败）

### Promise对象的特点
- 对象的状态不受外界影响。

	容器对象里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果，结果决定着最终的状态，
	> Promise 意思“承诺”，承诺状态只和结果有关，其他手段无法改变。

- 一旦状态改变，结果就确定了，并不再改变。
	
	Promise对象的状态改变，只有两种可能：从pending变为fulfilled（resolved）和从pending变为rejected

## 基本用法
ES6 规定，Promise对象是一个构造函数，用来生成Promise实例。
```js
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
```js
promise.then(function(value) {
  // resolved（成功）
}, function(error) {
  // rejected（失败）
});
```
> then方法可以接受两个回调函数作为参数，分别对应两个不同的状态切换结果，其中rejected对应的回调函数是可选的。

简单例子，注意：Promise 新建后就会立即执行
```js
let promise = new Promise((resolve, reject) => {
  console.log('Promise1');		 
  resolve();					//在本轮事件循环的末尾执行，才会执行回调函数，无论位置写在哪里
  console.log('Promise2');
});

promise.then(() => {		//实现了resolved结果的回调函数
  console.log('resolved.');
});

console.log('Hi!');				
/**********结果为***********/
		// Promise1
		// Promise2
		// Hi!
		// resolved
```
> then方法指定的回调函数，将在当前脚本**所有同步任务执行完**才会执行，所以resolved最后输出。

### 注意
一般来说，调用resolve或reject以后，Promise 的使命就完成了，后继操作应该放到then方法里面，而不应该直接写在resolve或reject的后面。
> 以上的`console.log('Promise2');`语句只是方便观察resolve()的执行事件

所以，最好在它们前面加上return语句，这样就不会有意外。
```js
new Promise((resolve, reject) => {
  return resolve(1);
  // 后面的语句不会执行
  console.log(2);
})
```
## 原型方法
Promise 实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。
### then方法
then方法返回的是一个**新的**Promise实例对象，因此可以采用链式写法，即then方法后面再调用另一个then方法，每个then方法也都可以接受两个回调函数作为参数，只是rejected一般不写。
> 注意，不是原来那个Promise实例。

```js
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);
```

### catch方法
Promise.prototype.catch()方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。
> catch()接受一个错误失败的回调函数，通常then()方法就写接受resolve的回调函数，当然你也可以都写在then方法里，一般不建议那样做。

Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止，也就是说，错误总是会被下一个catch语句捕获。
```js
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});
```
> 这也是上文建议catch()的写法，这样可以捕获前面then方法执行中的错误，也更接近同步的写法，当然，catch()后面还可以写then()方法。

###### 吃掉的错误
跟传统的try/catch代码块不同的是，如果没有使用catch()方法指定错误处理的回调函数，Promise 对象抛出的错误就不会被捕获，也不会传递到外层代码，即不会退出进程、终止脚本执行。
> 在浏览器的开发者模式里，会报错，打印抛出的错误，但不影响它后面代码的执行。

### finally方法
Promise.prototype.finally() 方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。
> 该方法是 ES2018 引入标准的，实际上也是then方法的特例。

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```
> 上面代码中，不管promise最后的状态，在执行完then或catch指定的回调函数以后，都会执行finally方法指定的回调函数。

## 静态方法
### `Promise.all()`
提供了并行执行异步操作的能力，并且在所有异步操作执行完后才执行回调：
- 所有异步结果都为resolved时，执行resolved回调
- 一旦有异步结果为rejected时，执行rejected回调

{% note warning %}
注意，如果作为参数的 Promise 实例，自己定义了catch方法(返回resolved实例)，那么它一旦被rejected，并不会触发Promise.all()的catch方法。
{% endnote %}

### `Promise.race()`
同`Promise.all()`，接受多个promise组成的数组，不同的是，数组中的某个promise状态率先改变（产生异步结果）的话，就会触发回调。

### `Promise.resolve()`和`Promise.reject()`
- 生成一个成功的promise实例  resolve()
- 生成一个失败的promise实例  reject()


## 注意
Promise对象的特点
- 对象的状态不受外界影响
- 一旦状态改变，就不会再变，任何时候都可以得到这个结果
- Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止
- Promise 内部的错误不会影响到 Promise 外部的代码，通俗的说法就是“Promise 会吃掉错误”。

`resolved`和`rejected`
- Promise对象的两个状态结果
- 对应`then`方法的两个回调函数（可以传参），rejected状态的回调函数可选
- 调用resolve或reject并不会终结 Promise 的参数函数的执行

`then`、`catch`和`finally`方法
- `then`方法返回新的Promise实例，可以有链式写法
- `catch`方法（也可以返回新的Promise实例）自动用来捕获rejected状态结果产生的错误
- `finally`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。

其他
- `Promise.resolve()`：将现有对象转为 Promise 对象
- `Promise.reject()`：同上