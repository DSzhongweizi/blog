---
title: call，apply和bind的模拟实现
cover: false
categories:
  - 前端
  - JS
  - JS编程
tags:
  - call
  - apply
  - bind
abbrlink: 95cdf460
date: 2020-10-06 12:59:24
updated:
---
call和apply的作用都是显式绑定this对象，看下面代码，试想当调用call或者apply的时候，把 foo 对象改造成如下：
```js
var foo = {
    value: 1,
    bar: function() {
        console.log(this.value)
    }
};
foo.bar(); // 1 
```
这个时候 this 就指向了 foo，是不是很简单呢？

所以基本思想很简单，就是**把函数变成绑定对象的一个属性方法，然后再删除它**

## 实现call函数
```js
Function.prototype.call2 = function (obj) {
  obj = obj || window; //obj为null时情况
  obj.fn = this; //将函数变成绑定对象obj的一个属性，this为调用call2的函数

  var args = [...arguments].slice(1); //从第二个开始截取参数
  var res = obj.fn(...args) //展开参数数组
  delete obj.fn;  //删除多余的对象属性
  return res;
}
```

## 实现apply函数
apply和call函数的区别在于传入的参数形式不一样，apply的参数以数组形式传入，相比较不确定个数的call参数，我们可以不需要使用argments获取apply参数，而是显式定义。
```js
Function.prototype.apply2 = function (obj, arr) {
  var obj = obj || window;
  obj.fn = this;
  // 无参数时，arr = undefined，展开运算符无法迭代它，需要加以判断。
  var res = arr ? obj.fn(...arr) : obj.fn() 
  delete obj.fn; 
  return res;
}
```

## 实现bind函数
bind函数与call和apply大有不同，其返回值是一个函数。

{% note info %}
bind最重要一特点：
    一个绑定函数也能使用new操作符创建对象：这种行为就像把原函数当成构造器。提供的 this 值被忽略，同时调用时的参数被提供给模拟函数。
{% endnote %}
也就是说当 bind 返回的函数作为构造函数的时候，bind 时指定的 this 值会失效，但传入的参数依然生效。
```js
Function.prototype.bind2 = function (context) {
  var self = this;
  var args = [...arguments].slice(1);
  var fBound = function () {
    var bindArgs = [...arguments];
    // 当作为构造函数时，this 指向实例，此时结果为 true，将绑定函数的 this 指向该实例，可以让实例获得来自绑定函数的值
    // 以上面的是 demo 为例，如果改成 `this instanceof fBound ? null : context`，实例只是一个空对象，将 null 改成 this ，实例会具有 habit 属性
    // 当作为普通函数时，this 指向 window，此时结果为 false，将绑定函数的 this 指向 context
    return self.apply(this instanceof fBound ? this : context, args.concat(bindArgs));
  }
  // 修改返回函数的 prototype 为绑定函数的实例，实例就可以继承绑定函数中的值
  fBound.prototype = Object.create(self.prototype); 
  // fBound.prototype = new self(); 
  return fBound;
}
```
另一种参考
```js
Function.prototype.myBind = function(context) {
    let self = this;
    let args = [...arguments].slice(1);
    return function fn() {
        //处理函数使用new的情况
        args = args.concat(...arguments);
        return this instanceof fn ? new self(...args) : self.apply(context, args);
    };
};
```

补充
```
Object.create =  function (o) {
    var F = function () {};
    F.prototype = o;
    return new F();
};
```
