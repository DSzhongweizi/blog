---
title: HTML基础
categories:
  - 前端
  - HTML
tags:
  - html
cover: false
abbrlink: e173abca
date: 2020-05-13 09:54:18
---
# HTML标签
## html
`<html>` 标签告知浏览器这是一个 HTML 文档。
### 属性
manifest，HTML5新增属性，Web应用缓存，定义一个 （绝对/相对）URL，在这个 URL 上描述了文档的缓存信息。
> manifest 文件需要配置正确的 MIME-type，即 "text/cache-manifest"，必须在 web 服务器上进行配置。
> 浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。
```html
<html manifest="demo.appcache">
...
</html>
```

**manifest 文件可分为三个部分：**
- CACHE MANIFEST - 在此标题下列出的文件将在首次下载后进行缓存
- NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
- FALLBACK - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）

```
CACHE MANIFEST
# 2012-02-21 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.php

FALLBACK:
/html/ /offline.html
```

**更新缓存**
> 一旦应用被缓存，它就会保持缓存，即使服务器文件已经被更新。

	直到发生下列情况：
	- 用户清空浏览器缓存
	- manifest 文件被修改（参阅下面的提示）
	- 由程序来更新应用缓存
## head
`<head>` 元素是所有头部元素的容器，必须包含文档的标题（title），可以包含脚本、样式、meta 信息 以及其他更多的信息。
- `<title>`：定义浏览器工具栏中的标题，提供页面被添加到收藏夹时的标题，显示在搜索引擎结果中的页面标题
  - 在头部中，这个元素是必需的
- `<style>`
	- `media` 为样式表规定不同的媒体类型。
	- `scoped` New，如果使用该属性，则样式仅仅应用到 style 元素的父元素及其子元素。
	- `type`	规定样式表的 MIME 类型，比如text/css
- `<base>`：规定页面上所有链接的默认 URL 和默认目标
  - 请把标签排在`<head>`元素中第一个元素的位置，这样`<head>`区域中其他元素就可以使用`<base>`元素中的信息了。
  - 如果使用了 <base> 标签，则必须具备 href 属性或者 target 属性或者两个属性都具备。
- `<link>`：标签定义文档与外部资源的关系，最常见的用途是链接样式表。
	- `href` 定义被链接文档的位置。
	- `rel` 定义当前文档与被链接文档之间的关系。
	- `type` 规定被链接文档的 MIME 类型。
	- `sizes` 定义了链接属性大小，只对属性 rel="icon" 起作用。
- `<meta>`：提供了 HTML 文档的元数据，通常用于指定网页的描述（如何显示内容或重新加载页面），搜索引擎（关键词），文件的最后修改时间，作者及其他元数据
	- `charset` New，定义文档的字符编码。
	- `content` 定义与`http-equiv`或`name`属性相关的元信息。
	- `http-equiv`把`content`属性关联到 HTTP 头部。
		- content-type
		- default-style
		- refresh	定义刷新页面
	- `name` 把 `content` 属性关联到一个名称。
		- application-name
		- author	定义页面作者
		- description	定义web页面描述
		- generator
		- keywords	定义文档关键词，用于搜索引擎
- `<script>`：用于定义客户端脚本，比如 JavaScript。
> 既可包含脚本语句，也可以通过 "src" 属性指向外部脚本文件。如果使用 "src" 属性，则 `<script>` 元素必须是空的，默认在浏览器继续解析页面之前，立即读取并执行脚本。
	
  - `async` New，规定脚本相对于页面的其余部分异步地执行（当页面继续进行解析时，脚本将被执行，仅适用于外部脚本）。
	- `defer`	规定当页面已完成解析时，执行脚本（仅适用于外部脚本）。
	- `charset` 规定在脚本中使用的字符编码（仅适用于外部脚本）。
	- `src` 规定外部脚本的 URL。
	- `type` 规定脚本的 MIME 类型。
- `<noscript>`：用来定义在脚本未被执行时的替代内容（文本），此标签可被用于可识别 `<noscript>`标签但无法支持其中的脚本的浏览器。

