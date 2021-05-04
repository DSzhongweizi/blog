---
title: link和@import
cover: false
categories:
  - 前端
  - CSS
  - CSS技术要点
tags:
  - link
  - import
abbrlink: 15d83fc0
date: 2020-09-26 12:21:58
updated:
---

- 属性不同
	- link是html提供的标签，不仅可以加载css文件，还能定义 RSS、rel 连接属性等；
	- @import是css中的语法规则。
- 加载顺序不同	
	- 页面打开时，link引用的css文件被加载；
	- @import引用的CSS等页面加载完后最后加载。
- 兼容性	
	- link是不存在兼容问题；
	- @import是css2.1后提出的。
- Dom控制	
	- js操作DOM，可以使用link改变样式；
	- 无法使用@import的方式导入的样式。
- 性能：link性能优于@import，后者少用。
- 权重（存疑）：link引入的样式权重大于@import引入的样式（虽然后被加载，却会在加载完毕后置于样式表顶部，最终渲染时自然会被下面的同名样式层叠。）
