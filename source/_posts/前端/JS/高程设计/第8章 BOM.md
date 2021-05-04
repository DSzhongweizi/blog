---
title: 第8章 BOM
cover: false
categories:
  - 前端
  - JS
  - 高程设计
tags:
  - BOM
abbrlink: eb0eaaf2
date: 2020-07-26 00:26:33
---
BOM提供了很多对象，用于访问浏览器的功能，这些功能与任何网页内容无关，W3C已经把BOM的主要方面纳入HTML5的规范当中。

## window对象
window对象是浏览器的一个实例，是BOM的核心。在浏览器中，window对象既是访问浏览器窗口的一个接口，也是ES规定的Global对象。
### 全局作用域
所有的全局作用域中声明的变量、函数都会变成window对象的属性和方法。
```js
var age = 20;
function sayHi() {
	alert("Hi");
}

alert(window.age);
alert(age);

sayHi();
window.sayHi();
```
尽管如此，定义全局变量和直接在window上定义属性还是不同的——全局变量不能通过delete操作符删除，而直接在window对象上的定义属性可以。
> 主要是因为`var`关键字声明的全局变量，默认的特性`[[Configurable]]`值为false

另外，尝试访问未声明的变量会抛出错误，但是通过查询window对象的访问不会报错。
> 属性访问（报错）和属性查询（未定义）不一样

### 窗口关系和框架
如果页面中包含框架，则每个框架都有自己的window对象，保存在frames集合中，每个window都有一个name属性，包含框架的名称。

frames在新的HTML5中已经抛弃，这里不过多研究。
### 窗口位置
用来确定和修改window对象位置的属性和方法有很多，每个浏览器还不一样:
- `screenLeft`和`screenTop` 支持IE、Safari、Opera和Chrome
- `screenX`和`screenY` 支持Firefox、Safari、Opera和Chrome
	> Opera慎用，这两个属性和`screenLeft`和`screenRight`并不对应

兼容代码
```js
var leftPos = (typeof window.screenLeft == "number") ? window.screenLeft : window.screenX;
var leftPos = (typeof window.screenTop== "number") ? window.screenTop : window.screenY;
```


### 窗口大小
和窗口大小有关的属性，主要有以下几种：
- `outerWidth`和`outerHeight` 
	- 在IE、Safari和Firefox中表示浏览器窗口本身的尺寸
	- 在Opera中表示页面视图容器的尺寸
	- 在Chrome中表示视口的尺寸
- `innerWidth`和`innerHeight`
	- 在IE、Safari、Firefox和Opera中表示页面视图区的大小
	- 在Chrome中表示视口的尺寸，同`outerWidth`和`outerHeight`
- `clientWidth`和`clientHeight`
	- 在IE、Safari、Firefox、Opera和Chrome中，`document.documentElement.clientWidth`和`document.documentElement.clientHeight`可以取到页面视口的信息。
	- 不过以上两种属性在IE6中的标准模式下有效，混杂模式需要`document.body.clientWidth`和`document.body.clientHeight`能取到
	- 在Chrome的混杂模式中，以上四种属性都能取到

我们最终是无法确定浏览器窗口本身的大小，但却可以取得到页面视口的大小
```js
var pageWidth = window.innerWidth, pageHeight = window.innerHeight;
if(typeof pageWidth != "number") {
	if(document.compatMode == "CSS1Compat") {
		pageWidth = document.documentElement.clientWidth;
		pageHeight= document.documentElement.clientHeight;
	} else {
		pageWidth = document.body.clientWidth;
		pageHeight= document.body.clientHeight;
	}
}
```
#### 移动设备
对于移动设备，`window.innerWidth`和`window.innerHeight`保存着可见视口，也就是屏幕上可见区域的大小，移动IE浏览器不支持这些属性，但通过`document.documentElement.clientWidth`和`document.documentElement.clientWidth`提供了相同的信息，随着页面的缩放，这些值也会相应变化。

在其他移动浏览器中，`document.documentElement`度量的是布局视口，即渲染后页面的实际大小（比可见视口大），移动IE把布局视口的信息保存在`document.body`中，这些值不会随着页面缩放而改变。

