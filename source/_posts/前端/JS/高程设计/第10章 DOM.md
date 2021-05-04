---
title: 第10章 DOM
cover: false
categories:
  - 前端
  - JS
  - 高程设计
tags:
  - DOM1
abbrlink: de8a6bd3
date: 2020-07-27 09:49:30
---
DOM（文档对象模型）是针对HTML和XML文档的一个API（应用程序编程接口）。它描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。
## 节点层次
DOM可以将HTML或XML文档描绘成一个由多层次节点构成的结构，节点分成不同的类型，每个类型的节点都拥有各自的属性、数据和方法，同时又保持着和其他节点的联系。

文档节点是每个文档的根节点，HTML的文档节点只有一个文档元素`<html>`。
### Node类型
DOM1级定义了一个Node接口，该接口由DOM中的所有节点类型实现，所有的节点类型都继承自Node类型，都拥有一套共享的基本属性和方法。
> 除IE之外，其他的浏览器都能访问这个类型。

每种节点都有一个`nodeType`属性，用于表明节点的类型：
- 元素节点——`ELEMENT_NODE`
- 属性节点——`ATTRIBUTE_NODE`
- 文本节点——`TEXT_NODE`
- `CDATA_SECTION_NODE`
- `ENTITY_REFERENCE_NODE`
- `ENTITY_NODE`
- `PROCESSING_INSTRUCTION_NODE`
- `COMMENT_NODE`
- `DOCUMENT_NODE`
- `DOCUMENT_TYPE_NODE`
- `DOCUMENT_FRAGMENT_NODE`
- `NOTATION_NODE`

#### nodeName和nodeValue属性
使用这两个属性前需要检查一下是不是一个元素`if(someNode.nodeType == 1)`，元素节点的`nodeName`值就是标签名，`nodeValue`值为null，
#### 节点关系
每个节点都有一个`childNodes`属性，其中保存着一个`NodeList`对象，这是一个类数组对象，我们可以通过`[index]`的方式访问它，或者使用`item()`方法迭代它。
> 该对象是动态的，是基于DOM结构动态执行查询的结果，`length`属性值会动态改变。

转成数组：`Array.prototype.slice.call(someNode.childNodes,0)`，兼容IE8以及更早的版本：
```js
function converToArray(nodes) {
	var array = null;
	try {
		array = Array.prototype.slice(nodes, 0); //针对非IE浏览器
	} catch(ex) {
		array = new Array();
		for(let i=0,len=nodes.length;i < len;i++) {	//逐个枚举
			array.push(nodes[i]);
		}
	}
	return array;
}
```
每个节点都还有`parentNode`属性，指向文档树中的父节点，反过来父节点还有`firstNode`和`lastNode`属性，指向`childNodes`节点的第一个和最后一个，`childNodes`节点之间还有`nextSibling`和`previousSibling`属性。

具体关系如下：
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/2020-07-27_110645.jpg" width=500 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">图片</div>
</center>
这些关系节点如果不存在，值就为null。

所有节点都有的最后一个属性是`ownerDocument`，该属性指向表示整个文档的文档节点
#### 操作节点
关系指针都是只读的，所以DOM提供了一些操作节点的方法。
- `appendCild(newNode)` 向`childNodes`的列表结尾添加一个节点，返回添加的节点
	> 如果传入的节点已存在，则将该节点转移到新的位置

- `insertBefore(newNode, referNode)` 向`childNodes`中某个参照的节点前插入新的节点
	> 如果参照节点为null，则行为同`appendCild()`

- `replaceChild(newNode, replacedNode)` 移除节点，替换成新节点
- `removeChild(removedNode)` 移除节点，返回移除的节点
	> 同上，移除的节点仍然在文档中，但它在文档中已经没有了自己的位置。

注意，并不是所有的类型节点都有子节点，这些节点并不支持以上方法。
#### 其他方法
有两个方法是所有节点都有的
- `cloneNode()` 复制节点，传入布尔值true表示深复制，包括该节点及其它的子节点，通常和插入节点的方法配合使用
- `normalize()` 该方法会清除目标节点的子节点中的空文本节点，合并两个相邻的文本节点

### Document类型
JS通过该类型表示文档，document对象是HTMLDocument（继承自Dcument的一个实例）的一个属性（最常见的应用对象），表示整个HTML页面，同时也是window对象的一个属性。

Document节点具有下列特征：
- `nodeType`的值为9
- `nodeName`的值为"#document"
- `nodeValue`的值为null
- `parentNode`的值为null
- `ownerDocument`的值为null
- 其子节点可能是一个`DocumentType`（最多一个）、`Element`（最多一个）、`ProcessingInstruction`或`Comment`


#### 文档的子节点
两个内置的访问其子节点的属性：
- `documentElement` 该属性指向HTML页面的`<html>`元素（更快更便捷）
	> `chidNode[0]`和`firstChild`也能访问到`<html>`元素

- `body` 该属性指向`<body>`元素，开发人员经常要使用这个元素

