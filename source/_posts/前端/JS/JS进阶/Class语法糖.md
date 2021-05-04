---
title: Class语法糖
cover: false
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - class
  - 继承
  - super
abbrlink: 25110d9
date: 2020-09-13 10:53:40
updated:
---
## 类的基本概念
### class关键字
基本上，ES6 的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

不过，相比较ES5，class还是有一些不同的，这后面会讲到。

定义一个Point类，并导出。
```
class Point{} // 等同于const Point = class{}

export default Point // 等同于export class {Point}
```
表达式定义的类，如果在类的内部用到，可以指定内部类名。
```js
const Point = class innerPoint{}
```
### constructor方法
constructor 表示类的构造方法，和类的普通方法不同的是，constructor 在新建实例的时候会自动调用，即便没有显式定义，也是默认存在一个空constructor的。
```js
class Point {
}
// 等同于
class Point {
  constructor() {}
}
```
注意，类的数据类型就是函数，类本身就指向构造函数。
```js
typeof Point // "function"

Point === Point.prototype.constructor // true
```
constructor方法默认返回实例对象（即this），当然你可以指定显式返回其他对象，但不建议这样做。
### 普通方法
类的所有方法都定义在类的prototype属性上面，包括上面提到的constructor方法。
```js
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于
Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
```
向类同时添加多个方法
```js
Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});
```

类的方法（属性）名，可以采用表达式。
```js
let methodName = 'getArea';
class Square {
  constructor(length) {
    // ...
  }
  [methodName]() {
    // ...
  }
}
```
### 实例
类的实例使用new命令得到，实例可以继承类原型上的所有属性和方法，在类里，实例就是this值。
```js
//定义类
class Point {
  z = 0;  // 实例属性也可以定义在构造函数之外，只不过不能用构造函数初始化。
  constructor(x, y) {
    this.x = x; //这里的x是实例上的属性
    this.y = y;
  }
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
  toValue = () => this.x + this.y;
}
var point = new Point(2, 3);
point.toString(); // (2, 3)
```
类内部this指向可能改变
```js
const { toString，toValue } = point; // 单独取出类方法
toString(); // 报错，x,y未定义，方法的this值指向运行时的环境(全局环境，类内部严格模式，所以指向undefined)
toValue(); //5 不报错，箭头函数的this在为定义时所在的对象
```

类的所有实例共享一个原型对象
```js
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__ === p2.__proto__
//true
```
这也意味着，可以通过实例的`__proto__`属性为“类”添加方法，但不建议这样做。`__proto__` 并不是语言本身的特性，它只是各大浏览器厂商提供的一个私有属性，不提倡在生产中使用该属性。
> 使用 `Object.getPrototypeOf` 方法来获取实例对象的原型，然后再来为原型添加方法/属性。

### 静态方法
如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

静态方法包含this关键字，这个this指的是类，而不是实例。
```js
class Foo {
  static getThis() {
    return this;
  }
}

Foo.getThis() // 类本身

var foo = new Foo();
foo.getThis() // TypeError: foo.classMethod is not a function
```
静态方法虽然不能被实例继承，不过会被子类继承，后面继承会讲到。
### 静态属性
静态属性指的是 Class 本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。
```js
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```
类的静态属性只能在类外定义，不过新提案据说已经在发起。
## 类的继承
Class 可以通过`extends`关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。
```js
class Point {
}
class ColorPoint extends Point {
  // 由于没有部署任何代码，所以这两个类完全一样，等于复制了一个Point类
  // 新建子类实例，默认在构造函数里调用super()方法复制父类属性方法
}
```
在子类中，如果显式声明了constructor方法，则需要在constructor方法里显式调用父类构造函数（`super()`），否则新建实例时会报错。
> 这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。

如果不调用super方法，子类就得不到this对象。这是因为子类实例的构建，基于父类实例，只有super方法才能调用父类实例。
```js
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}
class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```

### super 关键字
super这个关键字，既可以当作函数使用，也可以当作对象使用，用法完全不同。
#### 函数使用
super作为函数调用时，代表父类的构造函数，ES6 要求，子类的构造函数必须执行一次super函数。super函数虽然代表了父类A的构造函数，但是**返回的是子类B的实例。**

作为函数时，super()也只能用在子类的构造函数之中，用在其他地方就会报错。

#### 对象使用
super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。
> 使用super的时候，必须显式指定是作为函数、还是作为对象使用，否则会报错。

ES6 规定，在子类普通/静态方法中通过super调用父类的方法时，方法内部的this指向当前子类的实例/子类。
```js
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
  static print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;
  }
  m() {
    super.print();  // 实际执行super.print.call(this)
  }
  static m() {
    super.print();  // 子类静态方法里调用的父类方法，也必须是静态方法
  }
}

let b = new B();
b.m() // 2

B.x = 3;
B.m() // 3
```
由于this指向子类实例，所以如果通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性。
```js
class A {
  constructor() {
    this.x = 1;   // 父类实例上属性
  }
}
class B extends A {
  constructor() {
    super();
    this.x = 2;   // 子类实例上的属性
    super.x = 3;  
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();
```
上面的有点匪夷所思，super不是指向父类原型对象，所以`super.x = 3`不等于`A.prototype.x = 3`吗？那访问super.x为啥是`undefined`？

由于对象总是继承其他对象的，所以可以在任意一个**对象方法**中，使用super关键字。
```js
var obj = {
  toString() {
    return "MyObject: " + super.toString();
  }
};

obj.toString(); // MyObject: [object Object]
```

### `prototype`和`__proto__`属性
大多数浏览器的 ES5 实现之中，每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。Class 作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。
- 子类的__proto__属性，表示构造函数的继承，总是指向父类。
- 子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。

```js
class A {
}

class B extends A {
}

B.__proto__ === A // true
B.prototype.__proto__ === A.prototype // true
```
这是因为，类的继承是按照下面的模式实现的：
```js
class A {
}

class B {
}
// B 的实例继承 A 的实例
Object.setPrototypeOf(B.prototype, A.prototype);

// B 继承 A 的静态属性
Object.setPrototypeOf(B, A);
```
`Object.setPrototypeOf()`方法效果同`Object.create()`，建议用后者

继承后的原型链，注意父类和子类，以及它们的实例之间的关系。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/class-prototype-link.png" width="600"/>
</center>



## 其他
### 取值函数（getter）和存值函数（setter）
与 ES5 一样，在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。
```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```
### `name`和 `new.target` 属性 
`name`属性总是返回紧跟在class关键字后面的类名
```js
class Point {}
Point.name // "Point"
```
`new.target` 属性返回new命令作用于的那个构造函数，如果构造函数不是通过new命令或Reflect.construct()调用的，new.target会返回undefined，因此这个属性可以用来确定构造函数是怎么调用的。

### `Object.getPrototypeOf`方法
`Object.getPrototypeOf方法`不仅可以获取实例的原型对象，还可以用来从子类上获取父类，所以使用这个方法判断，一个类是否继承了另一个类。
```js
Object.getPrototypeOf(ColorPoint) === Point // true
```

### 类和ES5的不同
- 类不同于ES5的构造函数，必须使用new调用，否则会报错。
- 类的内部所有定义的方法，都是不可枚举的，ES5的写法可以枚举。
- 类同模块一样，内部默认就是严格模式。
- 类本身不存在变量提升（hoist），ES5的构造函数可以在定义前`new`出实例，所以子类要继承父类必须写在父类后面。
- ES6的继承是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）