总之，移动设备和桌面浏览器是由差异的，最好先检测一下用户是否在使用移动设备，再决定使用哪个属性。

#### 调整浏览器窗口大小
浏览器的窗口大小是可以调整的
- `resizeTo()`方法接受两个参数：浏览器的新宽度和新高度
- `resizeBy()`方法接受两个参数：浏览器需要增加/减少的新宽度和新高度

这两种方法可能会被浏览器禁用，同样不适用于框架，只对最外层的window对象使用。

### 导航
使用`window.open()`方法可以导航到一个特定的URL，也可以打开一个新的浏览器窗口，该方法共四个参数：
- 要加载的URL，通常指传递这个参数
- 窗口目标，已有窗口或框架的名字，或者`_selef`、`_parent`、`_top`和`_blank`中的一种
	> 名字不存在则根据第三个参数位置上传入的字符串创建新窗口
- 特性字符串
	> 名值对以等号表示，各个名值对以逗号分割
	<center>
	    <img style="border-radius: 0.3125em;
	    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
	    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/2020-07-26_184855.jpg" width=600 />
	    <br>
	    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
	    display: inline-block;
	    color: #999;">特性字符串</div>
	</center>

- 新页面是否取代浏览器历史记录中当前加载页面的布尔值，在不打开新窗口的情况下使用

`window.open()`方法会返回一个指向新窗口的引用，我们可以通过这个引用来对其进行更多的控制，比如调整大小、移动位置等。

该方法还可以模拟弹出窗口。
### 超时调用和间歇调用
- setTimeout()方法，接受两个参数：要执行的代码和以毫秒表示的时间
	> 第一个参数可以是字符串，也可以函数（建议），不过字符串会影响性能。</br>
	> 第二个参数表示的等待毫秒数，但经过该时间后代码也不一定会立马执行，需要看任务队列还有没有其他任务(JS是单线程的)

该方法会返回一个数值ID，作为该超时调用的唯一标识符，可以将这个ID作为`clearTimeout()`方法的参数，用来取消超时调用。

间歇调用有关的方法是`setInterval()`和`clearInterval()`方法，也会返回一个标识该间歇调用的ID，该ID比超时调用更有控制意义。

不过一般会用超时调用来模拟间歇调用，因为直接使用`setInterval()`，后一个间歇调用可能在前一个间歇调用之前启动。

### 系统对话框
系统对话框与浏览器中显示的网页没有关系，也不包含HTML，外观由浏览器设置或操作系统决定，不由CSS决定，弹出的对话框都是同步的，会阻塞代码的继续执行。
- `alert()` 警告弹出框，包含一个确定的按钮
- `confirm()` 操作对话框，包含一个确定和取消的按钮
- `prompt()` 提示弹出框，两个参数：提示文本和文本输入域的默认值(可以省略)

除上述三种对话框之外，Chrome，IE9，Firefox4还引入一个新特性——对于用户的同一次操作生成的多个对话框，从第二个对话框开始，都会显示一个是否阻止后续对话框的复选框。

还有两个可以通过JS异步打开的对话框，即查找`find()`和打印`print()`。

## location对象
location对象是最有用的BOM对象之一，它提供了与当前窗口加载的文档有关的信息，还提供了一些导航功能。
> 该对象既是window的属性，也是document的属性，`window.location == document.location`

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/2020-07-26_202549.jpg" width=600 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">location对象的所有属性</div>
</center>

### 查询字符串参数
`location.search`返回从问号到URL末尾的所有内容，但没办法解析出每个查询字符串参数，我们可以创建一个像下面解析查询字符串的参数。
```js
function getQueryStringArgs() {
	//取得查询字符串并去掉开头的问号
	let qs = location.search.length > 0 ? location.search.substring(1) : "";
	//保存数据的对象
	let args = {};
	//取得每一项
	let items = qs.length ? qs.split("&") : [];
	let item = null, name = null, value = null, len = items.length;
	for(let i = 0;i < len;i++) {
		item = items[i].split("=");
		name = decodeURIComponent(item[0]);
		value = decodeURIComponent(item[1]);
		if(name.length) {
			args[name] = value;
		}
	}
	return args;
}
```
### 位置操作
使用location对象可以通过很多方式来改变浏览器的位置，例如：
- `location.assign(http://www.baidu.com)`
- `window.locatin = "http://www.baidu.com"`
- `location.href = "http://www.baidu.com"`

