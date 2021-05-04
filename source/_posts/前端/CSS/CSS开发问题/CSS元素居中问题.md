---
title: CSS元素居中
categories:
  - 前端
  - CSS
  - CSS开发问题
tags:
  - 居中
cover: false
abbrlink: 5b3d95b9
date: 2020-06-15 17:38:30
updated: 
---
居中在对象上分为行内元素居中，块级元素居中；在方式上分为水平居中，垂直居中，以及两者都要求居中。
## 行内元素
行内元素是指文本text、图像img、按钮超链接等。
### 水平居中
单行：只需给父元素设置text-align:center即可实现
多行：同单行
### 垂直居中
单行：只需设置line-height的值与父级元素height 一致即可
多行：设置该元素：`vertical-align: middle;`，还需设置该元素的父级元素：`display: table;`。

## 块级元素
### 水平居中
定宽：
- 给元素加`margin:0 auto`即可
- 父元素：`position:absolute/relative/fixed` 子元素：`left:50%` 和 `margin-left:-值（一半）`（需知宽高）
- 父元素：`position:absolute/relative/fixed` 子元素：`left:50%` 和 `transform:translateX(-50%)`
- 父元素：`position:absolute/relative/fixed` 子元素：`left/right:0` 和 `margin:auto`

不定宽：
- 父元素：`margin:0 auto;display:table;`
- 父元素：`text-align:center` 子元素：`inline-block`
- 父元素：`display:flex;justify-content:center;`



### 垂直居中
定宽：
- 父元素：`position:absolute/relative/fixed` 子元素：`top:50%` 和 `margin-top:-值（一半）`（需知宽高）
- 父元素：`position:absolute/relative/fixed` 子元素：`top:50%` 和 `transform:translateY(-50%)`
- 父元素：`position:absolute/relative/fixed` 子元素：`top/bottom:0` 和 `margin:auto`

不定宽：
- 父元素：`display:flex;align-items：center`



### 水平垂直居中
- 父元素：`display:flex;display:table;align-items：center`
- 父元素：`position:absolute/relative/fixed` 子元素：`left:50%;top:50%` 和 `margin-left:-值（一半）;margin-top:-值（一半）`（需知宽高）
- 父元素：`position:absolute/relative/fixed` 子元素：`left:50%;top:50%` 和 `transform:translate(-50%,-50%)`
- 父元素：`position:absolute/relative/fixed` 子元素：`left/right/top/bottom:0` 和 `margin:auto`
- 父元素：`display:table` 子元素：`display:table-cell;text-align:center;vertical-align: middle`（子元素里只能是行内（块）元素） 
