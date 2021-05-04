---
title: 常用bom属性和方法
cover: false
categories:
  - 前端
  - 浏览器
tags:
  - 常用bom
abbrlink: 9f6eea35
date: 2020-08-22 22:33:38
---
### window对象
- `window.screenX`、`window.innerWidth`、`window.outerWidth`、`window.clientWidth`...窗口位置和窗口大小
	<center>
		<img style="border-radius: 0.3125em;
		box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
		src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200822225633.png" width=700 />
		<br>
		<div style="color:orange; border-bottom: 1px solid #d9d9d9;
		display: inline-block;
		color: #999;">元素的大小和位置</div>
	</center>

- `window.open(目标url,打开方式，特性，是否生成新的历史记录)`
- `setTimeout()`、`setInterval()` 超时调用/间歇调用
- `alert()`、`confirm()`、`prompt()` 警告/确认/提示对话框
### location对象
- `location.href` 返回或设置当前页面url
- `location.search` 返回url中包括?后面的所有查询字符串
- `location.hash` 返回#后面的内容，没有返回空
- `location.host` 返回url中的域名
- `location.hostname` 返回url中的主域名
- `location.reload` 重载当前页面

### history对象
- `history.go()` 前进后退页面数
- `history.foward()` 前进一页
- `history.back()` 后退一页
