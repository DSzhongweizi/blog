---
title: async和defer的作用是什么？有什么区别?
cover: false
categories:
  - 前端
  - 浏览器
abbrlink: b5a5b865
date: 2020-07-13 01:31:06
tags:
---
defer 和 async 属性的区别
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200713013200.png" width=600 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">蓝色线代表JavaScript加载；红色线代表JavaScript执行；绿色线代表 HTML 解析</div>
</center>

这两个语句都可以让浏览器在加载JS文件的同时，也会继续HTML文件的解析。

不同的地方在于
- defer（延迟执行）加载完后，并不会立即执行，直到html文件解析完毕
- async（异步下载）不一样，它加载完后就立马执行，这可能会阻塞html文件的解析
	> 如果html已经解析完那就没事，否则就会暂停html解析，先执行完JS，再继续解析

> 另外，在加载多个JS脚本的时候，async是无顺序的加载，而defer是有顺序的加载。