---
title: Node.js基础
categories:
  - 前端
  - JS
  - Node
tags:
  - node
  - 服务端语言
cover: 'https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/6kfgXyIjA.jpeg'
abbrlink: 829b7139
date: 2020-07-12 20:57:25
---

## 简介
Node.js是基于JavaScript语言和V8引擎的开源Web服务器项目，运行在服务端，具有事件驱动，非阻塞I/O等优良特性，执行速度非常快，性能非常好。

参考：[Node.js 中文文档](http://nodejs.cn/api/)

## REPL（交互解释器）
>这是一个运行在电脑环境node.js的交互式解释器，类似 Window 系统的终端或 Unix/Linux shell，我们可以在终端中输入命令，并接收系统的响应。

### REPL可以执行以下任务：
	- 读取读取用户输入，解析输入了Javascript 数据结构并存储在内存中。
	- 执行输入的数据结构
	- 打印输出结果
	- 循环循环操作
	以上步骤直到用户两次按下 ctrl-c 按钮退出。
#### 简单命令
 	- 启动：node -v
 	- 简单的表达式运算：一般的四则运算，或者加括号的优先级表达式运算
 	- 使用变量：使用 var 关键字声明将数据存储在变量中，并在你需要的时候使用它，如果没有使用 var 关键字变量会直接打印出来。
 	- 多行表达式：自动检测是否为连续表达式，通过生成…，回车换行即可
 	- 下划线(_)变量：使用下划线(_)获取上一个表达式的运算结果
#### 基础命令
	- ctrl + c 退出当前终端。
	- ctrl + c 按下两次 - 退出 Node REPL。
	- ctrl + d  退出 Node REPL.
	- 向上/向下 键  查看输入的历史命令
	- tab 键  列出当前命令
	- help  列出使用命令
	- break  退出多行表达式
	- clear  退出多行表达式
	- save filename  保存当前的 Node REPL 会话到指定文件
	- load filename  载入当前 Node REPL 会话的文件内容。

## 回调函数
Node.js 异步编程的直接体现就是回调，回调函数在完成任务后就会被调用，异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。
```js
// 同步代码	
var fs = require("fs");
var data = fs.readFileSync('input.txt');
console.log(data.toString());
console.log("程序执行结束!");//后执行
```
```js
// 异步代码
var fs = require("fs");
fs.readFile('input.txt', function (err, data) {
        if (err) return console.error(err);
        console.log(data.toString());
});
console.log("程序执行结束!");//先执行
```

## 事件循环
> Node.js 是单进程单线程应用程序，但是因为** V8 引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发**，所以性能非常高。基本上所有的事件机制都是用观察者模式实现，这样的单线程就类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数。

### 事件驱动（异步）I/O
> 当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户，web server 一直接受请求而不等待任何读写操作，过程非常高效。
在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);display:inline;margin:0" 
    src="https://cdn.jsdelivr.net/gh/DSzhongweizi/Resources/article/20200507213342.png" width=300 />
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;">事件循环</div>
</center>

#### EventEmitter
> Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列，所有这些产生事件的对象都是 events.EventEmitter 的实例。
events 模块只提供了一个对象： events.EventEmitter，**EventEmitter 的核心就是事件触发与事件监听器功能的封装。**
```js
	// 实例化EventEmitter 
	var eventEmitter = new events.EventEmitter()
	// 注册/触发事件函数，可以携带多个参数
	emit(event, [arg1], [arg2], [...])	
	eventEmitter.emit('data_received',arg1,arg2...);
	// 监听/绑定事件函数，同一个事件可以有多个监听器
	on(event, listener)
	eventEmitter.on('data_received', function(){
	      console.log('数据接收成功。');
		});
	// 单次监听器，即监听器最多只会触发一次，触发后立刻解除该监听器。
	once(event, listener)
	// 添加一个监听器到指定事件监听器数组的尾部；
	addListener(event, listener)
	// 移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。
	removeListener(event, listener)	
	// 其他函数
	removeAllListeners([event])
	setMaxListeners(n)
	listeners(event)
	listenerCount(emitter, event)
```

