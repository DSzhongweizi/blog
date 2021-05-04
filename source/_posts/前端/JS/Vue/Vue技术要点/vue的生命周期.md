---
title: vue的生命周期
cover: false
categories:
  - 前端
  - JS
  - Vue
  - Vue 技术要点
tags:
  - vue
  - 生命周期
  - 父子组件
abbrlink: 36c1deec
date: 2020-09-26 14:08:32
updated:
---
Vue 的整个生命周期如下图：
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/vue-lifecycle.png" width=700 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">vue 生命周期</div>
</center>

## 父子组件生命周期执行顺序
加载渲染过程
父beforeCreate->父created->父beforeMount->子beforeCreate->子created->子beforeMount->子mounted->父mounted

更新过程
父beforeUpdate->子beforeUpdate->子updated->父updated

销毁过程
父beforeDestroy->子beforeDestroy->子destroyed->父destroyed
