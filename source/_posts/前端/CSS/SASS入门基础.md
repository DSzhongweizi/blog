---
title: SASS基础
categories:
  - 前端
  - CSS
tags:
  - sass
cover: false
abbrlink: 5e7c45ed
date: 2020-07-06 08:40:17
---
> 原本的CSS是纯粹的样式描述，没有编程的思维，SASS作为一种CSS预处理器，是一种专门的编程语言，可以进行网页样式设计，然后再编译成正常的CSS文件。

sass的几个特性：变量（variables），代码混合（ mixins），嵌套（nested rules）以及 代码模块化(Modules)，用的比较多。
## 定义
- 一个 CSS 预处理器。
- CSS 扩展语言，可以帮助我们减少 CSS 重复的代码，节省开发时间。
- 完全兼容所有版本的 CSS。
- 扩展了 CSS3，增加了规则、变量、混入、选择器、继承、内置函数等等特性。
- 生成良好格式化的 CSS 代码，易于组织和维护。
- 文件后缀为 .scss。

## 安装和使用
[安装](https://www.runoob.com/sass/sass-install.html)
SASS文件就是普通的文本文件，里面可以直接使用CSS语法
### 相关命令
显示
```
sass test.scss //显示转换后的css
```
保存
```
sass test.scss test.css //保存转换后的css
```
编译风格
>* nested：嵌套缩进的css代码，它是默认值。
>* expanded：没有缩进的、扩展的css代码。
>* compact：简洁格式的css代码。
>* compressed：压缩后的css代码。

```
sass --style compressed test.sass test.css  //生产环境中的一般选择compressed
```
监听
> 可以让SASS监听某个文件或目录，一旦源文件有变动，就自动生成编译后的版本
```
sass --watch input.scss:output.css // 监听一个文件
sass --watch app/sass:public/stylesheets // 监听一个目录
```

## 基本用法
### 变量
SASS允许使用变量，可以重复使用，所有变量以$开头，类型可以是：
- 字符串
- 数字
- 颜色值
- 布尔值
- 列表
- null 值

如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中。
```css
$myFont: Helvetica, sans-serif;
$myColor: red;
$myFontSize: 18px;
$myWidth: 680px;

body {
  font-family: $myFont;
  font-size: $myFontSize;
  color: $myColor;
}

#container {
  width: $myWidth;
}
```

### 作用域
Sass 变量的作用域只能在当前的层级上有效果，如下所示 h1 的样式为它内部定义的 green，p 标签则是为 red。
```css
$myColor: red;

h1 {
  $myColor: green;   // 只在 h1 里头有用，局部作用域
  color: $myColor;
}

p {
  color: $myColor;
}
```
使用 `!global` 关键词来设置变量是全局的
{% note info %}
所有的全局变量我们一般定义在同一个文件，如：_globals.scss，然后我们使用 @include 来包含该文件
{% endnote %}

### 嵌套规则
#### 选择器嵌套
比如嵌套一个导航栏的样式：
```css
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li {
    display: inline-block;
  }
}
```

#### 属性嵌套
很多 CSS 属性都有同样的前缀，例如：font-family, font-size 和 font-weight
```css
font: {
  family: Helvetica, sans-serif;
  size: 18px;
  weight: bold;
}
```
## 指令
### @import
类似 CSS，Sass 支持 @import 指令，可以让我们导入其他文件等内容。
{% note info %}
CSS @import 指令在每次调用时，都会创建一个额外的 HTTP 请求，但 Sass @import 指令将文件包含在 CSS 中，不需要额外的 HTTP 请求。
{% endnote %}
{% note warning %}
包含文件时不需要指定文件后缀，Sass 会自动添加后缀 .scss。
{% endnote %}

### @mixin 与 @include
- @mixin 指令允许我们定义一个可以在整个样式表中重复使用的样式。
- @include 指令可以将混入（mixin）引入到文档中。

1. 定义一个混入
```css
@mixin important-text { /*Sass 的连接符号 - 与下划线符号 _ 是相同的*/
  color: red;
  font-size: 25px;
  font-weight: bold;
  border: 1px solid blue;
}
```
2. 使用混入
```css
selector {
  @include mixin-name;
}
```
3. 混入里可以包含混入
```css
@mixin special-text {
  @include important-text;
  @include link;
  @include special-border;
}
```
4. 向混入传递变量
```css
/* 混入接收两个参数，color默认值blue*/
@mixin bordered($color: blue, $width) {
  border: $width solid $color;
}
.myArticle {
  @include bordered(green, 1px); /* 调用混入，并传递两个参数 */
}
.myNotes {
  @include bordered(red, 2px); /* 调用混入，并传递两个参数 */
}
```
5. 可以使用 ... 来设置可变参数
```css
@mixin box-shadow($shadows...) {  /* 浏览器使用混入是很方便的 */
  -moz-box-shadow: $shadows;
  -webkit-box-shadow: $shadows;
  box-shadow: $shadows;
}
.shadows {
  @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
}
```

### @extend
@extend 指令告诉 Sass 一个选择器的样式从另一选择器继承，体现代码的复用。
```css
.button-basic  {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button-report  {
  @extend .button-basic;
  background-color: red;
}
```
等同于
```css
.button-basic, .button-report, .button-submit {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}

.button-report  {
  background-color: red;
}
```
好像区别不大？实际上，使用 @extend 后，我们在 HTML 按钮标签中就不需要指定多个类 class="button-basic button-report" ，只需要设置 class="button-report" 类就好了。

## 函数
...待补