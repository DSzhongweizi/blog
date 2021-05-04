---
title: 定位position
cover: false
categories:
  - 前端
  - CSS
  - CSS技术要点
tags:
  - position
  - 定位
abbrlink: 6d5c7366
date: 2020-09-26 13:08:21
updated:
---
## 五大定位
### static
默认属性，相对定位和绝对定位常用的属性top、bottom、left、right或者 z-index 声明在position为static的情况下无效

在改变了元素的position属性后可以将元素重置为static让其回归到页面默认的普通流中

### relative
相对定位，相对元素默认位置的定位，普通流中依然保持着原有的默认位置，并没有脱离普通流，只是视觉上发生的偏移

position: relative不能改变行内元素的display属性，即便设置了，行内元素的width属性仍然无效（absolute可以改变哦）

### absolute
绝对定位，相对设置了relative或absolute的父元素的进行定位，如果父元素没有设置，则相对body进行位置偏移

块状元素在position(relative/static)的情况下width为100%，但是设置了position: absolute之后，会将width变成auto（会受到父元素的宽度影响）

### fixed

绝对定位，fixed与absolute最大的区别在于：absolute的根元素是可以被设置的，而fixed则其根元素固定为浏览器窗口。

和absolute的共同点：
	• 会改变行内元素的呈现模式，使display之变更为block。
	• 会让元素脱离普通流，不占据空间。
	• 默认会覆盖到非定位元素上。

### sticky
粘性定位，可以被认为是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。

须指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

## 其他
### position定位 top百分比的问题
top，bottom（left，right），设置百分比定位是按父元素的高度（宽度）来计算的，而父元素的高度（宽度）有以下两种情况：
- 父元素设置有padding值：包括padding值
- 父元素设置有border值：不包括border值
