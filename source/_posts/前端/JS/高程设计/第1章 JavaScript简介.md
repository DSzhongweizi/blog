---
title: 第1章 JavaScript简介
cover: false
categories:
  - 前端
  - JS
  - 高程设计
tags:
  - JavaScript简介
abbrlink: 2f62f20b
date: 2020-07-15 10:36:16
---
> 自己学习高程的笔记，记录于此。

JS诞生于1995年，它最初的主要目的是用来替代服务端频繁的处理浏览器输入表单验证的操作。
## JS往事
### 命名
JS最初是用在Netscape Navigator这款浏览器上，开始的名字叫LiveScript，是网景（Netscape）公司和Sun公司联合开发的，在发布的时候为了搭上当时炒的很热的Java，就把名字改为了JavaScript。

随后Internet Explorer和Netscape Navigator浏览器大战中，微软凭借着PC端 Win 系列桌面系统，以捆绑免费的方式，逐渐在市场中获得主动权。
### 标准化
在浏览器的大战中，微软和网景公司使用的脚本语言很相似，但又不同，两个都有自己的JS语法和特性的标准，这会给业界开发带来很多问题的，随即标准化就被提上了日程。

1997年，以JS 1.1版本为蓝图的建议被提交给ESMA（欧洲计算机制造商协会），该协会指定了TC39（39号技术委员会）负责“标准化一种通用、跨平台、供应商中立的脚本语言的语法和定义”，这个标准命名为ECMAScript（发音ek-ma-script）。

第二年，国际标准化组织ISO也采用ES标准。自此以后，浏览器开发商就将ES作为各自JS实现的基础。

## JS实现
JS含义比ES中规定的多很多，完整的JS由三个部分组成：
### 核心（ES）
ES和WEb浏览器没有依赖关系，Web浏览器只是ES可能的宿主环境之一。简而言之，ES是抽象的规定，JS作为Web浏览器的脚本语言，是具体实现ES中，其中的一种。

ES规定的内容包括：
- 语法
- 类型
- 语句
- 关键字
- 保留字
- 操作符
- 对象

### 文档对象模型（DOM）
DOM是针对XML，但经过扩展用于HTML的应用程序编程接口，它可以将HTML文档表示为有层次结构的树形图。

通过DOM，开发人员可以很好的控制页面内容和结构，轻松地对节点进行增删改查。
#### 为什么要使用DOM
DOM的出现也是标准化的过程，最初的IE4和NN4都分别支持了不同形式的DHTML（动态 HTML），另一个WEb通信标准组织W3c（万维网联盟）开始着手规划DOM。
#### DOM级别
DOM目前共有三级，每一级都是一个功能的扩充。
> DOM并不是针对JS的，其他的语言有的也实现了DOM。

### 浏览器对象模型（BOM）
BOM是可以访问和操作浏览器窗口的浏览器对象模型，最早出现在IE3和NN3中，直到HTML5，BOM功能才被写入正式规范。
