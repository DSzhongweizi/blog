---
title: this指向问题
cover: false
categories:
  - 前端
  - JS
  - JS进阶
abbrlink: bcf5b237
date: 2020-09-25 22:36:57
updated:
tags:
  - this
  - 绑定
  - 箭头函数
  - call
  - apply
  - bind
---
js中this的指向大体上分为：new，显式，隐式，默认。

## 绑定方式
### 默认绑定
作用于（内部）函数直接调用的情况下，此时this指向全局对象，但严格模式下this指向undefined。
```js
function foo () {
    console.log(this)
}
foo() // => window 默认绑定

// 严格模式
function bar () {
    'use strict'
    console.log(this)
}
bar() // => undefined

```

### 隐式绑定
this指向它的调用者，即谁调用函数，他就指向谁。
```js
function foo () {
    console.log(this)
}
const obj = {
    foo: foo
}

obj.foo() // => obj调用foo，则this指向obj
foo() // => window 默认绑定
```
默认绑定有些情况下也可以理解为隐式绑定（`window.foo()`）
#### 隐式绑定的this丢失问题
常见于传入回调函数时。如：
```js
const obj = {
    foo: function () {
        console.log(this)
    }
}
setTimeout(obj.foo, 0) // => window ？？？为何？？？
setTimeout(function () {
    obj.foo() // => obj
}, 10)
```
函数也是引用类型，obj.foo作为参数传递给其他函数时，传递的是引用，所以传递的就是这个标识对应的匿名函数本身，和其所处的位置无关，在不在obj中都无所谓，哪怕n层嵌套。

这种不带任何修饰的默认绑定函数调用，限制了一些应用场景，不过可以用显式绑定解决这个问题。

### 显示绑定
call、apply或bind是js用来显式指定this的方法
```js
function foo() {
    console.log( this.a );
} 
 
var obj = {
    a: 2
}; 
 
foo.call( obj ); // => 2
foo.apply( obj ); // => 2

var tmp = foo.bind(obj)
tmp() // => 2
```
这些都是常规操作，但是有三点需要注意：
- call和apply是立即执行，bind则是返回一个绑定了this的新函数，只有你调用了这个新函数才真的调用了目标函数
- bind函数存在多次绑定的问题，如果多次绑定this，则以第一次为准，称为硬绑定。

#### 硬绑定
```js
function foo() {
    console.log( this.a );
} 
var obj1 = {
    a: 'obj1'
}; 
var obj2 = {
    a: 'obj2'
}

var tmp = foo.bind(obj1).bind(obj2)
tmp() // => 'obj1'
tmp.call(obj2) // => 'obj1'
```
为什么这样？看下面下面bind的简单版本。
```js
Function.prototype.bind = function (context, ...args) {
    const fn = this // 这个this就是我们绑定的函数，如上例即foo
    return function (...props) {
        fn.call(context, ...args, ...props)
    }
}

```
可以看到，bind就是给函数套一层函数，利用柯里化，提前设置好上下文对象。
> 根据这个原理，可以知道，无论你外面包裹了多少层，目标函数不会变。且由于闭包的存在，最初的这个context对象也不会变，所以当包裹在外层的函数一层层褪去后，最终使用到的context对象依旧是第一次绑定的对象。

bind函数只能绑定一次，多次绑定是没有用的，绑定后的函数this无法改变，即使call也不行，所以才称作硬绑定。

不过凡事总有例外，且看new绑定。

### new绑定
使用new操作符时实际做了四件事：
1. 创建一个全新的对象
2. 执行原型链接
3. 这个新对象会被绑定到构造函数中的this
4. 执行构造函数，判断返回值，如果为对象，则返回这个值，否则返回默认创建的对象

## 优先级
**优先级：new > 显式 > 隐式 > 默认**

于是我们判断this，就有了一个顺序：
1. 函数是否在new中调用？
2. 是否通过call、apply、bind等调用？
3. 是否在某个上下文对象中调用？
4. 都不是则是默认绑定。且严格模式下绑定到undefined。

当然你也可以像下面这种判断方式：
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/this%E6%8C%87%E5%90%91.png" width=600 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">this指向</div>
</center>

## 其他
### null或undefined
若将null、undefined等值作为call、apply的第一个参数，那么实际调用时会被忽略，从而应用到默认绑定规则，即绑定到window上，**有些时候我们不关心上下文，只关心参数时，可以这样做。**

{% note warning %}
但这样其实存在这一些潜在的风险，绑定到window很可能无意中添加或修改了全局变量，造成一些隐蔽的bug。
所以为了防止这种情况出现，可以将第一个参数绑定为一个空对象，当然具体还是看需求，这只是建议。
{% endnote %}

### 软绑定
关于软绑定，其实就是用来解决硬绑定后this无法再修改的问题
```js
Function.prototype.softBind = function(obj){
    var fn = this;
    var args = Array.prototype.slice.call(arguments,1);
    var bound = function(){
        return fn.apply((!this || this === (window || global)) ? obj : this, args.concat.apply(args,arguments));
    };
    bound.prototype = Object.create(fn.prototype);
    return bound;
};
```

关注点在这一行：
```js
// 判定当前this，如果绑定到了全局对象或undefined，null，
// 则修改this为传入的obj，否则什么也不做。
(!this || this === (window || global)) ? obj : this
```

意义何在？
```js
var name = 'global'
const obj = {
    name: 'obj',
    foo: function () {
        console.log(this.name)
    }
}
const obj1 = {
    name: 'obj1'
}

obj.foo() // => obj // 常规方式
setTimeout(obj.foo, 0) // this丢失，global
// 现在我们使用软绑定
const softFoo = obj.foo.softBind(obj)
setTimeout(softFoo, 0) // obj
softFoo.call(obj1) // obj1，可以使用call显式改变this
obj1.foo = softFoo
obj1.foo() // obj1，也可以隐式改变this
```

有了软绑定之后，排序为：new > 显式 > 隐式 > 软绑定 > 默认

### 箭头函数
- 函数体内的this对象，就是**定义时所在的对象**，而不是使用时所在的对象。
- 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
- 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
- 不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

**尤其是第一条值得注意，当定义时所在的对象的this改变时，箭头函数本身的this也将跟随改变。**

另外，对象不构成单独的作用域，在对象中慎用箭头函数。
### this赋值修改无效
在函数执行过程中，this一旦被确定，就不可更改了。
```js
var a = 10;
var obj = {
  a: 20
}

function fn() {
  this = obj; // 这句话试图修改this，运行后会报错
  console.log(this.a);
}

fn();
```


参考：[this绑定的四种方式：new，显式，隐式，默认](https://juejin.im/post/6844904113352736776#heading-10)