#### 完整实例
```js
    var events = require('events');
    var eventEmitter = new events.EventEmitter();
    // 监听器 #1
    var listener1 = function listener1() {
          console.log('监听器 listener1 执行。');
    }
    // 监听器 #2
    var listener2 = function listener2() {
        console.log('监听器 listener2 执行。');
    }
    // 绑定 connection 事件，处理函数为 listener1 
    eventEmitter.addListener('connection', listener1);
    // 绑定 connection 事件，处理函数为 listener2
    eventEmitter.on('connection', listener2);
    var eventListeners = eventEmitter.listenerCount('connection');
    console.log(eventListeners + " 个监听器监听连接事件。");
    // 处理 connection 事件 
    eventEmitter.emit('connection');
    // 移除监绑定的 listener1 函数
    eventEmitter.removeListener('connection', listener1);
    console.log("listener1 不再受监听。");
    // 触发连接事件
    eventEmitter.emit('connection');
    eventListeners = eventEmitter.listenerCount('connection');
    console.log(eventListeners + " 个监听器监听连接事件。");
    console.log
```
**大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它,包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。**

## Buffer(缓冲区)
>JS语言自身只有字符串数据类型，没有二进制数据类型，但在处理像TCP流或文件流时，必须使用到二进制数据。
因此在 Node.js中，定义了一个 Buffer 类，该类用来创建一个专门存放二进制数据的缓存区。

### Buffer 与字符编码	
	Buffer 实例一般用于表示编码字符的序列，通过使用显式的字符编码，就可以在 Buffer 实例与普通的 JavaScript 字符串之间进行相互转换。
```js
    const buf = Buffer.from('runoob', 'ascii');
    // 输出 72756e6f6f62
    console.log(buf.toString('hex'));
    // 输出 cnVub29i
    console.log(buf.toString('base64'));
```

### 创建 Buffer 类	
	Buffer.alloc(size[, fill[, encoding]])： 返回一个指定大小的 Buffer 实例，如果没有设置 fill，则默认填满 0
	Buffer.allocUnsafe(size)： 返回一个指定大小的 Buffer 实例，但是它不会被初始化，所以它可能包含敏感的数据
	Buffer.allocUnsafeSlow(size)
	Buffer.from(array)： 返回一个被 array 的值初始化的新的 Buffer 实例（传入的 array 的元素只能是数字，不然就会自动被 0 覆盖）
	Buffer.from(arrayBuffer[, byteOffset[, length]])： 返回一个新建的与给定的 ArrayBuffer 共享同一内存的 Buffer。
	Buffer.from(buffer)： 复制传入的 Buffer 实例的数据，并返回一个新的 Buffer 实例
	Buffer.from(string[, encoding])： 返回一个被 string 的值初始化的新的 Buffer 实例
### 写入缓冲区	
	buf.write(string[, offset[, length]][, encoding])
	- string - 写入缓冲区的字符串。
	- offset - 缓冲区开始写入的索引值，默认为 0 。
	- length - 写入的字节数，默认为 buffer.length
	- encoding - 使用的编码。默认为 'utf8' 。
返回实际写入的大小，如果 buffer 空间不足， 则只会写入部分字符串，只部分解码的字符不会被写入。

### 从缓冲区读取数据	
	buf.toString([encoding[, start[, end]]])
	- encoding - 使用的编码。默认为 'utf8' 。
	- start - 指定开始读取的索引位置，默认为 0。
	- end - 结束位置，默认为缓冲区的末尾。
	返回指定编码的字符串

### 将 Buffer 转换为 JSON 对象	
	buf.toJSON()// 当字符串化一个 Buffer 实例时，JSON.stringify() 会隐式地调用 toJSON()。
### 缓冲区合并	
	Buffer.concat(list[, totalLength])
	- list - 用于合并的 Buffer 对象数组列表。
	- totalLength - 指定合并后Buffer对象的总长度。
