---
title: html文件解析过程
cover: false
categories:
  - 前端
  - 浏览器
tags:
  - html文件解析过程
abbrlink: '91497009'
date: 2020-07-12 22:39:59
---
浏览器的内核是指支持浏览器运行的最核心的程序，分为两个部分的，一是渲染引擎，另一个是JS引擎，渲染引擎在不同的浏览器中也不是都相同的。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200712224630.png" />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">html文件解析过程</div>
</center>

## 浏览器解析过程
构建DOM树和CSSOM树是同时进行的。
### 构建DOM树
浏览器会遵守一套步骤将HTML 文件转换为 DOM 树。
- 浏览器从磁盘或网络读取HTML的原始字节，并根据文件的指定编码（例如 UTF-8）将它们转换成字符串。
- 将字符串转换成Token，例如：`<html>、<body>`等，Token中会标识出当前Token是“开始标签”或是“结束标签”亦或是“文本”等信息。
- 生成节点对象并构建DOM
> 注意：一边生成Token的同时，另一边也生成了节点对象。

#### JS文件的加载
构建DOM的过程中可能遇到JS文件，JS的加载、解析与执行会阻塞DOM的构建
> 遇到JS，浏览器会将控制权移交给JS引擎，等JS引擎运行完毕，浏览器再从中断的地方恢复DOM构建

所以建议JS文件放在body标签的底部，不过defer 或者 async 属性的出现解决了这一问题。
### 构建CSSOM树
过程和DOM树的构建相似
> 该过程可能因为JS里操作了样式而跟随JS的加载、解析与执行一起完成，也就是会被迫先于DOM树的构建完成。

所以样式能用CSS做的就用CSS，JS虽然更强大，但代价更高。
### 构建渲染树
当我们生成 DOM 树和 CSSOM 树以后，就需要将这两棵树组合为渲染树。
> 渲染树只会包括需要显示的节点和这些节点的样式信息，如果某个节点是 display: none 的，那么就不会在渲染树中显示。

### 布局（回流）
当浏览器生成渲染树以后，就会根据渲染树来进行布局（也可以叫做回流），这一阶段浏览器要做的事情是要弄清楚各个节点在页面中的确切位置和大小，通常这一行为也被称为“自动重排”。
> 布局流程的输出是一个“盒模型”，它会精确地捕获每个元素在视口内的确切位置和尺寸，所有相对测量值都将转换为屏幕上的绝对像素。
### 绘制
布局完成后，浏览器会立即发出“Paint Setup”和“Paint”事件，通过调用操作系统Native GUI的API绘制，将渲染树转换成屏幕上的像素。

## 注意
DOM和CSSOM操作应尽量少一些，因为 DOM/CSSOM 是属于渲染引擎中的东西，而 JS 又是 JS 引擎中的东西。

JS操作DOM/CSSOM，需要两个线程的频繁通信，并且操作DOM/CSSOM，还可能导致重绘回流，势必会带来一些性能上的损耗，
