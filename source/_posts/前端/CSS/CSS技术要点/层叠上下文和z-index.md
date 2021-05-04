---
title: 层叠上下文和z-index
cover: false
categories:
  - 前端
  - CSS
  - CSS技术要点
tags:
  - 层叠上下文
  - z-index
abbrlink: ce0b45bf
date: 2020-09-26 13:21:57
updated:
---

## 层叠上下文三大类型
产生创建层叠上下文主要有三种情况：
- HTML中的根元素`<html></html>`本身就具有层叠上下文，称为“根层叠上下文”。
- 普通元素设置`position`属性为非`static`值并设置`z-index`属性为具体数值，产生层叠上下文。
- CSS3中的新属性也可以产生层叠上下文。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/%E5%88%9B%E5%BB%BA%E5%B1%82%E5%8F%A0%E4%B8%8A%E4%B8%8B%E6%96%87.png" />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">创建层叠上下文的三大类型</div>
</center>

## z-index
z-index 属性是用来调整元素及子元素在 z 轴上的顺序。
{% note default %}
默认值为 auto，可以设置正整数，也可以设置为负整数
{% endnote %}
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/z-index.jpg" />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">z-index的7阶水平</div>
</center>

### 注意
关于`z-index`，认识下面两点比较重要：
- z-index属性值仅在定位元素（定义了position属性，且属性值为非- static值的元素）上有效果。
- 堆叠顺序实际由元素的层叠上下文、层叠等级共同决定，而不仅仅是`z-index`值

当元素层叠水平相当时，遵循两个原则：
- 后来居上原则
- 谁 z-index 大，谁在上的准则