参考：[方法参考手册](https://www.runoob.com/nodejs/nodejs-buffer.html)

## Stream(流)
> Stream 是一个抽象接口，Node 中有很多对象实现了这个接口。例如，对http 服务器发起请求的request 对象就是一个 Stream，还有stdout（标准输出）,所有的 Stream 对象都是 EventEmitter 的实例。
### 常用的事件：
	- data - 当有数据可读时触发。    
	- end - 没有更多的数据可读时触发。        
	- error - 在接收和写入过程中发生错误时触发。        
	- finish - 所有数据已被写入到底层系统触发。
### 常用的流操作
#### 从流中读取数据	
```js
创建 main.js 文件, 代码如下：	 
var fs = require("fs");
var data = '';
// 创建可读流
var readerStream = fs.createReadStream('input.txt');
// 设置编码为 utf8。
readerStream.setEncoding('UTF8');
// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
      data += chunk;
});
readerStream.on('end',function(){
      console.log(data);
});
readerStream.on('error', function(err){
      console.log(err.stack);
});
console.log("程序执行完毕");
```

#### 写入流	
```js
var fs = require("fs");
var data = '菜鸟教程官网地址：www.runoob.com';
// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');
// 使用 utf8 编码写入数据	
writerStream.write(data,'UTF8');
// 标记文件末尾
writerStream.end();
// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
        console.log("写入完成。");
});
writerStream.on('error', function(err){
      console.log(err.stack);
});
console.log("程序执行完毕");
```
#### 管道流	
```js
var fs = require("fs");
// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');
// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');
// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);
console.log("程序执行完毕");
```
#### 链式流	
```js
var fs = require("fs");
var zlib = require('zlib');
// 压缩 input.txt 文件为 input.txt.gz
fs.createReadStream('input.txt')
    .pipe(zlib.createGzip())
    .pipe(fs.createWriteStream('input.txt.gz'));
console.log("文件压缩完成。");
```

## 模块系统
> 模块是Node.js 应用程序的基本组成部分，文件和模块是一一对应的，模块使文件可以相互调用。换言之，一个 Node.js 文件就是一个模块，这个文件可能是JavaScript 代码、JSON 或者编译过的C/C++ 扩展。

### 模块导出导入
#### exports （导出）	
```js
exports.hello= function() {
    // ...函数
}
导出属性或方法
module.exports = function() {
    // ...对象
}
导出（包含了很多属性和方法）的对象
```
#### require （导入）	
```js
var hello = require('./hello');
hello.world();
导入属性或方法
var Hello = require('./hello'); 
hello = new Hello(); 
hello.world(); 
导入（包含了很多属性和方法）的对象
```
##### require优先级
	从文件模块缓存中加载→从原生模块加载→从文件加载

## 函数
> 在JS中，一个函数可以作为另一个函数的参数。
### 匿名函数
- 直接在另一个函数的括号中定义和传递这个函数
```js
function execute(someFunction, value) {
    someFunction(value);
}
execute(function(word){ console.log(word) }, "Hello");
```
- 简约而不简单的HTTP服务器
```js
var http = require("http");
http.createServer(function(request, response) {
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello World");
    response.end();
}).listen(8888);
```

## 全局对象
> 在浏览器JS中，通常 window 是全局对象， 而 Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global 对象的属性，global 最根本的作用是作为全局变量的宿主。

### 全局变量
- 在最外层定义的变量；
- 全局对象的属性；

```js
__filename 
	表示当前正在执行的脚本的文件（模块）名，它将输出文件（模块）所在位置的绝对路径。
__dirname 
	表示当前执行脚本所在的目录。
setTimeout(cb, ms)/setInterval(cb, ms)	
	在指定的毫秒(ms)数后执行指定函数(cb)
clearTimeout(t)/clearInterval(t) 
	用于停止一个之前通过setTimeout() 创建的定时器（t）
console	
	用于向标准输出流（stdout）或标准错误流（stderr）输出字符
process	
	用于描述当前Node.js 进程状态的对象，提供了一个与操作系统的简单接口
```
参考：[process](https://www.runoob.com/nodejs/nodejs-global-object.html)
- 隐式定义的变量（未定义直接赋值的变量）。

## 常用工具
> util 是一个Node.js 核心模块，提供常用函数的集合，用于弥补核心 JavaScript 的功能 过于精简的不足。

- util.callbackify（fn）	
	将 async 异步函数（或者一个返回值为 Promise 的函数）转换成遵循异常优先的回调风格的函数
- util.inherits(constructor, superConstructor)	
	实现对象间原型继承的函数
- util.inspect(object,[showHidden],[depth],[colors])
	将任意对象转换 为字符串的方法，通常用于调试和错误输出
- util.isArray(object)	
	如果给定的参数 "object" 是一个数组返回 true，否则返回 false。
- util.isRegExp(object)	
	如果给定的参数 "object" 是一个正则表达式返回true，否则返回false。
- util.isDate(object)	
	如果给定的参数 "object" 是一个日期返回true，否则返回false。

参考：[工具模块](https://www.runoob.com/nodejs/nodejs-utitlity-module.html)

## 文件系统
### 异步和同步
	文件系统（fs 模块）模块中的方法均有异步和同步版本，例如读取文件内容的函数有异步的 fs.readFile() 和同步的 fs.readFileSync()。
	
	异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)，建议大家使用异步方法，比起同步，异步方法性能更高，速度更快，而且没有阻塞。
### 方法
#### fs.open(path, flags[, mode], callback) //打开文件
	- path - 文件的路径。
	- flags - 文件打开的行为。具体值详见下文。
	- mode - 设置文件模式(权限)，文件创建默认权限为0666(可读，可写)。
	- callback - 回调函数，带有两个参数如：callback(err, fd)。
#### fs.stat(path, callback)	//获取文件信息
	- path - 文件路径。
	- callback - 回调函数，带有两个参数如：(err, stats), stats 是 fs.Stats 对象。
#### fs.writeFile(file, data[, options], callback)	// 写入文件
	- file - 文件名或文件描述符。
	- data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(缓冲) 对象。
	- options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
	- callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。
#### fs.read(fd, buffer, offset, length, position, callback)	//读取文件
	- fd - 通过 fs.open() 方法返回的文件描述符。
	- buffer - 数据写入的缓冲区。
	- offset - 缓冲区写入的写入偏移量。
	- length - 要从文件中读取的字节数。
	- position - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。
	- callback - 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。
#### fs.close(fd, callback)	// 关闭文件
	- fd - 通过 fs.open() 方法返回的文件描述符。
	- callback - 回调函数，没有参数。
#### fs.ftruncate(fd, len, callback)	// 截取文件
	- fd - 通过 fs.open() 方法返回的文件描述符。
	- len - 文件内容截取的长度。
	- callback - 回调函数，没有参数。
#### fs.unlink(path, callback)	  // 删除文件
	- path - 文件路径。
	- callback - 回调函数，没有参数。
#### fs.mkdir(path[, options], callback)  //创建目录
	- path - 文件路径。
	- options 参数可以是：
	- recursive - 是否以递归的方式创建目录，默认为 false。
	- mode - 设置目录权限，默认为 0777。
	- callback - 回调函数，没有参数。
#### fs.readdir(path, callback)	// 读取目录
	- path - 文件路径。
	- callback - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表
	- fs.rmdir(path, callback)	// 删除目录
	- path - 文件路径。
	- callback - 回调函数，没有参数。
	
参考：[文件模块方法参考手册](https://www.runoob.com/nodejs/nodejs-fs.html)

## GET/POST请求
### 获取GET请求内容	
> GET请求直接被嵌入在路径中，URL是完整的请求路径，包括了?后面的部分，使用 url.parse 方法来解析 URL 中的参数。

```js
var http = require('http');
var url = require('url');
var util = require('util');
http.createServer(function(req, res){
        res.writeHead(200, {'Content-Type': 'text/plain'});
        // 解析 url 参数
        var params = url.parse(req.url, true).query;
        res.write("网站名：" + params.name);
        res.write("\n");
        res.write("网站 URL：" + params.url);
        res.end();
}).listen(3000);
```

### 获取 POST 请求内容	
> POST 请求的内容全部的都在请求体中，http.ServerRequest 并没有一个属性内容为请求体，原因是等待请求体传输可能是一件耗时的工作，当你需要的时候，需要手动来做。（比如上传文件，而很多时候我们可能并不需要理会请求体的内容，恶意的POST请求会大大消耗服务器的资源，所以 node.js 默认是不会解析请求体的）

```js
var http = require('http');
var querystring = require('querystring');
var util = require('util');
http.createServer(function(req, res){
        // 定义了一个post变量，用于暂存请求体的信息
        var post = '';     
        // 通过req的data事件监听函数，每当接受到请求体的数据，就累加到post变量中
        req.on('data', function(chunk){    
                post += chunk;
        });
        // 在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回。
        req.on('end', function(){    
                post = querystring.parse(post);
                res.end(util.inspect(post));
        });
}).listen(3000);
```

## Web服务器
> Web服务器一般指网站服务器，是指驻留于因特网上某种类型计算机的程序，Web服务器的基本功能就是提供Web信息浏览服务。它只需支持HTTP协议、HTML文档格式及URL，与客户端的网络浏览器配合。大多数 web 服务器都支持服务端的脚本语言（php、python、ruby）等，并通过脚本语言从数据库获取数据，将结果返回给客户端浏览器，目前最主流的三个Web服务器是Apache、Nginx、IIS。

### Web 应用架构	
#### 创建 Web 服务器	
##### http 模块主要用于搭建 HTTP 服务端和客户端
```js
var http = require('http');
var fs = require('fs');
var url = require('url');
// 创建服务器
http.createServer( function (request, response) {  
      // 解析请求，包括文件名
      var pathname = url.parse(request.url).pathname;
      // 输出请求的文件名
      console.log("Request for " + pathname + " received.");
      // 从文件系统中读取请求的文件内容
      fs.readFile(pathname.substr(1), function (err, data) {
            if (err) {
                  console.log(err);
                  // HTTP 状态码: 404 : NOT FOUND
                  // Content Type: text/html
                  response.writeHead(404, {'Content-Type': 'text/html'});
            }else{             
                  // HTTP 状态码: 200 : OK
                  // Content Type: text/html
                  response.writeHead(200, {'Content-Type': 'text/html'});
                  // 响应文件内容
                  response.write(data.toString());        
            }
            //  发送响应数据
            response.end();
      });   
}).listen(8080);
// 控制台会输出以下信息
console.log('Server running at http://127.0.0.1:8080/');
```
#### 创建 Web 客户端	
```js
var http = require('http');
// 用于请求的选项
var options = {
      host: 'localhost',
      port: '8080',
      path: '/index.html'  
};
// 处理响应的回调函数
var callback = function(response){
      // 不断更新数据
      var body = '';
      response.on('data', function(data) {
            body += data;
      });
      response.on('end', function() {
            // 数据接收完成
            console.log(body);
      });
}
// 向服务端发送请求
var req = http.request(options, callback);
req.end();
```

## Express 框架
> Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具，使用 Express 可以快速地搭建一个完整功能的网站。
### 其框架核心特性：
	可以设置中间件来响应 HTTP 请求。
	定义了路由表用于执行不同的 HTTP 请求动作。        
	可以通过向模板递参数来动态渲染
### 安装 Express（此处省略）
### 框架实例
```js
var express = require('express');
var app = express();
app.get('/', function (req, res) {
      res.send('Hello World');
})
var server = app.listen(8081, function () {
    var host = server.address().address
    var port = server.address().port
    console.log("应用实例，访问地址为 http://%s:%s", host, port)
})
```
参考：[Express 4.x API 中文文档](https://www.runoob.com/w3cnote/express-4-x-api.html)、[Express - 中文文档( node.js Web应用框架)](http://expressjs.jser.us/)

