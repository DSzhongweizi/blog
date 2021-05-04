---
title: CSS基础
categories:
  - 前端
  - CSS
tags:
  - css
cover: false
abbrlink: 599b70c0
date: 2020-05-14 16:12:01
---
盒模型是CSS中的重要概念，在布局方面扮演着重要角色。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/%E7%9B%92%E6%A8%A1%E5%9E%8B.png" width=700 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">盒模型</div>
</center>

## CSS
CSS 指层叠样式表 (Cascading Style Sheets)，样式定义如何显示 HTML 元素，通常存储在样式表中。
### 创建
插入样式表的方法有三种:
#### 外部样式表(External style sheet)
当样式需要应用于很多页面时，外部样式表将是理想的选择。
```html
<link rel="stylesheet" type="text/css" href="mystyle.css">
```
#### 内部样式表(Internal style sheet)
当单个文档需要特殊的样式时，就应该使用内部样式表。
```html
<style>
    hr {
        color: sienna;
    }

    p {
        margin-left: 20px;
    }

    body {
        background-image: url("images/back40.gif");
    }
```
#### 内联样式(Inline style)
当样式仅需要在一个元素上应用一次时，适合用内联样式
```html
<p style="color:sienna;margin-left:20px">这是一个段落。</p>
```
### 样式优先级
#### 多重样式优先级
遵循就近原则
> 一般情况下，优先级如下：(内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式

#### 选择器优先级
从高到底：内联样式——>id——>类|属性|伪类样式——>元素|伪元素——>通用选择器（*）
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://www.runoob.com/wp-content/uploads/2017/06/jc6_002_thumb.png" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">权重计算</div>
</center>

### 背景
CSS 背景属性用于定义HTML元素的背景效果:`background {color image repeat attachment position }`

#### 背景颜色
background-color 属性定义了元素的背景颜色
CSS中，颜色值通常以以下方式定义:
- 十六进制 - 如："#ff0000"
- RGB - 如："rgb(255,0,0)"
- 颜色名称 - 如："red"

#### 背景图像
- background-image：属性描述了元素的背景图像。
  > 默认情况下，背景图像进行平铺重复显示，以覆盖整个元素实体。
- background-repeat：显式指明水平或垂直平铺
- background-position：改变图像在背景中的位置
- background-attachment：设置背景图像是否固定或者随着页面的其余部分滚动。
  - scroll：背景图片随着页面的滚动而滚动，这是默认的。
  - fixed：背景图片不会随着页面的滚动而滚动。
  - local：背景图片会随着元素内容的滚动而滚动。
  - initial：设置该属性的默认值。 
  - inherit：指定 background-attachment 的设置应该从父元素继承。 

### 文本
- 文本颜色：color颜色属性被用来设置文字的颜色
- 水平对齐：text-align文本排列属性是用来设置文本的水平对齐方式
  `left | right | center | justify`
- 垂直对齐：vertical-align
  `sub | super | top...`
- 文本方向：direction属性指定文本方向/书写方向
  `ltr（默认）| rtl（从右到左） | inherit`
- 文本修饰：text-decoration属性用来设置或删除文本的装饰，比如删除链接下划线
  `overline（上划线）| line-through（删除线）、underline（下划线）`
- 文本转换：text-transform用于所有文本变成大写或小写字母，或每个单词的首字母大写
  `uppercase（全部大写）| lowercase | capitalize（首字母大写）`
- 文本缩进：text-indent文本缩进属性是用来指定文本的第一行的缩进。
  `数字、%`
- 字符间距：letter-spacing（字符，汉字）和word-spacing（单词） 属性增加或减少字符间的空白（字符间距）
  `normal（默认）| length（定义字符间的固定空间，允许使用负值）`
- 行高：line-height属性
  `em、数字、%`
- 文本阴影：text-shadow
  `text-shadow: h-shadow（水平位置） v-shadow blur（模糊距离） color;`
- 文本换行：white-space属性指定元素内的空白怎样处理
  `pre（保留）| nowrap（不换行） | pre-wrap | pre-line`

### 字体
- 字体 font-family，多个系列用逗号分隔
- 样式 font-style
  `normal | italic（斜体）| oblique（倾斜）`
- 大小 font-size
  `长度值、%`
- 粗细 font-weight
  `normal | bold（粗） | bolder（更粗） | lighter | 数字（100-900，400属于normal，700属于bold）`

### 链接
顺序L-V-H-A
- a:link - 正常，未访问过的链接
- a:visited - 用户已访问过的链接
- a:hover - 当用户鼠标放在链接上时
- a:active - 链接被点击的那一刻

### 列表
`list-style:type position image`
- list-style-type 指定列表项标记的类型
  `none | disc | circle | square | decimal...`
- list-style-position 指定列表项标记的位置
  `outside | inside`
- list-style-image 指定列表项标记的图像

### 表格
- border-collapse：collapse，设置表格的边框是否被折叠成一个单一的边框或隔开

### 边框
`border:width style color //每个属性同margin、pading：上右下左`
- border-style 指定要显示什么样的边界
  `none | dotted | dashed | solid | double | groove（3D沟槽） | ridge（3D脊边） | inset（3D嵌入） | outset（3D突出）`
- border-width 为边框指定宽度
  `长度 | thick | medium（默认值）| thin`
- border-color 设置边框的颜色

### 边距
#### 外边距
margin可以使用负值，重叠的内容
`上 右 下 左 | 上 左右 下 | 上下 左右`

#### 内边距
padding同margin

### 轮廓
`outline:width style color`
轮廓（outline）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用，可以指定元素轮廓的样式、颜色和宽度，但**不占空间的**，既不会增加额外的width或者height。


### 尺寸
用来控制元素的高度和宽度，同样允许增加行间距。
`width | height | max-width | min-width`

### 隐藏
- display:none 元素不再占用空间。
- visibility: hidden 使元素在网页上不可见，但仍占用空间。

### 定位
- static 默认值，遵循正常的文档流对象，不会受到 top, bottom, left, right影响
- fixed 相对于浏览器窗口是固定位置，脱离文档流
- relative 相对其正常位置，经常用来作为绝对定位元素的容器块
- absolute 相对于最近的已定位父元素，脱离文档流
- sticky 基于用户的滚动位置，在relative（阈值内）与fixed定位之间切换

#### 其他属性
- top, bottom, left, right
- clip 剪辑一个绝对定位的元素
- cursor 显示光标移动到指定的类型
  > 参考[光标类型](https://www.runoob.com/cssref/pr-class-cursor.html)
- overflow 设置当指定高度元素的内容溢出其区域时发生的事情
  `visible | hidden | scroll | auto`
- z-index 设置元素的堆叠顺序

### 浮动
- float：指定一个盒子（元素）是否可以浮动
  `none | left | right`
- clear：指定不允许元素周围有浮动元素
  `none | left | right | both`

### 选择器
#### 组合选择器
- 后代选择器(以 分隔)
- 子元素选择器(以>分隔）
- 相邻兄弟选择器（以+分隔）
- 普通兄弟选择器（以~分隔）

#### 属性选择器
参考[属性选择器](https://www.runoob.com/css/css-attribute-selectors.html)
### 伪类和伪元素
- :before 选择器向选定的元素前插入内容
- :after 选择器向选定的元素之后插入内容
- :first-child 伪类来选择父元素的第一个子元素
- :lang 伪类使你有能力为不同的语言定义特殊的规则

### 媒体类型
媒体类型允许你指定文件将如何在不同媒体呈现，@media 规则允许在相同样式表为不同媒体设置不同的样式

## CSS3
CSS3已完全向后兼容，浏览器将永远支持CSS2，同时CSS3被拆分为了"模块"
### 边框
- border-radius：圆角边框
  `border-radius：` `左上角 右上角 右下角 左下角` | `左上角 右上角左下角 右下角` | `左上角右下角 右上角左下角`
- box-shadow：盒阴影 前两个参数必需
  `box-shadow：h-shadow(水平阴影)  v-shadow  blur(模糊距离)  spread(阴影大小)  color  inset`
- border-image：图片边框
  `border-image: source(图片资源)  slice(向内偏移)  width  outset  repeat | stretch(拉伸) | round(铺满) | initial | inherit;`

### 背景
- background-image：添加背景图片，可以添加多张，不同的背景图像和图像用逗号隔开，所有的图片中显示在最顶端的为第一张
- background-size：指定背景图片的大小
  `长度、百分比`
- background-origin：规定背景图片的定位区域
  `padding-box(默认) | border-box | content-box`
- background-clip：规定背景的绘制区域

### 渐变
> 颜色可以使用带有透明度的rgba(255,0,0,0)，还可以加%控制占比

#### 线性渐变 
创建一个线性渐变，必须至少定义两种（可以多种）颜色结点。
```css
background-image: linear-gradient(#e66465, #9198e5); //从上到下（默认）
background-image: linear-gradient(to right, red , yellow);  //从左到右
background-image: linear-gradient(to bottom right, red, yellow); //对角
```
##### 使用角度
```css
background-image: linear-gradient(-90deg, red, yellow);
```
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://www.runoob.com/wp-content/uploads/2014/07/7B0CC41A-86DC-4E1B-8A69-A410E6764B91.jpg" width=400 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">角度标准</div>
</center>

#### 径向渐变
径向渐变由它的中心定义，为了创建一个径向渐变，你也必须至少定义两种颜色结点。
```css
background-image: radial-gradient(shape size at position, start-color, ..., last-color);
```
### 文本效果
- text-shadow：指定文本水平阴影，垂直阴影，模糊的距离，以及阴影的颜色：
  `text-shadow: h-shadow v-shadow blur color;`
- text-overflow：指定应向用户如何显示溢出内容
  `clip(修剪文本)、ellipsis(省略号)、string(给定字符串)`
- word-wrap：规定非中日韩文本的换行规则
  `word-break: normal | break-all | keep-all;`
- word-break
  `word-wrap: normal | break-word;`

### 字体
> 使用以前 CSS 的版本，网页设计师不得不使用用户计算机上已经安装的字体。使用 CSS3，网页设计师可以使用他/她喜欢的任何字体。

在新的 @font-face 规则中，您必须首先定义字体的名称（比如 myFirstFont），然后指向该字体文件。
```css
<style> 
@font-face
{
    font-family: myFirstFont;
    src: url(sansation_light.woff);
}
 
div
{
    font-family:myFirstFont;
}
</style>
```
### 2D/3D转换
CSS3 转换可以对元素进行移动、缩放、转动、拉长或拉伸，转换的效果是让某个元素改变形状，大小和位置。
#### transform
应用于元素的2D或3D转换，这个属性允许你将元素旋转，缩放，移动，倾斜等。
> transform: none | transform-functions; //可以添加多个效果

- translate()：根据左(X轴)和顶部(Y轴)位置给定的参数，从当前元素位置移动。
  `translate(x,y) | translate3d(x,y,z) | translateX(x) | translateY(y) | translateZ(z)`
- rotate()：在一个给定度数顺时针旋转的元素，负值是允许的，这样是元素逆时针旋转。
  `rotate(angle)、rotate3d(x,y,z,angle)、rotateX(angle)、rotateY(angle)、rotateZ(angle)`
- skew() ：两个参数值，分别表示X轴和Y轴倾斜的角度
  `skew(x-angle,y-angle) | skewX(angle) | skewY(angle)`
- matrix() ：有六个参数，包含旋转，缩放，移动（平移）和倾斜功能
  `matrix(n,n,n,n,n,n)、matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)	`
- scale() ：该元素增加或减少的大小，取决于宽度（X轴）和高度（Y轴）的参数
  `scale(x,y) | scaleX(n) | scaleY(n)`

#### transform-origin 
允许你改变被转换元素的位置：`transform-origin: x-axis y-axis z-axis;`
- x-axis：定义视图被置于 X 轴的何处，可能的值：`left | center | right | length | %`
- y-axis：定义视图被置于 Y 轴的何处，可能的值：`top | center | bottom | length | %`
- z-axis：定义视图被置于 Z 轴的何处，可能的值：`length`

#### transform-style 
规定被嵌套元素如何在 3D 空间中显示：`transform-style: flat | preserve-3d;`
- flat：表示所有子元素在2D平面呈现。
- preserve-3d：表示所有子元素在3D空间中呈现。

#### perspective
规定3D元素的透视效果：`perspective: number | none;`
- number：元素距离视图的距离，以像素计。
- none：默认值，与 0 相同，不设置透视。

#### perspective-origin	
规定 3D 元素的底部位置：`perspective-origin: x-axis y-axis;`
- x-axis：定义该视图在 x 轴上的位置，默认值：50%，可能的值：`left | center | right | length | %`
- y-axis：定义该视图在 y 轴上的位置，默认值：50%，可能的值：`top | center | bottom | length | %`

#### backface-visibility	
定义元素在不面对屏幕时是否可见：`backface-visibility: visible | hidden;`
- visible 背面是可见的。
- hidden 背面是不可见的。

### 过渡
CSS3中，我们为了添加某种效果可以从一种样式转变到另一个的时候，无需使用Flash动画或JavaScript

CSS3 过渡是元素从一种样式逐渐改变为另一种的效果，要实现这一点，必须规定两项内容：
- 指定要添加效果的CSS属性
- 指定效果的持续时间
  > 如果未指定的期限，transition将没有任何效果，因为默认值是0。

#### 过渡属性
`transition：property duration timing-function delay`
- transition-property：规定应用过渡的 CSS 属性的名称。
- transition-duration：定义过渡效果花费的时间，默认是 0。
- transition-timing-function：规定过渡效果的时间曲线，默认是 "ease"。
- transition-delay：规定过渡效果何时开始，默认是 0。

### 动画
`animation：name duration timing-function delay iteration-count direction fill-mode;`
> CSS3 可以创建动画，它可以取代许多网页动画图像、Flash 动画和 JavaScript 实现的效果。

@keyframes 规则用来创建动画，指定一个 CSS 样式和动画将逐步从目前的样式更改为新的样式。

当在 @keyframes 创建动画，把它绑定到一个选择器，否则动画不会有任何效果，至少指定两个CSS3的动画属性绑定在一个选择器：
- 规定动画的名称
- 规定动画的时长

### 多列
属性描述
- column-rule	所有 column-rule-* 属性的简写
	- column-rule-width	指定两列间边框的厚度
	- column-rule-style	指定两列间边框的样式
	- column-rule-color	指定两列间边框的颜色
- columns	设置 column-width 和 column-count 的简写
	- column-width	指定列的宽度
	- column-count	指定元素应该被分割的列数。
- column-span	指定元素要跨越多少列
- column-fill	指定如何填充列
- column-gap	指定列与列之间的间隙

### 用户界面
> 在 CSS3 中, 增加了一些新的用户界面特性来调整元素尺寸，框尺寸和外边框。

- resize：指定一个元素是否应该由用户去调整大小
  `resize：none | both | horizontal | vertical`
- box-sizing：指定盒模型标准
  `box-sizing：content-box | border-box | inherit`
- outline-offset：对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓
  `outline-offset: length | inherit;`
  轮廓与边框有两点不同：
	- 轮廓不占用空间
	- 轮廓可能是非矩形


### 弹性盒子
CSS3 弹性盒（ Flexible Box 或 flexbox），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。
- flex-direction：属性指定了弹性子元素在父容器中的位置。
`flex-direction: row | row-reverse | column | column-reverse`
- justify-content：内容对齐属性应用在弹性容器上，把弹性项沿着弹性容器的主轴线（main axis）对齐。
`justify-content: flex-start | flex-end | center | space-between | space-around`
- align-items：设置或检索弹性盒子元素在侧轴（纵轴）方向上的对齐方式。
`align-items: flex-start | flex-end | center | baseline | stretch（默认值）`
- flex-wrap：属性用于指定弹性盒子的子元素换行方式。
`flex-wrap: nowrap|wrap|wrap-reverse|initial|inherit;`
- align-content：属性用于修改 flex-wrap 属性的行为，类似于 align-items, 但它不是设置弹性子元素的对齐，而是设置各个行的对齐。
`align-content: flex-start | flex-end | center | space-between | space-around | stretch`
- flex：属性用于指定弹性子元素分配空间的比例。
`flex: flex-grow flex-shrink flex-basis | auto | initial | inherit;`