以上三种方式是一样的效果，其中`location.href`方式最常用

修改location对象的其他属性也可以改变当前加载的页，初始`URL = "http://www.baidu.com/ds/"`：
- `location.hash = "#section1"`——`http://www.baidu.com/ds/#section1`
- `location.search = "?q=javascript"`——`http://www.baidu.com/ds/?q=javascript`
- `location.hostname = "www.souhu.com"`——`http://www.souhu.com/ds`
- `location.pathname = "mydir"`——`http://www.baidu.com/mydir/`
- `location.post = 8080`——`http://www.baidu.com:8080/ds`

每次修改location属性，除hash以外页面都会以新URL重新加载，并生成新的浏览记录，用户可以单击返回。
> 禁用单击返回，可以用`location.replace()`方法

- `location.reload()`用来以最有效的方式重载当前页面，传入布尔值true会从服务器重新请求资源。

## navigator对象
navigator对象的属性一栏：

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200726225959.png" width=600 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">navigator对象的属性</div>
</center>

表中的这些属性通常用于检测显示网页的浏览器类型
### 检测插件
检测浏览器中是否安装了特定的插件，对于非IE浏览器，可以使用`plugins`数组来做，该数组包含以下属性：
- `name` 插件的名字
- `description` 插件的描述
- `filename` 插件的文件名
- `length` 插件所处理的MIME类型数量

```js
//检测插件（在IE中无效）
function hasPlugin(name) {
	name = name.toLowerCase();
	for(let i = 0;i < navigator.plugins.length;i++) {
		if(navigator.plugins[i].name.toLowerCase().indexOf(name) > -1) {
			return true;
		}
	}
	return false;
}
//检测Flash 
alert(hasPlugin("Flash"));

//检测QuickTime 
alert(hasPlugin("QuickTime"))
```
检测IE的插件比较麻烦，因为IE不支持Netscape式的插件，唯一方式就是专有的`ActiveXObject`类型，并尝试创建一个特定类型的实例。
```js
function hasIEPlugin(name) {
	try {
		new ActiveXObject(name);
		return true;
	} catch (ex){
		return false;
	}
}

//检测Flash 
alert(hasPlugin("ShockwaveFlash.ShockwaveFlash"));

//检测QuickTime 
alert(hasPlugin("QuickTime.QuickTime"))
```
IE是以COM对象的方式实现插件的，每个插件都有唯一标识符来确定。

### 注册处理程序
Firefox2为navigator对象新增了`registerContentHandler()`和`registerProtocolHandler()`方法，这两个方法可以让一个站点指明它可以处理特定类型的信息，提供给桌面程序一样的在线程序，比如RSS阅读器。

`registerContentHandler()`方法，三个参数：
- 要处理的MIME类型
- 处理该类型的页面URL
- 应用程序名称

`registerProtocolHandler()`方法，同样有三个参数：
- 要处理的协议
- 处理协议的页面URL
- 应用程序名称

## screen对象
这个对象对编程用处不大，基本只用来表明客户端的性能
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200726235519.png" width=600 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">navigator对象的属性</div>
</center>
这些信息经常集中出现在测定客户端能力的站点跟踪工具中，有时候也可能需要用到其中的信息来调整浏览器窗口大小。

## history对象
该对象保存着用户上网的历史记录，也是window对象的一个属性，因此每个浏览器窗口、每个标签页乃至每个框架，都会有自己的history对象与特定的window对象关联。

出与安全隐私的考虑，开发人员无法得知用户浏览过的URL的，不过有几个重要的方法可以实现后退和前进：
- `history.go()` 参数为数字表示前进/后退的页，也可以为字符串，跳转到包含该字符串的第一个位置
- `history.back()` 后退1页
- `history.foward()` 前进1页

除了以上几个方法，还有一个`history.length`的属性，保存着历史记录的数量，该属性的值可以用来确定用户是否一开始就打开了你的页面。
```js
if(history.length == 0) { }
```

history在创建自定义后退和前进按钮时，是必须要用的。