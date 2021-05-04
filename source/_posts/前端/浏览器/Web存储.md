---
title: cookie和session、sessionStorage和localStorage
cover: false
categories:
  - 前端
  - 浏览器
tags:
  - manifest
  - cookie
  - session
  - sessionStorage
  - localStorage
abbrlink: 9dc24832
date: 2020-08-23 00:10:49
---
## HTML5的离线存储（主要针对Web App）
> 在用户没有与因特网连接时，可以正常访问站点或应用；在用户与因特网连接时，更新用户机器上的缓存文件。

基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来；之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。
### 使用
页面头部像下面一样加入一个manifest的属性；
```html
<!DOCTYPE HTML>
<html manifest = "cache.manifest">
...
</html>
```
在cache.manifest文件的编写离线存储的资源
	
	CACHE MANIFEST
	#v0.11
	
	CACHE: #表示离线访问的资源
	js/app.js
	css/style.css
	
	NETWORK: #表示不需要离线访问的资源
	resourse/logo.png
	
	FALLBACK: #表示资访问失败的替代资源
	//offline.html

### 管理/加载离线资源
在线的情况下，浏览器发现html头部有manifest属性，它会请求manifest文件。
- 如果是第一次访问app，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。
- 如果已经访问过app并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后对比新旧manifest文件；
- 如果文件改变，重新下载文件中的资源并进行离线存储。

离线的情况下，浏览器就直接使用离线存储的资源。

## cookie和session
同Web离线存储（sessionStorage和localStorage）比较，cookie大小只有4k，很多浏览器限制一个站点最多保存20个cookie，默认浏览器关闭后销毁，遵循同源策略。

### 区别和关系：
- cookie安全性低，可以从本地获取进行cookie欺骗，考虑到安全性，应该使用session；
- cookie保存在客户端，session保存在服务端的数据库或文件中，当访问量增多，考虑到减轻服务器性能压力，应当使用cookie；
- session依赖于session ID，而cookie是实现传递session ID的一种方式，当浏览器禁止cookie，session ID可以携带在url中。

### 应用场景
- cookie
	- 第一次访问之后直接存储，第二次请求携带到服务器识别用户
	- 通过sessionID，判断用户是否登录

## sessionStorage和localStorage
### 共同点：
- 存储大小均为5M左右
- 都有同源策略限制
- 仅在客户端中保存，不参与和服务器的通信

### 不同点：
- 生命周期
	- sessionStorage在页面关闭后随之销毁
	- localStorage一直存在，除非手动删除

- 作用域
	- sessionStorage在同源的基础上，还要同一个窗口
	- localStorage在同源文档之间都共享

### 应用场景
- sessionStorage：表单页面拆分成多个子页面，然后按步骤引导用户填写
- localStorage：保存用户在电商网站的购物车信息

