---
title: webworker
cover: false
categories:
  - 前端
  - JS
  - JS进阶
tags:
  - worker
  - sharedWorker
  - 单线程
  - 多线程
abbrlink: d8995760
date: 2020-09-26 10:35:07
updated:
---
## 概述
web worker是用来为JS创造多线程环境，充分利用发挥电脑多核CPU的计算能力。
- 创造的多线程是基于JS的主线程，计算任务也是主线程分配的。
- 等到 Worker 线程完成计算任务，再把结果返回给主线程
- Worker 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断，造成资源的浪费，不宜过多使用，任务完成后应该及时关闭。

好处：一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢

{% note warning %}
注意，web worker非完全独立线程，有诸多限制：
- 同源限制：运行的js文件必须和主线程同源
- dom限制：无法读取主线程所在网页的 DOM 对象，也无法使用document、window、parent这些对象，不过可以使用navigator对象和location对象
- 通信限制：与主线程不在同一个上下文环境，不能直接通信，必须通过消息完成
- 脚本限制：不能执行`alert()`方法和`confirm()`方法，但可以使用 XMLHttpRequest 对象发出 AJAX 请求
- 文件限制：无法读取本地文件，加载的资源只能来自网络

{% endnote %}

## worker
主线程
```js
var worker = new Worker('worker.js'); // 新建一个 Worker 线程，第二个参数是配置信息，比如Worker名字，用于多个Worker的情况

worker.postMessage('Hello World');  // 向 子线程Worker 发消息
worker.postMessage({ method: 'echo', args: ['Work'] });

// 指定监听函数，接收子线程发回来的消息
worker.onmessage = function (event) {
    console.log('Received message ' + event.data);// data属性可以获取 Worker 发来的数据
    doSomething();
}
// 监听错误
worker.onerror(function (event) {
  console.log([
    'ERROR: Line ', e.lineno, ' in ', e.filename, ': ', e.message
  ].join(''));
});
// 回调函数
function doSomething() {
    // 执行任务
    worker.postMessage('Work done!');
}
// 内部也可以监听错误
worker.addEventListener('error', function (event) {
  // ...
});
worker.terminate(); // 关闭Worker
```
Worker 子线程
```js
// self可以省略
self.addEventListener('message', function (e) {
    var data = e.data;  // data属性包含主线程发来的数据
    // 根据主线程发来的数据，Worker 线程可以调用不同的方法
    switch (data.cmd) {
        case 'start':
            self.postMessage('WORKER STARTED: ' + data.msg);  //传回给主线程
            break;
        case 'stop':
            self.postMessage('WORKER STOPPED: ' + data.msg);  //传回给主线程
            self.close(); // 内部关闭自身
            break;
        default:
            self.postMessage('Unknown command: ' + data.msg);  //传回给主线程
    };
}, false);
```

## 其他
- `importScripts('script1.js')`：用来在 Worker 内部如果要加载其他脚本，可以加载多个
- 主线程和子线程的传参是值的传参，文本对象都行
- 通常情况下，Worker 载入的是一个单独的 JS 脚本文件，但是也可以载入与主线程在同一个网页的代码

参考
  - [JavaScript 的多线程，Worker 和 SharedWorker](https://www.zhuwenlong.com/blog/article/590ea64fe55f0f385f9a12e5)