`document.doctype`属性的浏览器支持不一致，用的不多。
#### 文档信息
作为HTMLDocument的一个实例，document对象还有一些标准的Document对象没有的属性。
- `title` 可以取得文档标题
- `URL` 包含页面完整的URL
- `domain` 包含页面的域名
- `referrer`保存着连接到当前页面的URL，可能为空串

有关网页请求的信息在请求的HTTP的头部可以查到，我们只是通过JS访问到它。它们中只有`domain`属性可以修改，不过**只能将其设置为`URL`本身包含的域**，比如将子域名设置为主域名，但主域名反过来不能设置为子域名。
> 由于跨域安全的限制，`domain`的修改，可以方便子域的页面通过JS通信，只要将`document.domain`都设置成主域名，它们就可以互相访问对方的JS对象

#### 查找元素
取得特定的某个或某族元素的引用，再执行一些操作，是DOM最常见的应用。
- `getElementById()` 接受要取得元素的id并查找，返回该元素，不存在则返回null
- `getElementByTagName()` 接受要取得元素的标签名（传入*返回全部元素），返回一个HTMLCollection对象，其中包含所有该标签的元素，该对象同NodeList一样可以用`[index]`和`item()`方法访问
	> 该对象还有一个叫做`namedItem()`的方法，可以通过标签中的name属性访问到特定的标签集合。

- `getElementByName()` 接受元素的name属性值，返回相应的所有元素的HTMLCollection对象集合，常用于取得单选按钮的情况。

#### 特殊集合
除了属性和方法，document对象还有一些特殊的属性集合，这些集合都是HTMLCollection对象，为访问文档常用的部分提供了快捷方式。
- `anchors` 包含文档中所有带 name 特性的`<a>`元素
- `applets` 包含文档中所有的`<applet>`元素，该标签不再推荐使用
- `forms` 包含文档中所有的`<form>`元素
- `images` 包含文档中所有的`<img>`元素
- `links` 包含文档中所有带href特性的`<a>`元素

#### DOM一致性检测
#### 文档写入
文档写入是将输出流写入到网页中的能力
- `write()` 接受一个字符串参数，向页面写入
	> 可以动态地包含外部资源，比如JS文件

- `writeln()` 同上，只不过写入后会在末尾添加一个换行符`(\n)`

如果在文档加载结束后再调用，那么输出的内容将会重写整个页面。

方法`open()`和`close()`用于打开和关闭输出流，如果在页面加载期间使用`write()`，那不需要用这两个方法

### Element类型
该类型和Document类型一样，是Web编程中最常用的类型之一，主要用于表现XML或HTML元素，提供对元素标签名、子节点及特性的访问，具有以下特征：
- `nodeType`的值为1
- `nodeName`的值为元素的标签名
- `nodeValue`的值为null
- `parentNode`可能为Document或Element
- 其子节点可能为Element、Text、Comment、ProcessingInstruction、CDATASection或EntityReference

要访问元素的标签名，可以使用`nodeName`或`tagName`属性，两个是一样的
> 这两个属性得到的标签名全部大写表示，比较的时候记得统一转换成大/小写。

#### HTML元素
所有的HTML元素都由HTMLElement类型和它的子类型标识，每个元素都存在下列标准特性：
- `id` 元素在文档中的唯一标识
- `title` 元素的附加说明，一般通过工具提示条显示出来
- `lang` 元素内容的语言代码，很少使用
- `dir` 语言的方向，值有"ltr"何"rtl"，也很少使用
- `className` 与元素的class特性对应

#### 操作特性
每个元素都有一个或多个特性，操作特性的方法主要有三个：
- `getAttribute()`  取得元素的特性，传入的参数字符串不区分大小写

	该方法可以访问自定义特性，非自定义特性也可以DOM元素本身的属性取得，效果大部分是一样的，不过有两个特性需要注意一下：
	
	- `style`特性，`div.getAttribute("style")`返回的是字符串文本，而`div.style`返回的是对象
	- `onclick`特性，`div.getAttribute("onclick")`返回的只是字符串，而`div.onclick`返回的是一个函数

	因此，开发人员如果能使用属性直接访问的，就不会用`getAttribute()`

- `setAttribute()` 该方法接受两个参数，要设置的特性名和值，如果特性名已存在，则用指定的值替换原来的值，特性名大小写随意，最终都能转成小写
- `removeAttribute()` 用于彻底删除元素的所有特性，在序列化，需要重新指定元素的特性时很有用

#### attributes属性
> 这个属性只有Element类型拥有，该属性用的不多，大部分功能可以由操作特性相关方法替代

attributes属性中包含一个NamedNodeMap，与NodeList类似，是一个动态集合，该对象有下列方法：
- `getNamedItem(name)` 返回元素nameNode属性等于name的节点
- `removeNamedItem(name)` 从列表中移除nameNode属性等于name的节点
- `setNamedItem(node)` 向列表中添加节点，以节点的nodeName属性为索引
- `item(pos)` 返回位于数字pos位置处的节点

...
#### 创建元素
使用`document.createElement()`方法可以创建新元素，接受一个要创建的标签名的参数，标签名在HTML中不区分大小写。

