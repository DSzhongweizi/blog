---
title: CDN原理
cover: false
categories:
  - 前端
  - 性能优化
tags:
  - cdn
abbrlink: ccbef645
date: 2020-10-14 19:21:19
updated:
---

## CDN原理
把各种缓存服务器分布到用户访问相对集中的地区或网络中，当用户访问网站时，利用全局负载技术将用户的访问指向距离最近的工作正常的缓存服务器上，由缓存服务器响应用户请求。
<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/cdn.png" />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">cdn原理</div>
</center>

过程：
- 用户输入URL回车，DNS解析后将域名解析权交给CNAME指向的CDN专用DNS服务器
- 解析后获得全局负载均衡设备的IP地址，用户向全局负载均衡设备发送内容访问请求
- 全局负载均衡设备将实时地根据网络流量和各节点的连接、负载状况以及到用户的距离和响应时间等信息把用户的请求重新导向到离用户最近的服务节点上
- 用户获得所需内容

作用：由于网络带宽小、用户访问量大、网点分布不均等原因，提高用户访问网站的响应速度。