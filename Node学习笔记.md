# Node JS学习笔记

## 程序概念

##### 进程：

​	进程负责为程序的运行提供必备的环境，进程就相当于工厂中的车间。

##### 线程：

​	线程是计算机中的最小的计算单位，线程负责执行进程中的程序，线程就相当于工厂中的工人。

##### Node JS 的线程

​	nodejs 是单线程，单进程。Node 处理请求时是单线程，但是在后台拥有一个 I / O 线程池。

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









```javascript
let a = 0;
console.log(a);
```