## body
标签定义文档的主体，包含文档的所有内容（比如文本、超链接、图像、表格和列表等等）。
> 属性在HTML5中全部废除

## 标题
通过`<h1> - <h6>`标签进行定义的。`<h1>`定义最大的标题。`<h6>`定义最小的标题
> 搜索引擎使用标题为网页的结构和内容编制索引，用户也可以通过标题来快速浏览网页，所以用标题来呈现文档结构是很重要的。

## 段落
段落是通过`<p>`标签定义的。
> 在不产生一个新段落的情况下进行换行（新行），请使用`<br>`标签，HTML 代码中的所有连续的空行（换行）也被显示为一个空格。

## 文本格式化
- `<b> <strong>` 定义粗体文本
- `<i> <em>` 定义着重文字
- `<small>` 定义小号字
- `<sup>` `<sub>` 定义上下标字
- `<ins> <del>` 定义插入和删除字
	> `cite` 规定一个解释了文本被插入/删除的原因的文档的 URL。
	> 
	> `datetime`	YYYY-MM-DDThh:mm:ssTZD格式，规定文本被插入/删除的日期和时间。

### 计算机输出
- `<code>`	定义计算机代码
- `<kbd>`	定义键盘码
- `<samp>`	定义计算机代码样本
- `<var>`	定义变量
- `<pre>`	定义预格式文本
	> 格式化的文本通常会保留空格和换行

### 引文, 引用, 及标签定义
- `<abbr>`	定义缩写
	> 通过对缩写词语进行标记，您就能够为浏览器、拼写检查程序、翻译系统以及搜索引擎分度器提供有用的信息
- `<address>`	定义地址
	> 定义文档作者/所有者的联系信息，文本通常呈现为斜体
- `<bdo>`	定义文字方向
	> dir 规定 <bdo> 元素内的文本方向,值ltr rtl	。
- `<blockquote>`	定义长的引用
	> 定义摘自另一个源的块引用,浏览器通常会对 `<blockquote>` 元素进行缩进。
- `<q>`	定义短的引用语
	> 定义一个短的引用，浏览器经常会在这种引用的周围插入引号。
- `<cite>`	定义引用、引证
	> 定义作品（比如书籍、歌曲、电影、电视节目、绘画、雕塑等等）的标题
- `<dfn>`	定义一个定义项目
	> 是一个短语标签，用来定义一个定义项目

## 链接
设置超文本链接，可以是一个字，一个词，或者一组词，也可以是一幅图像，您可以点击这些内容来跳转到新的文档或者当前文档中的某个部分。
```html
<a href="url">链接文本</a> 
```
- `target` 定义被链接的文档在何处显示
- `id` 可用于创建在一个HTML文档书签标记
	- 在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：
		> `<a id="tips">有用的提示部分</a>` </br>
		> `<a href="#tips">访问有用的提示部分</a>`
	- 或者，从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：
		> `<a href="https://www.xxx.com/html-links.html#tips">访问有用的提示部分</a>`

{% note warning %}
请始终将正斜杠添加到url的子文件夹，否则浏览器会向服务器产生两次 HTTP 请求
{% endnote %}

## 图像
`<img>` 是空标签，只包含属性，并且没有闭合标签
> 加载图片是需要大量时间的，慎用
- `src` 图像的 URL 地址
- `alt` 为图像定义一串预备的可替换的文本
- `width`、`height` 用于设置图像的宽高

### 图像映射`<map>`
用于客户端图像映射，图像图像映射指带有可点击一幅图像的区域
> `<img>`中的 usemap 属性可引用 <map> 中的 id 或 name 属性（取决于浏览器），所以我们应同时向 `<map>` 添加 id 和 name 属性。</br>

### 图像映射`<area>`
`<area>` 元素永远嵌套在 map 元素内部，可定义图像映射中的区域。
- `alt` 规定区域的替代文本。如果使用 href 属性，则该属性是必需的。
- `coords` 规定区域的坐标。
- `href` 规定区域的目标URL。
- `shape` 规定区域的形状。
	- default、rect、circle、poly	

