# Node JS学习笔记

## 程序概念

##### 进程：

​	进程负责为程序的运行提供必备的环境，进程就相当于工厂中的车间。

##### 线程：

​	线程是计算机中的最小的计算单位，线程负责执行进程中的程序，线程就相当于工厂中的工人。

##### Node JS 的线程

​	nodejs 是单线程，单进程。Node 处理请求时是单线程，但是在后台拥有一个 I / O 线程池。

##### 主线程

宏任务：主体script, setTimeout, setInterval, new Promise()

微任务：Promise.then, process.nextTick

开定时器是 同步的，定时器的任务（即定时器要做的事情）是异步的。

```javascript
/* 情况一： */
let d = 3;
var c = setInterval(show(), 10000);

function show() {
    console.log('hha');
    console.log(d);
    function a() {
        clearInterval(c);
        console.log('hi');
    };
    return a;
}


/* 情况二： */
let d = 3;
let c = setInterval(show, 10000);

function show() {
    console.log('hha');
    clearInterval(c);
    function a() {
        console.log('hi');
    };
    a();
    return a;
}
```

##### I / O

* input 输入
* output 输出

##### 同步，异步

​	同步：synchronous

​	异步：asynchronous

## Node 的用途

1. Web 服务 API，比如：RESTful API。
2. 实时多人游戏。
3. 后端的 Web 服务，例如跨域，服务器端的请求。
4. 多客户端的通信，如：即时通信。

## Node 开发方向

- GUI：Graphical User Interface 图形用户界面 office, vscode, 浏览器, 播放器...
- CLI：Command-Line Interface 命令行界面，也称 CUI,字符用户界面。虽然没有 GUI 操作直观，但是 CLI 更加节省计算机资源（所以一般用于服务器环境） babel, tsc, webpack, vue-cli
- Server 服务提供(Web Server, IM...)

## 模块化

​	如果程序设计的规模达到了一定程度，则必须对其进行模块化，模块化可以有多种形式。但至少应该提供能够将代码分割为多个源文件的机制。

##### CommonJS

CommonJS 的模块化功能可以帮我们解决模块化的问题。

CommonJS 对模块化的定义：

1. 模块引用
2. 模块定义
3. 模块标识。 

在 Node 中，一个 JS文件 就是一个模块，每一个 JS 文件中的JS代码都是独立运行在一个函数中，而不是全局作用域，所以一个模块中的变量和函数在其他模块中无法访问。

实际上模块中的代码都是包装在一个函数中执行的，并且在函数执行时，同时传递进了 五个实参。

* exports：该对象用来将变量或函数暴露到外部。
* require：函数，用来引入外部的模块
* module：module 代表的是当前模块本身，exports 就是 module 的属性。
* __filename：当前模块的完整路径。
* __dirname：当前模块所在文件夹的完整路径。

##### require

require 的两个作用：

1. 加载文件模块并执行里面的代码
2. 拿到被加载文件模块导出的接口对象

注意事项：

1. 当引入的模块有语法错误时，会报错。
2. 当引入的模块路径有错时，也会报错。
3. 当一个模块被多次引入时，只执行一次。将暴露的对象直接写入缓存，以后就直接从缓存读取。

##### exports 和 module.exports

通过 exports 只能使用 . 的方式来向外暴露对象或内部变量。是 module.exports 的引用。

通过 module.exports 既可以通过 . 的形式，也可以直接赋值。module.exports 才是真正的暴露对象。

既可以使用 exports 导出，也可以使用 module.exports 导出，本质上是一样的。模块暴露的本质是对象（exports）。
```js
console.log(module.exports ==== exports); // true

// 可以使用，是在改对象，此时 module.exports !== exports;因为更改了引用指向的空间。
module.exports = {
    name: 'sky'
};

// 不可以，更改了变量的指向。
exports = {
    name: 'sky'
};

// 可以，此时 module.exports === exports;
exports.name = 'sky';
module.exports.name = 'sky';

// 下面的四种 require 写法，实际引入的是谁？
require('./a.js'); // 引入的是：当前文件夹中的 a.js 文件
require('a.js'); // 引入的是：node_modules 文件夹中的 a.js 文件
require('a'); // 引入的是：node_modules 文件夹中的 a 文件夹中的 index.js 文件
require('./a'); // 引入的是：当前文件夹中 a 文件夹中的 index.js 文件
```

## NPM

常用安装命令：

1. npm install <包的名称> 安装指定的包。
2. npm i <包名> 效果同上，缩写形式。
3. npm i <包的名称>@版本号 安装指定版本的包。
4. npm i <包的名称> -g 全局安装包，一般是安装一些工具。
5. npm i 自动查找当前目录下的 package.json 文件，安装所有依赖。

卸载包：npm uninstall <包名> 卸载包

更新包：npm update <包名> 更新一个包

## 异步（asynchronous）

##### 抛出错误

​	异步：手动抛出错误，没有错误则为 null，不抛出则看不到错误。

​	同步：系统自动抛出错误。

##### 异步的三种实现方式

1. 回调函数。回调函数不一定是异步，但异步一定有回调函数。
2. 事件（基于回调）。on 监听事件
3. promise 作用：用于传递异步消息，是一个承诺对象。

##### 回调函数的机制

* 定义一个普通函数（可用作回调函数）
* 将函数作为参数传入另外一个函数（调用者）
* 调用者在执行过程中根据时机和条件来决定是否调用回调函数

回调函数的用途：

​	通常用于达到某个时机或条件时需要执行代码的情况，我们就会使用回调函数。