返回的元素引用可以给他设置特性，不过有时候直接传参包含特性的标签字符串也是可以的。
#### 元素的子节点
元素可以有任意数目的子节点和后代节点，属性`childNodes`包含所有的子节点，如果需要通过某个特定的标签名取得子节点或后代节点，可以用`getElementsByTagName()`方法，该方法除了搜索起点是当前元素以外，其他的和`document.getElementsByTagName()`是一样的。

### Text类型
文本节点由Text类型表示，`document.createTextNode()`可以创建新文本节点，节点的内容会经过HTML/XML的编码，该类型包含的是可以照字面解释的纯文本内容（包含转义后的HTML字符，但不能包含HTML代码），Text节点具有以下特征：
- `nodeType`的值为3
- `nodeName`的值为"#text"
- `nodeValue`的值为节点所包含的文本，可以通过此节点修改文本内容
- `parentNode`是一个Element
- 没有子节点

可以通过`nodeValue`属性或`data`属性访问Text节点包含的文本，二者是一样的。

操作节点中的文本
- `appendData(text)` 将text添加到节点的末尾
- `deleteData(offset, count)` 从offset 指定的位置开始，删除count个字符
- `insertData(offset, text)` 在offset指定的位置插入text
- `replaceData(offset, count, text)` 用text替换从offset指定的位置开始，共count个字符
- `normalize()` 父元素调用，合并多个相邻的子文本节点
- `splitText(offset)` 从offset指定的位置，将当前文本节点分成两个文本节点
- `substringData(offset, count)` 提取从offset位置开始，共count个字符

除了以上方法，本节点还有一个`length`属性，保存着节点中字符的数目，`nodevalue.length`和`data.length`也保存着同样的值

默认情况下，每个可以包含内容的元素最多只能有一个文本节点
（空格也算），而且确实有内容存在。

### Comment类型
注释在DOM中通过Comment类型来表示，可以通过`document.createComment()`创建，它具有以下特征：
- nodeType的值为8；
- nodeNaame的值为"#comment"
- nodeValue的值是注释的内容
- parentNode可能是Document或Element
- 没有子节点

Comment类型与Text类型继承自相同的基类，因此它拥有除`splitText(offset)`方法之外的所有字符串操作
### CDATASection类型
CDATASection类型只针对基于XML的文档，表示的是CDATA区域，可以通过`document.createCDataSection()`来创建，具有以下特征：
- nodeType的值为4；
- nodeNaame的值为"#cdata-section"
- nodeValue的值是CDATA区域的内容
- parentNode可能是Document或Element
- 没有子节点

与Comment类型类似，继承自Text类型，因此它拥有除`splitText(offset)`方法之外的所有字符串操作
### DocumentType类型
该类型在Web浏览器中并不常用，少部分浏览器支持
### DocumentFragment
该节点类型是文档中唯一没有对应标记的，使用`document.createDocumentFragment()`方法创建，作为一种“轻量级”的文档，可以包含和控制节点，但不会像完整的文档那样占用额外的资源，它具有以下特征：
- nodeType的值为11；
- nodeNaame的值为"#document-fragment"
- nodeValue的值为null
- parentNode的值为null
- 子节点可以是Element、ProcessingInstruction、Comment、Text、CDATASection或EntityReference

文档片段对于需要一次性进行大量DOM操作，比如添加大量`<li>`元素，可以作为代理，减少浏览器在渲染上耗费的性能。
### Attr类型
元素的特性在DOM中以Attr类型表示，使用`document.createAttribute()`方法创建新特性，技术上讲，特性存在于元素的`attributes`属性的节点，该节点具有下列特征：
- nodeType的值为11；
- nodeNaame的值为特性的名称
- nodeValue的值为特性的值
- parentNode的值为null
- 在HTML中没有子节点

尽管它们也是节点，但特性却不被认为是DOM树的一部分，开发人员最常使用的是`getAttribute()`、`setAttribute()`和`removeAttribute()`方法，很少直接引用特性节点，也不推荐使用。

Attr对象有三个属性：
- `name` 同`nodeName`
- `value` 同`nodeValue`
- `specified` 布尔值，用以区别特性是代码中指定的，还是默认的

## DOM操作技术
### 动态脚本
src版
```js
 function loadScript(url) {
	 var script = document.createElement("script");
	 script.type = "text/javascript";
	 script.src = url;
	 document.body.appendChild(script);
 }
```
code版
```js
function loadScriptString(code) {
	var script = document.createElement("script");
	script.type = "text/javascript";
	try {
		script.appendChild(document.createTextNode(code));
	} catch(ex) {
		script.text = code;	//兼容IE
	}
	document.body.appendChild(script)
}
```
### 动态样式
href版
```js
function loadStyle(url) {
	var link = document.createElement("link");
	link.rel = "stylesheet";
	link.type = "text/css";
	link.href = url;
	document.getElementsByTagName("head")[0].appendChild(link);
}
```
code版
```js
function loadStyleString(css) {
	var style = document.createElement("style");
	style.type = "text/css";
	try {
		style.appendChild(document.createTextNode(css));
	} catch(ex) {
		style.stylesheet.cssText = css; //兼容IE
	} 
	document.getElementsByTagName("head")[0].appendChild(style);
}
```

