---
title: vue的nextTick
cover: false
categories:
  - 前端
  - JS
  - Vue
  - Vue 技术要点
tags:
  - vue
  - nextTick
abbrlink: 57175a7a
date: 2020-09-26 16:44:44
updated:
---
数据的变化到 DOM 的重新渲染是一个异步过程，发生在下一个 tick。

在开发的过程中，比如从服务端接口去获取数据的时候，数据做了修改，如果我们的某些方法去依赖了数据修改后的 DOM 变化，我们就必须在 nextTick 后执行，形如下面伪代码：
```js
getData(res).then(()=>{
  this.xxx = res.data
  this.$nextTick(() => {
    // 这里我们可以获取变化后的 DOM
  })
})
```