异步函数内部不能使用 return。

##### Promise 的状态

Pending 等待中

Resolved 成功

Rejected 失败

* Pending => Resolved
* Pending => Rejected

由于异步的返回结果时间顺序不可控，所以需要使用 Promise 来统一控制输出结果。

利于 Promise 对象的 all 方法可以实现手动调整输出的顺序，相当于把异步变成了同步。

```javascript
/* 回调函数转成 promise */
const util = require('util');
const fs = require('fs');

const readFile = util.promisify(fs.readFile);

// 使用一：
readFile('./a.txt')
    .then(data => {
    	console.log(data);
	})
	.catch(err => {
    	console.log(error);
	})

// 使用二：
async function init() {
    try {
        let data = await readFile('./a.txt');
        console.log(data.toString());
    } catch(err) {
        console.log(err);
    }
}
init();
```

## 文件读写

##### 进制

* Bin 二进制
* Oct 八进制
* Dec 十进制
* Hex 十六进制

##### UTF-8

* UTF-8 一个汉字三个字节，一个英文一个字节。
* GBK 一个汉字两个字节，一个英文一个字节。

##### 文件读取的方式

1. 直接读取：将硬盘上（该文件）的所有内容全部读入内存以后才触发回调函数。有两种写法：异步与同步。
2. 流式读取。将数据从硬盘中读取一节就触发回调函数，实现大文件操作。流式读取，一节大小是 64 kb === 65536 字节。

##### 流

什么是流？

​	所有互联网传输的数据都是以流的方式，流是一组有起点有终点的数据传输方式。

流的操作：

1. 流式读取
2. 流式写入

输出流.pipe(输入流)

链式流：输出流.pipe(输入流).pipe(输入流)

## 网络

##### IP (Internet Protocol) 网络协议

​	所谓协议就是一套规则，而 IP 协议就是为了能够让计算机互连的一种规则，为了标识每台连入网络中的计算机（网卡）的唯一性，每个连入网络的网卡都会绑定一个 IP 地址（固定 - 买断、临时分配）

##### IP 分配使用

 * IP 地址的使用是有一定规则和规划的。
* 127.0.0.1： 本地回环网络地址（其实就是自己 CALL 自己的一个快捷方式）
* (192.168.0.1 - 192.168.255.254 / 172.16.0.1 - 172.31.255.254 / 10.0.0.1 - 10.255.255.254) 局域网络（局域网）使用
* 其他

##### 端口

发送数据的端口由系统分配

一个应用程序可以同时监听多个网卡的多个端口

一个端口只能同时被一个程序监听

如果一个程序尝试监听一个已经被其他程序监听的端口，就会报端口占用的错误。

##### 数据传输协议

1. TCP
   * 可靠的、面向连接的协议、传输效率低
   * 效率要求相对低，但对准确性要求相对高的场景
   * 文件传输、接受邮件、远程登录
2. UDP
   * 不可靠的、无连接的服务、传输效率高
   * 效率要求相对高，对准确性要求相对低的场景
   * 在线视频、网络语音电话

##### dgram (数据报)

​	dgram 模块提供了 UDP 数据包 socket 的实现。socket 又称“套接字”，应用程序通常通过“套接字”向网络发出请求或者应答网络请求，其本质上就是一套用于实现网络数据交换的接口(API)。

```javascript
const dgram = require('dgram');
```

##### Net 模块 (TCP)

net 模块提供了创建基于流的 TCP 或 IPC 服务器 (net.createServer()) 和客户端 (net.createConnection()) 的异步网络 API

```javascript
const net = require('net');
```

使用：

* 服务端： 提供服务，被连接，被请求的一方
* 客户端： 获取服务，发起连接，请求的一方

##### HTTP

HTTP 的结构： HTTP 是一个基于 TCP / IP 通信协议来传输（超文本）数据

HTTP 是基于 TCP / IP 协议来定位传输数据

同时在 TCP / IP 包基础上对要传输的数据进行再次包装

HTTP 是单向单链接、无状态协议

HTTP: 传输 ht(超文本) 这样的文本的规则：

* 规定了请求发送的数据格式
  * Request Line: 请求行
  * Request header: 请求头
  * Request body: 请求正文
* 规定了返回的数据的格式
  * Response Line: 响应行
  * Response header: 响应头
  * Response body: 响应正文
* 传输的规则

##### HTTP 状态码分类：

​	HTTP/1.1 中定义了 5 类状态码，状态码由三位数字组成，第一个数字定义了响应的类别。

* 1 XX 提示信息 - 表示请求已被成功接收，继续处理没有特殊含义。
* 2 XX 成功 - 表示请求已被成功接收，理解，接受。
  * 200 成功
* 3 XX 重定向 - 表示完成请求必须进行更进一步的处理。
  * 301 永久重定向
  * 302 临时重定向
  * 304 使用缓存（服务器没有更新过）
* 4 XX 客户端错误 - 请求有语法错误或请求无法实现。
  * 400 请求语义有误
  * 401 当前请求需要用户验证
  * 403 权限不足，无法访问
  * 404 资源找不到
* 5 XX 服务器端错误 - 服务器未能实现合法的请求。
  * 500 服务器端代码有错误
  * 502 网关错误
  * 503 服务器已奔溃

##### URL

Uniform Resource Locator: 统一资源定位符，是对可以从互联网上得到的资源的位置和访问方法的一种简洁的表示，是互联网上标准资源的地址。

格式： protocol://hostname[:port]/path/[?query]#fragment





```javascript
let a = 0;
console.log(a);
```











