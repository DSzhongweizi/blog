---
title: vue的双向绑定
cover: false
categories:
  - 前端
  - JS
  - Vue
  - Vue 技术要点
tags:
  - vue
  - 数据绑定
abbrlink: 8bc55543
date: 2020-09-26 16:48:03
updated:
---
## 数据双向绑定
vue的响应式原理：
![](https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/vue%E5%93%8D%E5%BA%94%E5%BC%8F%E5%8E%9F%E7%90%86.png)
**所谓的数据绑定：数据变化更新视图（数据劫持），视图变化更新数据（绑定事件）。**

在Vue内部，Vue里面的数据绑定是通过数据劫持的方式来实现的，通过Object.defineProperty方法属性拦截的方式，把data对象里每个数据的读写转化成getter/setter，当数据变化时通知视图更新。

基本流程：
1. 实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。
2. 实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。
3. 实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。

![](https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/vue-%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A.png)

vue实现双向绑定的设计模式叫做发布订阅者模式

### 监听器Observer
**监听器——对数据属性变化进行劫持监听**
```js
/**
 * 把一个对象的每一项都转化成可观测对象
 * @param { Object } obj 对象
 */
function observable(obj) {
    if (!obj || typeof obj !== 'object') {
        return;
    }
    let keys = Object.keys(obj);
    keys.forEach((key) => {
        // 为obj的每个属性添加getter和setter
        defineReactive(obj, key, obj[key])
    })
    return obj;
}
/**
 * 使一个对象转化成可观测对象
 * @param { Object } obj 对象
 * @param { String } key 对象的key
 * @param { Any } val 对象的某个key的值
 */
function defineReactive(obj, key, val) {
    let dep = new Dep();    // 植入订阅器
    Object.defineProperty(obj, key, {
        enumerable: true,
        configurable: true,
        get() {
            if (Dep.target) {.  // 判断是否需要添加订阅者
                dep.addSub(Dep.target); // 在这里添加一个订阅者
            }
            console.log(`${key}属性被读取了`);
            return val;
        },
        set(newVal) {
            val = newVal;
            console.log(`${key}属性被修改了`);
            dep.notify()  // 数据变化通知所有订阅者
        }
    })
}
```

**消息订阅器——专门收集这些订阅者，联系监听器和订阅者**
```js
/**
 * 依赖收集容器，也就是消息订阅器Dep，用来容纳所有的“订阅者”
 */
class Dep {
    constructor() {
        this.subs = []
    }
    //增加订阅者
    addSub(sub) {
        this.subs.push(sub);
    }
    //通知订阅者更新
    notify() {
        this.subs.forEach((sub) => {
            sub.update()
        })
    }

}
Dep.target = null; // 恢复成上一个状态，释放自己
```
### 订阅者Watcher
```js
/**
 * 订阅者Watcher
 */
class Watcher {
    constructor(vm, exp, cb) {
        this.vm = vm;   // 一个Vue的实例对象
        this.exp = exp; // node节点的v-model或v-on：click等指令的属性值。如v-model="name"，exp就是name;
        this.cb = cb;   // Watcher绑定的更新函数;
        this.value = this.get();  // 将自己添加到订阅器的操作
    }
    get() {
        Dep.target = this;  // 缓存自己
        let value = this.vm.data[this.exp]  // 强制触发执行监听器里的get函数
        Dep.target = null;  // 释放自己
        return value;
    }
    // 当数据发生变化时调用Watcher自身的更新函数进行更新的操作
    update() {
        let value = this.vm.data[this.exp];
        let oldVal = this.value;
        // 对比新旧数据，判断是否调用更新函数cb进行更新。
        if (value !== oldVal) {
            this.value = value;
            this.cb.call(this.vm, value, oldVal);
        }
    }
}
```

测试代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <h1 id="name"></h1>
    <input type="text">
    <input type="button" value="改变data内容" onclick="changeInput()">

    <script src="observer.js"></script>
    <script src="watcher.js"></script>
    <script>
        function myVue(data, el, exp) {
            this.data = data;
            observable(data);                      //将数据变的可观测
            el.innerHTML = this.data[exp];           // 初始化模板数据的值
            new Watcher(this, exp, function (value) {
                el.innerHTML = value;
            });
            return this;
        }

        var ele = document.querySelector('#name');
        var input = document.querySelector('input');

        var myVue = new myVue({
            name: 'hello world'
        }, ele, 'name');

        //改变输入框内容
        input.oninput = function (e) {
            myVue.data.name = e.target.value
        }
        //改变data内容
        function changeInput() {
            myVue.data.name = "难凉热血"

        }
    </script>
</body>
</html>
```