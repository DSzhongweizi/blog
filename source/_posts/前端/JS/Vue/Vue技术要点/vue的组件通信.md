---
title: vue的组件通信
cover: false
categories:
  - 前端
  - JS
  - Vue
  - Vue 技术要点
tags:
  - vue
abbrlink: 8d0d1102
date: 2020-09-26 18:52:27
updated:
---
## 父子组件
### 子组件访问父组件数据
#### $props
当前组件接收到的 props 对象，在子组件中定义 props 有三种方式（三种方式可以同时存在）:
```js
// 第一种数组方式
props: [xxx, xxx, xxx]
// 第二种对象方式
props: { xxx: Number, xxx: String}
// 第三种对象嵌套对象方式，默认支持 4 种可选属性
props: {
    xxx: {
        //类型不匹配会警告
        type: Number,
        default: 0,
        required: true,
        // 验证函数，返回值不是 true,会警告
        validator(val) { return val === 10}
    }
}
```
子组件中的模板
```html
<template>
    <button>{{xxx}}</button>
</template>
```
父组件中引用子组件
```html
<Child :xxx='yyy'></Child> // yyy为父组件传递给子组件的数据
```
#### $parent
指向子实例的父实例对象，这个使用起来很简单，在子组件中使用：
```js
this.$parent.xxx  //xxx为父实例的数据
```

#### provide/ inject
父组件中通过provide来提供变量, 然后再子组件中通过inject来注入变量。
> 这里**不论子组件嵌套有多深**, 只要调用了inject 那么就可以注入provide中的数据，而不局限于只能从当前父组件的props属性中回去数据

父组件中提供变量name
```js
provide: {
    name: 'xxx'
},
```

子组件中注入
```js
inject: ['foo'],
```

{% note warning %}
provide 和 inject 主要在开发高阶插件/组件库时使用,并不推荐用于普通应用程序代码中。
{% endnote %}

#### $attrs
键值对对象，包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (class 和 style 除外)

父组件中使用子组件
```html
<Child
  :name="name"
  :age="age"
  :gender="gender"
  :height="height"
  title="程序员成长指北"
></Child>
```
子组件中`props:['name']`，剩余的都包含在`this.$attrs`对象中，可以
> // this.$attrs = { "age": "18", "gender": "女", "height": "158", "title": "程序员成长指北" }

#### $listeners
类似$attrs，包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。
### 父组件访问子组件数据
#### $emit
参数为事件名和可选参数数组，用于触发当前实例上的事件，附加参数都会传给监听器（$on）回调。

子组件的模板
```html
<template>
    <button @click="addCount"></button> // 点击事件
</template>
```
传递给父组件的数据和`click`事件触发的回调方法`addCount`
```
data() {
    return {
        count:0,
    };
},
methods: {
     addCount () {
        this.$emit('add:count',this.count++)  //add:count事件名，其他任意小写字符串都行，父组件中监听得到就行
    }
},
```
父组件中使用子组件
```html
<Child @add:count="showCount"></Child>  //@后面的事件名对应$emit写的，showCount为该事件对应的回调函数
```
`add:count`事件对应的回调方法`showCount`
```js
methods: {
    showCount (count) {
        alert(count); //count对应$emit中第二个参数，
    }
},
```
#### $children
当前实例的直接子组件，是一个数据对象，里面存放着子组件实例，父组件中使用：
```js
this.$children[0].xxx
```
#### $refs
一个对象，有注册过 ref attribute 的所有 DOM 元素和组件实例

父组件中使用子组件
```html
<Child refs='xxx'></Child>  // xxx为子组件的id引用
```
`this.$refs.xxx`表示的就是子组件实例，假设子组件data属性中有数据`name`，父组件中使用`this.$refs.xxx.name`就能得到这个数据
## 兄弟组件
eventBus 又称为事件总线，在vue中可以使用它来作为沟通桥梁的概念，就像是所有组件共用相同的事件中心，可以向该中心注册发送事件或接收事件，所以组件都可以通知其他组件。

{% note warning %}
eventBus也有不方便之处, 当项目较大,就容易造成难以维护的灾难
{% endnote %}

步骤：
1. 初始化：创建一个事件总线并将其导出, 以便其他模块可以使用或者监听它
```js
// event-bus.js
import Vue from 'vue'
export const EventBus = new Vue()
```
父组件中引用两个子组件（兄弟组件）
```js
<Child1></Child1>
<Child2></Child2>
```
2. 发送事件：子组件`<Child1>`方法函数中发送事件
```js
EventBus.$emit('addition', {
  num:this.num++
})
```
3. 接收事件：子组件`<Child2>`方法函数中接受事件
```js
EventBus.$on('addition', param => {
  this.count = this.count + param.num;
})
```
移除事件的监听`EventBus.$off('addition', {})`

## 隔代组件
- $attrs：同父子组件中类似，可以通过`v-bind="$attrs"`传入到内部组件——在创建更高层次的组件时非常有用
- $listeners：同父子组件中类似，可以通过 `v-on="$listeners"` 传入内部组件——在创建更高层次的组件时非常有用

## 其他
原生js的`localStorage / sessionStorage`其实也可以用来通信，还比较简单，只不过是数据和状态比较混乱，不太容易维护。

👉[Vuex通信]()

常见使用场景可以分为三类:
- 父子组件通信: `props`、`$parent/$children`、`provide/inject`、`refs` 、 `$attrs/$listeners`
- 兄弟组件通信: `eventBus`、`vuex`
- 跨级通信:  `eventBus`、`vuex`、`provide/inject`、`$attrs/$listeners`