```html
<img src="planets.gif" width="145" height="126" alt="Planets" usemap="#planetmap">
<map name="planetmap">
  <area shape="rect" coords="0,0,82,126" href="sun.htm" alt="Sun">
  <area shape="circle" coords="90,58,3" href="mercur.htm" alt="Mercury">
  <area shape="circle" coords="124,58,8" href="venus.htm" alt="Venus">
</map>
```

## 表格
表格由 `<table>` 标签来定义，`<tr>` 元素定义表格行，`<th>` 元素定义表头，`<td>` 元素定义表格单元。
> 字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。
```html
<table border="1">
    <tr>
        <th>Header 1</th>  # 表头
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```
`<th>和<td>`
- `colspan` 规定表头单元格可横跨的列数。
- `rowspan` 规定表头单元格可横跨的行数。

`<thead>、<tfoot> 和 <tbody> `
>用来规定表格的各个部分（表头、主体、页脚），默认不会影响表格的布局。</br>
通过使用这些元素，使浏览器有能力支持独立于表格表头和表格页脚的表格主体滚动。</br>
当包含多个页面的长的表格被打印时，表格的表头和页脚可被打印在包含表格数据的每张页面上。

## 列表
无序列表`<ul>`、有序列表`<ol>`和列表项`<li>`</br>
自定义列表`<dl>`标签开始，每个自定义列表项以`<dt>`开始，列表项的定义以`<dd>`开始。

