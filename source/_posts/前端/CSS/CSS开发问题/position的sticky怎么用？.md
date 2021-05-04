---
title: position的sticky怎么用？
cover: false
categories:
  - 前端
  - CSS
  - CSS开发问题
tags:
  - position
  - sticky
abbrlink: 540d07bb
date: 2020-10-07 20:55:51
updated:
---

## 基本用法
`position:sticky`大体上是`position:relative`和`position:fixed`的结合体——当元素在屏幕内，表现为`relative`，当要滚出显示器屏幕的时候，表现为`fixed`。

`position:sticky`最基本的表现，特别适合导航的跟随定位效果，比如下面这种：

<iframe 
  src="/custom/iframe/nav-sticky.html" 
  width="100%" 
  height="200" 
  frameborder="1">
</iframe>

当然`position:sticky`的用法不止于此，结合使用合适的HTML结构，它也能做出富有层次的滚动效果：

<iframe 
  src="/custom/iframe/level-sticky.html" 
  width="100%" 
  height="300" 
  frameborder="1">
</iframe>

## 相关概念
### 流盒（flow box）
指的是粘性定位元素最近的可滚动元素（overflow属性值不是visible的元素）的尺寸盒子，如果没有可滚动元素，则表示浏览器视窗盒子。

### 粘性约束矩形
表示的是粘性定位元素的包含块在文档流中呈现的矩形区域和流盒的四个边缘在应用粘性定位元素的left、top、right、bottom属性的偏移计算值后的新矩形的交集。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/sticky-top-theory.png" width=700 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">粘性定位理论示意图</div>
</center>

由于滚动的时候，流盒不变，而粘性定位元素的包含块跟着滚动，因此粘性约束矩形随着滚动的进行是实时变化的。

## 使用注意
`position:sticky`有个非常重要的特性，那就是**sticky元素效果完全受制于父级元素们。**
> 这和`position:fixed`定位有着根本性的不同，fixed元素直抵页面根元素，其他父元素对其left/top定位无法限制。

- 父级元素不能有任何`overflow:visible`以外的overflow设置，否则没有粘滞效果，因为改变了滚动容器（即使没有出现滚动条）。因此，如果你的position:sticky无效，看看是不是某一个祖先元素设置了overflow:hidden，移除之即可。
- 父级元素设置和粘性定位元素等高的固定的height高度值，或者高度计算值和粘性定位元素高度一样，也没有粘滞效果。
- 同一个父容器中的sticky元素，如果定位值相等，则会重叠；如果属于不同父元素，且这些父元素正好紧密相连，则会鸠占鹊巢，挤开原来的元素，形成依次占位的效果。
- sticky定位，不仅可以设置top，基于滚动容器上边缘定位；还可以设置bottom，也就是相对底部粘滞。如果是水平滚动，也可以设置left和right值。

