---
title: CSS选择器
cover: false
categories:
  - 前端
  - CSS
  - CSS技术要点
tags:
  - 选择器
  - 基础选择器
  - 组合选择器
  - 选择器优先级
  - 伪类
  - 伪元素
abbrlink: b6981f1b
date: 2020-09-26 12:25:47
updated:
---

## 基础选择器
### 优先级
内联样式 > ID 选择器 > 类选择器 = 属性选择器 = 伪类选择器 > 标签（元素）选择器 = 伪元素选择器

### 五大选择器
- ID选择器：理论上效率最高	
- 类选择器：效率也很快，多用
- 标签选择器：通常用来重置某些标签的样式
- 通配选择器：选择页面里所有元素，不推荐用
- 属性选择器	
	1. 属性选择器要做文本的匹配，所以效率也不会高，`a. [attr]`用来选择带有 attr 属性的元素
	2. 在使用属性选择器时，尽量要给它设置上生效的范围，如果只用了个 [href] 相当于要在所有元素里找带 href 的元素，效率会很低。如果用 a [href] 会好的多，如果用 .link [href] 就更好了
	3. 属性选择器很灵活，如果能使用 CSS 代替 JS 解决一些需求，可以不用太纠结性能的问题，用 JS 实现也一样要耗费资源的。	
	
ID、类和标签选择器用起来比较简单，伪类和伪元素[请看这里](https://dengsong.icu/article/7c517d42.html)，这里注重说一下属性选择器：		
1. 下面的CSS会使所有带href的a标签字体变红色：
```css
a[href]{
    color: red;
}
```

2. [attr=xxx]，用来选择有 attr 属性且属性值等于 xxx 的元素
```css
/*<input type="text" value="大花碗里扣个大花活蛤蟆"/>*/
input[type=text]{
    color: red;
}
```

3. [attr~=xxx]，选择属性值中包含 xxx 的元素
```css
/*<input class="input text" type="text" value="大花碗里扣个大花活蛤蟆"/>*/
input[class~=input]{
    color: red;
}
```
4. [attr|=xxx]，选择属性值为 xxx 或 xxx- 开头的元素（1、2生效）
```css
/* 
<div class="article">我是article</div>
<div class="article-title">我是article-title</div>
<div class="article_footer">我是article_footer，不是以article-开头的</div>
*/
div[class|=article]{
    color: red;
}
```
5. [attr^=xxx]，匹配以 xxx 开头的元素（1、2、3生效）
```css
/*
<div class="article">我是article</div>
<div class="article-title">我是article-title</div>
<div class="article_footer">我是article_footer，是以artical开头的</div>
*/
div[class^=article]{
    color: red;
}
```
6. [attr$=xxx]，选择属性值以 xxx 结尾的元素（1、3生效）
```css
/*
<button class="btn btn-disabled">禁用的按钮</button>
<select class="select select-disabled city-select"></select>
<input class="input input-disabled" value="禁用的输入框"/>
*/
[class$=disabled]{
    display: none;
}
```
7. [attr*=xxx]，选择属性值中包含 xxx 字符的所有元素（1、2、3生效）
```css
/*
<button class="btn btn-disabled">禁用的按钮</button>
<select class="select select-disabled city-select"></select>
<input class="input input-disabled" value="禁用的输入框"/>
*/
[class*=disabled]{
    display: none;
}
```

## 组合选择器
- 并选择器：多个元素使用同一个样式，语法div,p
- 交选择器：一个元素使用多个样式，语法divp
- 后代选择器：选择嵌套在标签内部任意层级的标签元素，语法div p
- 子选择器：选择当前标签往内一层的元素，即子选择器只能匹配直接后代，语法div>p
- 相邻兄弟选择器：选择指定元素后的相邻兄弟元素，且二者有相同父元素，语法div+p
- 后续兄弟选择器：选择指定元素后的所有兄弟元素，且二者有相同父元素，语法div~p

{% note info %}
组合选择器本身并没有优先级之分，它的优先级基于基础选择器优先级累加，多个相同基础选择器优先级累积。
{% endnote %}