## 表单
参考[HTML 表单和输入](https://www.runoob.com/html/html-forms.html)
- `<form>`	定义供用户输入的表单
- `<input>`	定义输入域
- `<textarea>`	定义文本域 (一个多行的输入控件)
- `<label>`	定义了 `<input>` 元素的标签，一般为输入标题
- `<fieldset>`	定义了一组相关的表单元素，并使用外框包含起来
- `<legend>`	定义了 `<fieldset>` 元素的标题
- `<select>`	定义了下拉选项列表
- `<optgroup>`	定义选项组
- `<option>`	定义下拉列表中的选项
- `<button>`	定义一个点击按钮
- `<datalist>` New指定一个预先定义的输入控件选项列表
- `<keygen>` New 定义了表单的密钥对生成器字段
- `<output>` New 定义一个计算结果

## 框架
`<iframe> `规定一个内联框架，一个内联框架被用来在当前 HTML 文档中嵌入另一个文档。
> 通过使用框架，你可以在同一个浏览器窗口中显示不止一个页面。
- `witdh`、`height`	规定 `<iframe>` 的宽高。
- `name`	规定 `<iframe>` 的名称。
- `src`	规定在 `<iframe>` 中显示的文档的 URL。

## 实体（Entities）
- &lt; 等同于 <
- &gt; 等同于 >
- &#169; 等同于 ©

# HTML5标签
## 新标签
HTML5定了8个新的HTML语义（semantic）元素，所有这些元素都是块级元素。
>为了能让旧版本的浏览器正确显示这些元素，你可以设置 CSS 的 display 属性值为 block
```css
header, section, footer, aside, nav, main, article, figure {
    display: block; 
}
```
html5shiv主要解决HTML5提出的新的元素不被IE6-8识别，这些新元素不能作为父节点包裹子元素，并且不能应用CSS样式。

## 画布
`<canvas>`标签定义图形，比如图表和其他图像，该标签基于 JavaScript 的绘图 API
- width、height 规定画布的宽高

```js
<canvas id="myCanvas"></canvas>
 
<script type="text/javascript">
var canvas=document.getElementById('myCanvas');
var ctx=canvas.getContext('2d');
ctx.fillStyle='#FF0000';
ctx.fillRect(0,0,80,100);
</script>
```

## 内联 SVG
参考 [HTML5 SVG](https://www.runoob.com/html/html5-svg.html)

## MathML
MathML 是数学标记语言，是一种基于XML（标准通用标记语言的子集）的标准，用来在互联网上书写数学符号和公式的置标语言，对应的标签是 `<math>...</math`

## 音频和视频
`<audio>` 标签定义声音，比如音乐或其他音频流，目前支持的3种文件格式：MP3、Wav、Ogg。
- autoplay New 如果出现该属性，则音频在就绪后马上播放。
- controls New 如果出现该属性，则向用户显示音频控件（比如播放/暂停按钮）。
- loop New 如果出现该属性，则每当音频结束时重新开始播放。
- muted New 如果出现该属性，则音频输出为静音。
- preload New 规定当网页加载时，音频是否默认被加载以及如何被加载。
	- auto
	- metadata
	- none	
- src New 规定音频文件的 URL。

`<video>` 标签定义视频，比如电影片段或其他视频流目前，支持三种视频格式：MP4、WebM、Ogg。
- 除了拥有`<audio>`有的，还多了视频播放窗口的width和height属性值

### 资源和轨道
`<source>` 标签为媒体元素定义媒体资源。</br>
`<track>` 标签为媒体元素规定外部文本轨道（字幕和歌词）。
- 
default New 规定该轨道是默认的，如果用户没有选择任何轨道，则使用默认轨道。
- kind New 规定文本轨道的文本类型。
	- captions
	- chapters
	- descriptions
	- metadata
	- subtitles	
- label New 规定文本轨道的标签和标题。
- src New 必需的，规定轨道文件的 URL。
- srclang New 规定轨道文本数据的语言，如果 kind 属性值是 "subtitles"，则该属性是必需的。
```html
<video width="320" height="240" controls>
	<source src="forrest_gump.mp4" type="video/mp4">
	<source src="forrest_gump.ogg" type="video/ogg">
	<track src="subtitles_en.vtt" kind="subtitles" srclang="en"
	label="English">
	<track src="subtitles_no.vtt" kind="subtitles" srclang="no"
	label="Norwegian">
</video>
```

## 插件
`<embed>` 标签定义了一个容器，用来嵌入外部应用或者互动程序（插件）。
- width、height New 规定嵌入内容的宽高。
- src New 规定被嵌入内容的 URL。
- type New 规定嵌入内容的 MIME 类型。
>注：MIME = Multipurpose Internet Mail Extensions。

## 拖放
在HTML5中，拖放是标准的一部分，也是一种常见的特性，即抓取对象以后拖到另一个位置，任何元素都能够拖放。

参考[拖放（Drag 和 Drop）](https://www.runoob.com/html/html5-draganddrop.html)

## 地理定位、
Geolocation API 用于获得用户的地理位置，鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

参考[Geolocation（地理定位）](https://www.runoob.com/html/html5-geolocation.html)

## 新表单元素
`<datalist>` 定义选项列表，需要和`<input>`元素配合使用，来定义 `<input>`可能的值。
```html
<input list="browsers">
<datalist id="browsers">
  <option value="Internet Explorer">
  <option value="Firefox">
  <option value="Chrome">
  <option value="Opera">
  <option value="Safari">
</datalist>
```
`<output>` 定义不同类型的输出，比如脚本的输出。
- for New element_id 描述计算中使用的元素与计算结果之间的关系。
- form New form_id 定义输入字段所属的一个或多个表单。
- name New 定义对象的唯一名称（表单提交时使用）。
```html
<form oninput="x.value=parseInt(a.value)+parseInt(b.value)">0
	<input type="range" id="a" value="50">100
	+<input type="number" id="b" value="50">
	=<output name="x" for="a b"></output>
</form>
```
参考[新的 Input 类型](https://www.runoob.com/html/html5-form-input-types.html)、[表单属性](https://www.runoob.com/html/html5-form-attributes.html)

## 新的语义和结构元素
许多现有网站都包含以下HTML代码： `<div id="nav">`, `<div class="header">`, 或者 `<div id="footer">`, 来指明导航链接, 头部, 以及尾部。HTML5 提供了新的语义元素来明确一个Web页面的不同部分:
- `<nav>`标签定义导航链接的部分，并不是所有的HTML文档都要使用到 `<nav>`元素，它只是作为标注一个导航链接的区域。
- `<header>` 标签定义文档或者文档的一部分区域的页眉，可以作为介绍内容或者导航链接栏的容器。 
- `<footer> `标签定义文档或者文档的一部分区域的页脚，该元素会包含文档创作者的姓名、文档的版权信息、使用条款的链接、联系信息等等。
- `<article>` 标签定义独立的内容，内容本身必须是有意义的且必须是独立于文档的其余部分，比如论坛帖子、博客文章、新闻故事、评论。
- `<figure>` 标签规定独立的流内容（图像、图表、照片、代码等等），这些内容应该与主内容相关，同时元素的位置相对于主内容是独立的，如果被删除，则不应对文档流产生影响。
	- `<figcaption>` 标签为 `<figure>` 元素定义标题，应该被置于 `<figure>` 元素的第一个或最后一个子元素的位置。
- `<progress>` 标签定义运行中的任务进度（进程）。
	- max New number 规定需要完成的值。
	- value New number 规定进程的当前值。
- `<section>` 标签定义了文档的某个区域，比如章节、头部、底部或者文档的其他区域。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://www.runoob.com/wp-content/uploads/2013/07/html5-layout.jpg" width=300 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">新的语义和结构元素</div>
</center>

参考[语义元素](https://www.runoob.com/html/html5-semantic-elements.html)

## Web存储
web 存储,一个比cookie更好的本地存储方式，以键值对存储，更加的安全与快速，数据不会被保存在服务器上，只用于用户请求网站数据上,并且可以大量存储而不影响网站的性能。

### 检查是否支持Web存储
```js
if(typeof(Storage)!=="undefined")
{
    // 是的! 支持 localStorage  sessionStorage 对象!
    // 一些代码.....
} else {
    // 抱歉! 不支持 web 存储。
}
```

### localStorage
用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除。
- 保存数据：localStorage.setItem(key,value);
- 读取数据：localStorage.getItem(key);
- 删除单个数据：localStorage.removeItem(key);
- 删除所有数据：localStorage.clear();
- 得到某个索引的key：localStorage.key(index);

### sessionStorage
用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。
- API和localStorage一样

### JSON.stringify和JSON.parse
JSON.stringify 可以将对象转换为字符串，可以用来存储对象数据。

## Web SQL 数据库
Web SQL 数据库 API 并不是 HTML5 规范的一部分，但是它是一个独立的规范，引入了一组使用 SQL 操作客户端数据库的 APIs。

### 三个核心方法：
- openDatabase：这个方法使用现有的数据库或者新建的数据库创建一个数据库对象。
- transaction：这个方法让我们能够控制一个事务，以及基于这种情况执行提交或者回滚。
- executeSql：这个方法用于执行实际的 SQL 查询。
```js
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
/*
* 五个参数说明：
* 数据库名称
* 版本号
* 描述文本
* 数据库大小
* 创建回调（在创建数据库后被调用）
*/
var msg;
// 创建和插入数据
db.transaction(function (tx) {
    tx.executeSql('CREATE TABLE IF NOT EXISTS LOGS (id unique, log)');
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (1, "菜鸟教程")');
    tx.executeSql('INSERT INTO LOGS (id, log) VALUES (2, "www.runoob.com")');
    msg = '<p>数据表已创建，且插入了两条数据。</p>';
    document.querySelector('#status').innerHTML =  msg;
});
// 删除数据
db.transaction(function (tx) {
  tx.executeSql('DELETE FROM LOGS  WHERE id=1');
  msg = '<p>删除 id 为 1 的记录。</p>';
  document.querySelector('#status').innerHTML =  msg;
});
// 更新数据
db.transaction(function (tx) {
 tx.executeSql('UPDATE LOGS SET log=\'www.w3cschool.cc\' WHERE id=2');
  msg = '<p>更新 id 为 2 的记录。</p>';
  document.querySelector('#status').innerHTML =  msg;
});
// 查询和读取数据
db.transaction(function (tx) {
tx.executeSql('SELECT * FROM LOGS', [], function (tx, results) {
    var len = results.rows.length, i;
    msg = "<p>查询记录条数: " + len + "</p>";
    document.querySelector('#status').innerHTML +=  msg;
 
    for (i = 0; i < len; i++){
        msg = "<p><b>" + results.rows.item(i).log + "</b></p>";
        document.querySelector('#status').innerHTML +=  msg;
    }
}, null);
});
```

## Web Workers
web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。
> 您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

```html
<!DOCTYPE html>
<html>
<head> 
<meta charset="utf-8"> 
<title>菜鸟教程(runoob.com)</title> 
</head>
<body>
 
<p>计数： <output id="result"></output></p>
<button onclick="startWorker()">开始工作</button> 
<button onclick="stopWorker()">停止工作</button>
 
<p><strong>注意：</strong> Internet Explorer 9 及更早 IE 版本浏览器不支持 Web Workers.</p>

<script>
var w;
function startWorker() {
	// 检查是否支持web worker
    if(typeof(Worker) !== "undefined") {
        if(typeof(w) == "undefined") {
			// 创建 Web Worker 对象
            w = new Worker("demo_workers.js"); //引用创建好的js脚本
        }
        w.onmessage = function(event) {
            document.getElementById("result").innerHTML = event.data;
        };
    } else {
        document.getElementById("result").innerHTML = "抱歉，你的浏览器不支持 Web Workers...";
    }
}
// 终止 Web Worker对象
function stopWorker() 
{ 
    w.terminate();
    w = undefined;
}
</script>
 
</body>
</html>
```

## 服务器发送事件(Server-Sent Events)
HTML5 服务器发送事件（server-sent event）允许网页获得来自服务器的更新。
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
<h1>获取服务端更新数据</h1>
<div id="result"></div>

<script>
if(typeof(EventSource)!=="undefined"){
	var source=new EventSource("demo_sse.php");
	source.onmessage=function(event){
		document.getElementById("result").innerHTML+=event.data + "<br>";
	};
}
else{
	document.getElementById("result").innerHTML="抱歉，你的浏览器不支持 server-sent 事件...";
}
</script>

</body>
</html>
```

## WebSocket
WebSocket 是 HTML5 开始提供的一种在单个TCP连接上进行全双工通讯的，本质是TCP的协议，允许服务端主动向客户端推送数据。
>在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。相比较Ajax轮询进行推送，WebSocket 协议能更好的节省服务器资源和带宽，并且能够更实时地进行通讯。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://www.runoob.com/wp-content/uploads/2016/03/ws.png" width=300 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">Ajax和WebSocket</div>
</center>
创建 WebSocket 对象。
```js
var Socket = new WebSocket(url, [protocol] );
```

### 属性
- Socket.readyState	
只读属性 readyState 表示连接状态，可以是以下值：
>0 - 表示连接尚未建立。</br>
1 - 表示连接已建立，可以进行通信。</br>
2 - 表示连接正在进行关闭。</br>
3 - 表示连接已经关闭或者连接不能打开。</br>
- Socket.bufferedAmount	
只读属性 bufferedAmount 已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。

### 事件
- open	Socket.onopen	连接建立时触发
- message	Socket.onmessage	客户端接收服务端数据时触发
- error	Socket.onerror	通信发生错误时触发
- close	Socket.onclose	连接关闭时触发

### 方法
- Socket.send() 使用连接发送数据
- Socket.close() 关闭连接
```html
<!DOCTYPE HTML>
<html>
   <head>
   <meta charset="utf-8">
   <title>菜鸟教程(runoob.com)</title>
    
      <script type="text/javascript">
         function WebSocketTest(){
            if ("WebSocket" in window){
               alert("您的浏览器支持 WebSocket!");
               
               // 打开一个 web socket
               var ws = new WebSocket("ws://localhost:9998/echo");
                
               ws.onopen = function(){
                  // Web Socket 已连接上，使用 send() 方法发送数据
                  ws.send("发送数据");
                  alert("数据发送中...");
               };
                
               ws.onmessage = function (evt) { 
                  var received_msg = evt.data;
                  alert("数据已接收...");
               };
                
               ws.onclose = function(){ 
                  // 关闭 websocket
                  alert("连接已关闭..."); 
               };
            }
            
            else{
               // 浏览器不支持 WebSocket
               alert("您的浏览器不支持 WebSocket!");
            }
         }
      </script>
        
   </head>
   <body>
      <div id="sse">
         <a href="javascript:WebSocketTest()">运行 WebSocket</a>
      </div>
   </body>
</html>
```