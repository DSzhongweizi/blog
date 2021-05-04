---
title: CSS清除浮动
categories:
  - 前端
  - CSS
  - CSS开发问题
tags:
  - 清除浮动
  - bfc
cover: false
abbrlink: b7cb6b1b
date: 2020-07-05 23:47:46
---
**浮动的定义**
使元素脱离文档流，按照指定方向发生移动，遇到父级边界或者相邻的浮动元素停了下来。

> 当容器的高度为auto，且容器的内容中有浮动（float为left或right）的元素，在这种情况下，容器的高度不能自动伸长以适应内容的高度，使得内容溢出到容器外面而影响（甚至破坏）布局的现象，**称为浮动溢出，或者父元素高度塌陷。**
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200706082930.jpg
" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">浮动的img</div>
</center>

## 清除浮动
为了防止浮动溢出现象的出现而进行的CSS处理，叫CSS清除浮动。
> 前两种原理本质一样

### clear
在浮动元素的后相邻兄弟块级元素使用样式
> 清除具体的块级元素旁边的浮动样式
```
.clearfix {
    clear:left|both|right;
}
```

### :after
在包裹浮动元素的父元素应用下列样式
> 针对浮动元素后面无相邻兄弟块级元素的情况（相当于添加兄弟元素）
```
.clearfix:after {
    content: '.';
    height: 0;
    display: block;
    clear: both;
}
```

### overflow
在包裹浮动元素的父元素应用下列样式
> 构建一个父级块格式化上下文（BFC）区域，利用了BFC的特性
```
.clearfix {
    overflow: auto/hidden;  //值不为visible就行，不同的值可能会有不同的小问题
    zoom: 1; //适配IE6
}
```


