---
title: 第11章 DOM扩展
cover: false
categories:
  - 前端
  - JS
  - 高程设计
tags:
  - DOM扩展
abbrlink: cd6cb0e7
date: 2020-07-27 23:49:48
---
## 选择符 API
根据CSS选择符选择与某个模式匹配的DOM元素，`Selectors API`是这方面的主要扩展。
- `querySelector()`方法，传入CSS选择符，返回与该模式匹配的第一个元素，找不到返回null
	> Document类型调用，在文档元素范围内匹配；Element类型调用，在该元素后代范围内匹配查找

- `querySelectorAll()`方法，同`querySelector()`，只不过匹配返回的是一个NodeList对象
- `matchesSelector()`方法，传入CSS选择符，存在则返回true

## 元素遍历
对于元素间的空格，大部分浏览器会返回文本节点，这会导致`childNodes`和`firstChild`等属性的行为不一致，下面的一组属性是新规范为弥补这一差异导致的。
- `childElementCount`：返回子元素（除去文本节点和注释）的个数
- `firstElementChild`：指向第一个元素
- `lastElementChild`：指向最后一个元素
- `previousElementSibling`：指向前一个同辈元素
- `nextElementSibling`：指向后一个同辈元素

## HTML5
HTML5规范和传统HTML不同，它主要围绕如何使用新增标记定义大量的JS API，而不是定义标记。
### 与类相关的扩充
为增加对CSS类的class属性的操作，增加了新的API
- `getElementsByClassName()`：接受一个参数，包括一个或多个类名，返回带有指定类的所有元素的NodeList，`document`和`element`对象可以调用
- `classList`：新集合类型DOMTokenList的实例，该属性列表对象上，有操作类名实现类名的添加、删除和替换的方法：
	- `add(value)`：将给定的字符串值添加到列表中，如果已存在则不添加
	- `contains(value)`：判断列表中是否存在给定的值
	- `remove(value)`：从列表中删除给定的字符串
	- `toggle(value)`：如果列表已经存在给定值，删除它；如果不存在，添加它

### 焦点管理
- `document.activeElement`属性，始终引用DOM中当前获得了焦点的元素
> 元素焦点的方式有页面加载、用户输入和代码中调用`focus()`方法

默认情况下，文档刚刚加载完成时，`document.activeElement`中保存的是`document.body`元素的引用；文档加载期间，`document.activeElement`的值为null。

- `document.hasFocus()`方法，用于确定文档是否获得了焦点，判断用户是否在和页面交互

查询文档获知哪个元素获得了焦点，以及确定文档是否获得了焦点，这两个功能最重要的用途是提高Web应用的无障碍性

### HTMLDocument的变化
- `readState`：指示文档是否加载完毕，包括`loading`和`complete`两个值
- `compatMode`：告诉开发人员浏览器采用了哪种渲染模式（标准模式值为`CSS1Compat`，混杂模式值为`BackCompat`）
- `head`：作为`document.body`的补充，引用文档的`head`元素

### 字符集属性
- `charset`：表示文档实际使用的字符集
- `defaultCharset`：表示浏览器或操作系统文档默认的字符集

### 自定义数据属性
HTML规定，为元素添加非标准的属性，需要添加`data-`前缀，目的是为元素提供与渲染无关的信息，添加的自定义属性可以通过元素的`dataset`属性访问
### 插入标记
- `innerHTML`：读模式下，返回调用元素的所有子节点的HTML标记；写模式下，根据指定值创建新的DOM树，替换子节点
	
	该属性使用时，会创建一个HTML解析器，多次的创建和销毁解析器会带来性能的损失，所以当需要重复替换元素，比如`<li>`元素，可以单独构建字符串，再统一赋值给innerHTML
- `outerHTML`：读模式下，返回调用元素及所有子节点的HTML；写模式下，根据指定值完全替换调用者
- `insertAdjacentHTML()`：接受两个参数：插入的位置和要插入的HTML文本
	
	位置参数包括下列
	- `beforebegin`：作为前一个同辈元素插入
	- `afterbegin`：作为第一个子元素插入
	- `beforeend`：作为最后一个子元素插入
	- `afterend`：作为后一个同辈元素插入

> 假设某个元素绑定了一个事件处理器，当该元素从DOM树中移除，则它绑定的事件处理程序并没有一并删除，因此使用插入标记时最好手动删除要替换的元素所有的事件处理程序，否则可能带来内存泄漏问题

### scrollIntoView()方法

## 专有扩展
待补，或许已经被写进新标准










<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">图片</div>
</center>