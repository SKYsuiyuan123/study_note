# JS 学习笔记

## 语言分类

#### 编译型

通篇翻译，翻译成文件。优点：执行快。缺点：移植性不好（不夸平台）。如：C，C++，Go。

#### 解释型

解释一行执行一行。优点：夸平台。缺点：执行慢。如：PHP, Python, JavaScript

Java 是一个具有编译型和解释型的语言。

JS 是动态性语言，脚本语言，解释型语言，跨平台，单线程。

## 内存

#### 内存溢出

一种程序运行出现的错误，当程序运行需要的内存超过了剩余的内存时，就会抛出内存溢出的错误。

#### 内存泄露

占用的内存没有及时释放

内存泄露积累多了就容易导致内存溢出

常见的内存泄露：

- 意外的全局变量
- 没有及时清理的计时器或回调函数
- 闭包

## 进程 & 线程

#### 进程

程序的一次执行，它占有一片独有的内存空间，可以通过 windows 任务管理器查看进程。

#### 线程

- 是进程内的一个独立执行单元
- 是程序执行的一个完整流程
- 是 CPU 的最小的调度单元

#### 线程池

保存多个线程对象的容器，实现线程对象的反复利用

#### 相关知识

- 应用程序必须运行在某个进程的某个线程上
- 一个进程中至少有一个运行的线程：主线程，进程启动后自动创建
- 一个进程中也可以同时运行多个线程，我们会说程序是多线程运行的
- 一个进行内的数据可以供其中的多个线程直接共享
- 多个线程之间的数据是不能直接共享的
- 线程池：保存多个线程对象的容器，实现线程对象的反复利用

#### 多进程 & 多线程

多进程运行：一个应用程序可以同时启动多个实例运行

多线程：在一个进程内，同时有多个线程运行



js 是单线程运行的，但是使用 H5 中的 Web Workers 可以多线程运行。

## JS 基础语法

### 变量

#### 变量的类型

 - 原始值： Number, Boolean, String, Undefined, Null, Symbol
 - 引用值：Array, Object, Function, Date, RegExp
 - 原始值：不可改变的原始值（栈内存的东西是无法删除的，只能覆盖）

#### 变量的存放位置

 - 原始值：栈内存中，stack 先进后出的原则。
 - 引用值：堆内存中，heap 散列结构。

#### 变量之间的拷贝

 - 栈内存与栈内存之间的赋值是拷贝（原始值方式）。
 - 堆内存与堆内存之间的赋值是拷贝的栈内存中的地址（引用值方式）。
 - 多个引用变量指向同一个对象，通过一个变量修改对象内部数据，其它所有变量看到的是修改之后的数据。
 - 2 个引用变量指向同一个对象，让其中一个引用变量指向另一个对象，另一个引用变量依然指向前一个对象。

```javascript
/* 情况一： */
let obj1 = {name: 'Tom'};
let obj2 = obj1;
obj2.age = 12;
console.log(obj1.age); // 12
function fn(obj) {
    obj.name = 'A'; // 给引用设置属性
}
fn(obj1);
console.log(obj2.name); // 'A'

/* 情况二： */
let a = {age: 12};
let b = a;
a = {name: 'BoB', age: 13};
b.age = 14;
console.log(b.age, a.name, a.age); // 14, 'BoB', 13
function fn2(obj) {
    obj = {age: 15}; // 更改变量的值
}
fn2(a);
console.log(a.age); // 13
```

#### 内存管理

内存生命周期：

1. 分配需要的内存
2. 使用分配到的内存
3. 不需要的将其释放 / 归还

* 分配小内存空间，得到它的使用权。
* 存储数据，可以反复进行操作
* 释放小内存空间

释放内存：

* 局部变量：函数执行完自动释放
* 对象：成为垃圾对象 ==> 垃圾回收器回收

```javascript
let a = 3;
let obj = {};
obj = undefined;

function fn() {
    let b = {};
    // b 是自动释放，b所指向的对象是在后面的某个时刻由垃圾回收器回收。
}
fn();
```

### Number

```javascript
/*
* Infinity
* -Infinity
* NaN
* 以上几个值都是 Number 类型
*/
let a = 1 / 10; // Infinity 正无穷
let b = -1 / 10; // -Infinity 负无穷
let c = 0 / 0; // NaN

// JS中可以表示的数字的最大值
let a = Number.MAX_VALUE;
console.log(a); // 1.7976931348623157e+308
let b = a * a;
console.log(b); // Infinity
// JS中可以表示的数字的最小值
let a = Number.MIN_VALUE;
console.log(a); // 5e-324
```

### typeof 操作符

写法：typeof xx 或者 typeof (xx)

返回值：'number', 'string', 'boolean', "object", "undefined", "function", "symbol"

typeof null 返回 'object'

返回值类型：string

当且仅当，一个变量未经声明，放在 typeof 里边时，不报错，返回 undefined。

```javascript
let b = typeof(a); // undefined
```

### 显示类型转换

Number(mix)，parseInt(string, radix)，parseFloat(string)，toString(radix)，String(mix)，Boolean()

方法：

* toString(radix)，

函数：

* String(mix)，Number(mix)，parseFloat(string)，parseInt(string, radix)，

parseInt(目标值，radix); radix 的取值范围 2 - 36，可省略，默认是 10。以 radix 进制为 基理，转化为 十进制。从数字位开始看，一直看到非数字位。转化不了返回 NaN。

目标值.toString(radix)；radix 的取值范围 2 - 36，可省略，默认是 10。以 10 进制为基理，转化为 目标进制。转化后的值是 string 类型。undefined 和 null 不可以调用（原型链上没有该方法）String()函数 是通用的，undefined 和 null 都可以使用。

```javascript
Number(undefined); // NaN
Number(null); // 0
parseInt(undefined); // NaN
parseInt(null); // NaN

let d = 'b';
let num = parseInt(d, 16); // 以 十六 进制为 基理，转化为 十进制
console.log(typeof num + ' : ' + num); // number : 11
let d2 = '123abc';
console.log(parseInt(d2)); // 123

// 二进制的 10000 转化为 十六进制
let e = 10000;
let rs = parseInt(e, 2); // 16
let rs2 = rs.toString(16); // '10'
console.log(parseInt(rs2)); // 10
```

### 隐式类型转换

隐式类型转换： 内部调用的都是 显示类型转换 的方法。

调用以下方法(隐式调用的是 Number())，转换后的值的类型都是 Number 类型

* isNaN()，+ / -（一元运算符），++ / -- （加加，减减） *，-，%

加号（+） 转换为 String 类型

运算符 &&， ||， ！，<，>，<=，>=，==，!= 转换后的值为 Boolean 类型

没有类型转换： ===，!==

undefined，null，NaN，'""，0，这六个转换为 布尔值 后都是 false

```javascript
/* 隐士类型转换 */
/*
* true 隐士类型转换后为 1
* false 隐士类型转换后为 0
*/
let num = true + true; // 2

/* 字符串与字符串比较的是 asc 码的顺序 */
/*
* A - Z: 65 - 90
* a - z: 97 - 122
* A 与 a 相差 32
*/
let a = 'A' > 'B'; // false
let c = '3' > '2'; // true

/* 逻辑运算 短路与或非 &&, ||, ! */
let b = 1 + 1 && 1 - 1; // 0
let b2 = 2 & 3; // 3
let b3 = 0 && 2; // 0
let b4 = 1 + 1 || 1 - 1; // 2
let b5 = 2 || 3; // 2
let b6 = 0 || 2; // 2

// 短路与，执行 JS代码
1 + 1 && console.log('111'); // '111'

/* 数字比较 */
let d = 1 > '2'; // false

/* 特殊的 */
false > true; => 0 > 1; // false
undefined > 0 => // false
undefined < 0 => // false
undefined == 0 => // false
2 > 1 > 3 => // false
2 > 3 < 1 => // true

/* NaN 不等于一切 */
NaN == undefined; // false
NaN == NaN; // false
console.log(isNaN(NaN)); // true

null > 0 => false
null < 0 => false
null == 0 => false
null == undefined => true
null == null => true

Infinity == Infinity => true
```

### 循环

```javascript
for(let i = 0; i < 10; i++) {
    console.log('a');
}
/*
步骤：
1, let i = 0;
2, 
if(i < 10) {
	console.log('a');
}
3, i++;
4, 
if(i < 10) {
	...
}
先执行一遍 1，判断 2， 执行语句，执行 3，判断 2，执行语句，执行 3。... 不符合 2 退出循环。
*/

/*
break 和 continue 默认只会对离它最近的 循环起作用
*/

/*
可以为循环语句创建一个 label，来标识当前的循环
label:循环语句
使用 break 语句时，可以在 break 后跟着一个 label，这样 break 将会结束指定的循环，而不是最近的
*/
outer:
for(let i = 0; i < 5; i++) {
    console.log('@外层循环：' + i);
    for(let j = 0; i < 5; j++) {
        break outer;
        console.log('内层循环：' + j);
    }
}
```

never-ending，loop 无限循环（死循环）。

### 函数

特点：高内聚，低耦合。

基本用法：简化代码。

- 函数声明：function a() {}
- 函数表达式 / 匿名函数表达式：let b = function() {}
- 命名函数表达式：let c = function adc() {} 这里的 abc 没啥用。 

使用 函数声明 形式创建的函数，可以在函数声明前来调用函数。

使用 函数表达式 创建的函数，不可以声明前调用。

function sum(a, b, c) {

​	// a, b, c是不定参。

​	// 形参与实参是映射关系。

​	sum.length: 形参长度。

​	arguments.length: 实参长度。

​	// arguments 类数组

​	当实参长度小于形参长度时，形参后边对应不到的值会返回 undefined ，同理用 arguments 取不存在的实参时，也会返回 undefined。

}

sum(1, 2, 3, 4, 5);

函数返回值：函数会在最后默认加上 return

不定参：参数的个数不固定，参数决定了函数的神奇。

```javascript
function mianji(r) {
    return 3.14 * r * r;
}
function fun(a) {
    console.log(a);
}
fun(mianji); // 将 mianji 这个函数对象作为参数传递进 fun() 里边。
fun(mianji(10)); // 将 mianji(10) 的返回值作为参数传递进 fun() 里边。
```

JS 调用函数传递变量参数时，是值（基本值 或 引用值）的传递。

函数的参数：

- 如果是简单类型，会做一个值类型的数值副本传到函数内部。
- 如果是引用类型，会将引用类型的地址值复制给参数。

```javascript
let a = 3;
function fn(a) {
    a = a + 1;
}
fn(a);
console.log(a); // 3

function fn2(obj) {
    console.log(obj.name); // 'Tom'
}
let obj = {name: 'Tom'}
fn2(obj);
```

### 递归

递归要注意的地方：

1. 找规律
2. 找出口（拿已知条件）

递归可以让代码变得简洁，但是执行的慢。特别复杂的程序不能用递归。

### 作用域

function test(){ }

test.[[scope]]; ==> 存在的是这个函数的作用域（执行期上下文的集合），无法使用（系统使用）

执行期上下文：每次调用都独一无二，重新调用会新创建。函数执行完毕销毁。

查找变量：从作用域链的顶端依次向下查找。

作用域链：[[scope]] 中所存储的执行期上下文对象的集合，这个集合呈链式链接，我们把这种链式链接叫做作用域链。

内部函数的第二个 AO 就是外部函数的 AO，是一种引用。

变量未经声明就赋值，归全局所有。

全局作用域在程序打开时创建（定义时产生，而非运行时产生），在程序关闭时销毁。

```javascript
let arr = [1,2];
function tst(){
    arr.push(4);
    console.log(arr); // [1, 2, 4]
}
tst();
console.log(arr); // [1, 2, 4]

let a = 3;
function test() {
    a -= 1;
    console.log(a); // 2
}
test();
console.log(a); // 2

let x = 10;
function fn() {
    console.log(x); // 10
}
function show(f) {
    let x = 20;
    f();
}
show(fn);

let fn2 = function () {
    console.log(fn2);
}
fn2(); // fn2 函数体
let obj = {
    fn3: function () {
        console.log(fn3); // fn3 is not defined
    }
}
obj.fn3();

if (!('a' in window)) {
  var a = 10;
}
console.log(a); // 用 var 声明结果为 undefined，用 let 声明结果为：Uncaught ReferenceError: a is not defined
```

### 闭包

能不用闭包就不用闭包。

```javascript
function a() {
    function b() {
        let bbb = 234;
        console.log(aaa); // 打印123
	}
	let aaa = 123;
	// 里边的函数活的比外边的还长，就产生闭包。
	return b; // 内部函数被保存到外部必须生成闭包
}
let demo = a();
demo();

function test() {
    let num = 100;
    function a() {
        num++;
        console.log(num);
    }
    function b() {
        num--;
        console.log(num);
    }
    return [a, b];
}
let myArr = test();
myArr[0](); // 101
myArr[1](); // 100

myArr = null; // 让内部函数成为垃圾对象 --> 回收闭包
```

闭包会导致原有作用域链不释放，造成内存泄露。内存会占用很多，就是另一个泄露。

闭包的作用：

1. 实现公有变量。eg：函数累加器
2. 可以做缓存（存储结构）。
3. 可以实现封装，属性私有化。
4. 模块化开发，防止污染全局变量。

闭包产生的问题，要用闭包解决。

### 立即执行函数

IIFE（Immediately-Invoked Function Expression）

针对初始化功能的函数执行完就被释放，可以有返回值，参数。

```javascript
(function() { }());

let num = (function(a, b, c) {
    let d = a + b + c * 2 - 2;
    return d;
}(1, 2, 3));
console.log(num); // 7

(function() {
    let a = 1;
    function test() {
        console.log(++a);
    }
    window.$ = function() {
        return {
            test,
        }
    }
}());
$().test(); // 2
```

形式：

1. (function () { }()); // W3C 建议使用这种
2. (function() { })();

只有表达式才能被执行符号执行。

function test() {}(); ==> 报错，函数声明不可以被执行

let test = function() {}(); ==> 函数表达式可以执行，会自动放弃名字。

console.log(test); // undefined，函数默认返回 undefined

能被执行符号执行的表达式，函数名字就会被忽略。

以下两个可以，但是放弃名字，本质也是立即执行函数（+，-，！都可以）。

+function test() {}();

-function test() {}();

不可以：*function test() {}();

function test(a, b, c, d) {

​	console.log(a + b + c + d); // 不会报错，也不执行。

}(1, 2, 3, 4); // 系统执行时把它俩分开了。

(1, 2, 3, 4); // 4  ',' 也是一种运算符。

(window.foo || (window.foo = 'bar')); // 返回 'bar'，并且 window.foo 的值也是 'bar'

立即执行函数：经常用来初始化数据。

```javascript
let x = 1;
// if 的判断条件是 函数表达式
if(function f(){}) { // 里边的值不是 false，继续执行
    x += typeof f; // f 未定义，返回 'undefined'
}
console.log(x); // '1undefined'

let f = (
    function f() {
        return '1';
    },
    function g() {
        return 2;
    }
)();
console.log(typeof f); // 'number'
```

### JS 的运行

1. 语法分析 --> 通篇扫描，找语法错误。
2. 预编译。函数声明整体提升，变量声明提升(主要是用 var 声明的会提升)，变量赋值不提升。
3. 解释执行。

### 预编译

 预编译：发生在函数执行的前一刻。

1. imply global 暗示全局变量，即任何变量，如果变量未经声明就赋值，此变量就为全局对象所有。
2. 一切声明的全局变量，全是 window 的属性。
3. let 声明的全局变量 并不是，var 声明的才是。

```javascript
let a = 3;
console.log(a); // 3
console.log(window.a); // undefined
console.log(window.a == a); // false
console.log(window.a === a); // false
var b = 4;
console.log(b); // 4
console.log(window.b); // 4
console.log(window.b == b); // true
console.log(window.b === b); // true
```

步骤：

1. 创建 AO(Activation Object) 执行期上下文 --> 作用域     全局生成 GO === window，局部生成 AO
2. 找形参和变量声明，将变量和形参名作为 AO 属性名，值为 undefined。
3. 将实参值和形参统一。
4. 在函数体里面找函数声明（不是函数表达式，函数表达式算变量），值赋予函数体。

```javascript
function fn(a) {
    console.log(a); // f a() {}
    var a = 123;
    console.log(a); // 123
    function a() {}
    console.log(a); // 123
    var b = function () {}
    console.log(b); // f () {}
    function d() {}
}
fn(1);

/* ES6 */
function fn2(a) {
    console.log(a); // f a() {}
    let a = 123; // 报错，'a' has already been declared
    console.log(a); // 从报错部分往下，都不再执行。
    function a() {}
    console.log(a);
    let b = function () {}
    console.log(b);
    function d() {}
}
fn2(1);

function fn3(a) {
    console.log(a); // f a() {}
    function a() {}
    console.log(a); // f a() {}
    let b = function () {}
    console.log(b); // f () {}
    function d() {}
}
fn3(1);
```

### 对象

```javascript
let sky = {
    name: 'sky',
    age: 24,
    drink: function(dink) {
        console.log('I like dink:' + dink);
    }
}
```

对象属性的增、删、改、查

- 增：sky.sex = 'male';
- 查：sky.name; // 'sky'
- 改：sky.age = 25;
- 删：delete sky.age; // true 删除一个 存在 或 不存在 的属性时 都会返回 true
- console.log(sky.age); // undefined 当查询对象的一个不存在的属性时，不会报错，而是返回 undefined。

#### 对象的创建

对象字面量（plainObject）/ 对象直接量（建议使用）let obj = {};

构造函数法：

1. 系统自带的构造函数 let obj = new Object();
2. 自定义 function Person(){}  let p1 = new Person();

构造函数内部原理（前提是使用时要加 new）：

1. 在函数体最前面隐式的加上 this = {};
2. 执行 this.xxx = xxx;
3. 隐式的返回 this

```javascript
function Car() {
    this.name = 'BMW';
    this.height = 1400;
    this.run = function () {
        this.height--;
    }
    // 如果显式的加上：return {}; 则都是空对象。
    // 如果显式的加上：return 123/null; 则不影响。
}
let car1 = new Car();
let car2 = new Car();
// car1 与 car2 是两个对象，互相之间不会影响。
```

#### 对象的属性描述符

参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

```javascript
let obj = {};

// 默认情况下，使用 Object.defineProperty() 添加的属性值是不可修改的。
Object.defineProperty(obj, 'a', {
  configurable: true, // 当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。
  enumerable: true, // 当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。
  value: 'aa', // 该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
  writable: true // 当且仅当该属性的writable为true时，value才能被赋值运算符改变。默认为 false。
});
```

#### 包装类

原始值：let num = 123;

对象 123：let num2 = new Number(123); 可以添加属性和方法。eg: num2.a = 3; console.log(num2.a); => 3

num2 * 2; => 246 此时返回 原始值，不再是对象了。

字符串，布尔值都可以。但是，undefined 和 null 不可以有属性。

let str = new String('abcd');

let bol = new Boolean('true');

let str1 = 'abcd'; // 不能有属性和方法，是原始值

str1.a = 'e'; => 经历了一个包装类： => new String('abcd').a = 'e'; delete

console.log(str1.a); => undefined   => new String(str1).a 没有了，所以返回 undefined。

console.log(str1.length); => 4

```javascript
let arr = [1, 2, 3, 4];
arr.length = 2; // 数组截断操作。
console.log(arr); // [1, 2];

let str = 'abcd';
str.length = 2; // 字符串没有截断操作。
console.log(str); // 'abcd'
console.log(str.length); // 4 等价于：new String(str).length 这是系统自带的属性
```

严格模式下，在不可扩展的对象上添加属性时会报 TypeError（类型错误），而不是忽略。

#### 原型

定义：原型是 function  对象的一个属性，它定义了构造函数制造出的对象的公共祖先。通过该构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象。

Person.prototype -- 原型

Person.prototype = {} 是祖先。

```javascript
Person.prototype = {
    height: 175,
    width: 200
};
Person.prototype.name = 'hehe'; // 公有祖先，被继承。
function Person() {
    this.name = 'sky'; // 自己有的用自己的，没有的用祖先的。
}
let person1 = new Person();
let person2 = new Person();
```

person1.constructor => 返回构造器，可以被更改。对象无法修改原型的属性（后代无法修改祖先的东西），可以自己添加自己的，原型的属性通过 prototype 改。

1. 利用原型特点和概念，可以提取公有属性。
2. 对象如何查看原型： 隐式属性 _ _proto_ _
3. 对象如何查看对象的构造函数：constructor

```javascript
Person.prototype.name = 'sky';
function Person() {
    // let this = {
    // 隐式属性：__proto__: Person.prototype // 可以被更改。
    // __proto__ 是连接的作用。
    // Person.prototype 是原型。
	// }
}
let p1 = new Person();
p1.name; // 'sky'
let obj = {
    name: 'sunny'
};
person.__proto__ = obj;
// 此时：person.name = 'sunny'

Person.prototype.name = 'sky';
function Person() {
    // let this = {
    //	 __proto__: Person.ptototype
    // 	 __proto__ 指向原来的空间
	// }
}
let p2 = new Person();
// 下边这行与上边这行换一下 就不一样了。
Person.prototype = {
    // 开辟新空间
    name: 'cherry'
};
console.log(p2.name); // 'sky'
```

#### 原型链

原型链通过 prototype 连接。本质上是隐式原型链，按照 _ _proto_ _ 向上查找。

Object {} 是原型链的终端。

Object.prototype._ _proto_ _ => null

后代无法：增，删，改，祖先原型的属性。引用值 . 可以调用修改。

绝大多数对象的最终都会继承自：Object.prototype

但是这个就没有原型：let obj = Object.create(null); // 括号里不能放原始值

let obj = Object.create(原型); 也可以创建出一个对象。

null 和 undefined 不是对象，也没有原型，所以没有 toString() 方法。

let num = 123;

num.toString(); => '123'

但是 123.toString(); 则会报错。系统把 . 当成了小数点，后边的无法识别。

Number 上有 toString 方法，不用去 Object.prototype 上找，它把 Object.toString() 方法重写了。

Array 和 Boolean 以及 String 都重写了该方法。

```javascript
let obj = {};
obj.toString(); // '[object Object]'

Object.prototype.toString.call(123); // '[object Number]'
Object.prototype.toString.call(true); // '[object Boolean]'
Object.prototype.toString.call([]); // '[object Array]'

document.write(); // 该方法会隐式的调用 toString 方法。
```

```javascript
console.log(Object instanceof Function); // true
console.log(Object instanceof Object); // true
console.log(Function instanceof Function); // true
console.log(Function instanceof Object); // true

function Foo() {}
console.log(Object instanceof Foo); // false

function F() {}
Object.prototype.a = function() {
    console.log('a()');
}
Function.prototype.b = function() {
    console.log('b()');
}
let f = new F();
F.a(); // a()
F.b(); // b()
f.a(); // a()
f.b(); // 报错 f.b is not a function
```

```javascript
// 情况一 位置不同打印不同。
Person.prototype.name = "sunny";
function Person() {}

var p1 = new Person();

console.log(p1);
console.log(p1.name); // "sunny"

Person.prototype.name = "cherry";

var p2 = new Person();

console.log(p1.name); // "cherry"
console.log(p2.name); // "cherry"


// 情况二
Person.prototype.name = "sunny";
function Person() {}

var p1 = new Person();

console.log(p1);
console.log(p1.name); // "sunny"

Person.prototype = {
  name: "cherry"
};

var p2 = new Person();

console.log(p1.name); // "sunny"
console.log(p2.name); // "cherry"
```

#### call / apply

作用：改变 this 指向。

区别：后面传的参数形式不同(传参列表不同)。

call：可以一位一位的传实参。

apply：必须传数组形式的实参。传的是 arguments。

```javascript
function Person(name, age) {
    // 用了 call 后 this = obj;
    this.name = name;
    this.age = age;
}
let p1 = new Person('deng', 30);
let obj = {};
Person.call(obj, 'sky', 25); // obj 是 this 的指向，后边跟的是参数（实参）
// 借助 Person 环节，制造自己的对象。
console.log(obj); // {name: 'sky', age: 25}

function Student(name, age, sex) {
    /* 借用别人的函数，实现自己的功能 */
    Person.call(this, name, age);
    // 或者使用：Person.apply(this, [name, age]);
    this.sex = sex;
}

let s1 = new Student('sky2', 26, 'male');
console.log(s1); // {name: 'sky2', age: 26, sex: 'male'}

// call / apply 第一个参数的不同
function sum(a, b) {
  console.log(this);
  return a + b;
}

sum.call(null, 3, 5);
sum.call(undefined, 1, 2);
sum.apply(undefined, [1, 2]); // 一个参数为 null / undefined 时 this 指向 window。

sum.apply(1, [1, 2]);
sum.apply('2', [1, 2]);
sum.call(true, 1, 2); // 第一个参数为 string / number / boolean 时 this 指向 该类型的包装类。

sum.call({
  a: 2,
  b: 3
}, 2, 3); // {a: 2, b：3}
```

#### 继承发展史

1. 传统形式：原型链。prototype。过多的继承了没用的属性。
2. 借用构造函数。call() / apply()。不能继承借用构造函数的原型，每次构造函数都要多走一个函数。
3. 共享原型。不能随便改动自己的原型。=> Son.prototype = Father.prototype
4. 圣杯模式

```javascript
Father.prototype.lastName = 'Deng';
function Father() {
    this.name = 'hehe';
}
function Son() {
    this.name = 'ha';
}
// 圣杯模式 的继承
function inherit(Target, Origin) {
    function F() {}
    F.prototype = Origin.prototype;
    Target.prototype = new F();
    Target.prototype.constructor = Target;
    Target.prototype.uber = Origin.prototype; // 指出，该对象到底继承自谁。
}
/* 更完美一点 */
let inherit = (function() {
    let F = function() {}; // 私有化变量。
    return function(Target, Origin) {
        F.prototype = Origin.prototype;
        Target.prototype = new F();
        Target.prototype.constructor = Target;
        Target.prototype.uber = Origin.prototype; // 指出，该对象到底继承自谁。
    }
}());

inherit(Son, Father);
let s1 = new Son();
let f1 = new Father();
```

#### 命名空间

管理变量，防止污染全局，适用于模块化开发。用闭包解决。

```javascript
let init = (function() {
    let name = 'abc'; // 闭包，变量私有化，不污染全局变量。
    function callName() {
        console.log(name);
    }
    return function() { // 接口，方便启动
        callName();
    }
}());
init(); // 启动初始化程序。
```

#### 链式调用

```javascript
let deng = {
    smoke: function() {
        console.log('smoking...');
        return this; // 不写，默认返回 undefined 就无法实现链式调用
    },
    drink: function() {
        console.log('drinking...');
        return this;
    }
};
deng.smoke().drink(); // 链式调用对象的方法。
```

#### 对象的枚举

对象.hasOwnProperty(prop); 判断 prop 这个属性是否是 该对象上的，是的话返回 true，不是返回 false，原型链和原型上的都不是自己的属性。prop：属性名的字符串。

```javascript
let obj = {
    name: 'sky',
    age: 25,
    height: 175
};
'name' in obj; // true 判断该属性名是不是该对象上的，但是无法分辨是否是该对象的原型上的。
obj.hasOwnProperty('age'); // true
console.log(obj.height); // 175
console.log(obj['height']); // 175

for(let prop in obj) {
    console.log(obj.prop); // 错误  obj.prop 原理 --> obj['prop']
    console.log(obj[prop]); // 正确  obj[prop] 原理 --> obj['name']/...
}
```

A instanceof B // 返回值：true / false 判断 A 对象是不是 B 构造函数构造出来的，看 A 对象的原型链上有没有 B 的原型。

hasOwnProperty：是否是 对象自身属性

区别数组和对象的方法：

1. toString.call()
2. constructor
3. instanceof

```javascript
Object.prototype.toString.call([]); // '[object Array]'
Object.prototype.toString.call(123); // '[object Number]'
Object.prototype.toString.call({}); // '[object Object]'

[].constructor; // ƒ Array() { [native code] }

[] instanceof Array; // true
[] instanceof Object; // true
({} instanceof Array); // false
```

#### this

1. 函数预编译过程 this => window
2. 全局作用域里 this => window
3. call / apply  可以改变函数运行时 this 指向
4. obj.func(); func() 里面的 this 指向 obj。

```javascript
let name = '222';
let a = {
    name: '111',
    say: function() {
        console.log(this.name);
    }
}
let fun = a.say; // 等号 是函数体引用
fun(); // 当全局变量name是用 var 声明时打印'222'，用 let 声明，就打印空值
a.say(); // '111'

let b = {
    name: '333',
    say: function(fun) {
        fun(); // 空执行，走编译，没有人调用，默认 this 指向 window。
    }
}
b.say(a.say); // '222'
b.say = a.say;
b.say(); // '333'


let foo = 123;
function print() {
    this.foo = 234;
    console.log(foo);
}
print(); // 123 用 var 声明 foo 就打印 234
new print(); // 123 用 var 声明 foo 就打印 234

let bar = {a: '002'};
function print() {
    bar.a = 'a';
    Object.prototype.b = 'b';
    return function inner() {
        console.log(bar.a);
        console.log(bar.b);
    }
}
print()(); // 'a', 'b'
```

arguments.callee 指向函数自身的引用（严格模式无法使用）。

#### 深度克隆

利用递归

```javascript
let obj = {
    name: 'sky',
    se: 'male',
    height: 175,
    like: ['songyang', 'xiaocai', [1, 2], {
        key: 'oo',
        aaa: [3, 4, 5]
    }],
    movie: {
        a: 'haha',
        b: {
            bb: 'ok',
            bbb: ['okb', 'okbbb']
        }
    }
};

let obj1 = {};

function clone(Origin, Target = {}) {
    for (const key in Origin) {
        if (Origin.hasOwnProperty(key)) {
            // 判断是否是原始值 或 函数
            if (typeof Origin[key] === 'number' || typeof Origin[key] === 'string' || typeof Origin[key] == 'boolean' || typeof Origin[key] === 'undefined' || Origin[key] === null || typeof Origin[key] === 'function') {
                Target[key] = Origin[key];
            } else {
                // 区别是那种对象
                if (Array.isArray(Origin[key])) {
                    // 是一个数组
                    Target[key] = [];
                    for (let i = 0; i < Origin[key].length; i++) {
                        if (Array.isArray(Origin[key])) {
                            clone(Origin[key], Target[key]);
                        } else if (Object.prototype.toString.call(Origin[key]) === '[object Object]') {
                            clone(Origin[key][i], Target[key][i]);
                        } else {
                            Target[key].push(Origin[key][i]);
                        }
                    }

                } else if (Object.prototype.toString.call(Origin[key]) === '[object Object]') {
                    // 是一个对象
                    Target[key] = {};
                    clone(Origin[key], Target[key]);
                }
            }
        }
    }
    return Target;
}

clone(obj, obj1);
console.log(obj1);

/* 渡一 */
// 1.判断是不是原始值
// 2.判断是数组还是对象
// 3.建立相应的数组或对象
// 4.递归
function deepClone(Origin, Target = {}) {
    for (const key in Origin) {
        if (Origin.hasOwnProperty(key)) {
            if (typeof Origin[key] === 'object' && Origin[key] !== null) {
                // 对象
                Target[key] = Array.isArray(Origin[key]) ? [] : {};
                deepClone(Origin[key], Target[key]);
            } else {
                // 原始值
                Target[key] = Origin[key];
            }
        }
    }
    return Target;
}
```

### 数组

#### 普通数组

字面量：let arr = [];

构造函数：let arr2 = new Array(); // 当只有一位元素的话，不能写小数，不合法。一个参数，如果写入的是 数字，代表的是这个数组的长度，如果是字符串，代表数组有一个元素，值是 这个参数。

console.log(arr[n]); // 越界读取，不报错，返回 undefined。arr 是基于对象的。

arr[n] = 'abc'; 不会报错，会撑大数组。

方法：

1. 改变原数组：push, pop, shift, unshift, sort, reverse, splice, ...
2. 不改变原数组：concat, join, slice, toString, ..

```javascript
// 使用 delete 删除
let arr = [1, 2, 3];
delete arr[0]; // 删除后数组的长度不变。
console.log(arr); // [empty, 2, 3]
console.log(arr[0]); // undefined
```

#### 类数组

类数组：把数组和对象的好处，特性结合在一起。但是数组的方法要自己添加。

arguments：类数组，属于对象， typeof(arguments) => object。

```javascript
// 1.属性要为索引（数字）属性，必须有 length 属性，最后加上 push。
let obj = {
    0: 'a',
    1: 'b',
    2: 'c',
    length: 3,
    push: Array.prototype.push,
    splice: Array.prototype.splice // 加上这个属性，类数组就跟数组长的一样了。
}

let arrObj = Array.from(obj); // 将类数组，转化为真正的数组。
console.log(arrObj);

/* 类数组原理 --> 借助了 数组的 push 方法 */
Array.prototype.push = function(target) {
    this[this.length] = target; // 这个 this.length 是关键
    this.length++;
    return this.length;
};

// 题
let obj2 = {
    '2': 'a',
    '3': 'b',
    'length': 2,
    'push': Array.prototype.push
};
obj2.push('c');
obj2.push('d');
console.log(obj2); // {'2': 'c', '3': 'd', 'length': 4}

var num = 123;
b = 10;
delete num; // false
// 一旦经历了 var 的操作，所得出的属性，window，这种属性叫做不可配置的属性。不可配置的属性 delete 不掉。
delete b; // true b 此时可以被删除掉

let obj3 = {};
obj3.num = 234;
delete obj3.num; // true
console.log(obj3.num); // undefined
window.num = 123;
delete window.num; // true
console.log(window.num); // undefined

[] + ''; // ''
[] + 1; // '1'
Number([]); // 0
[] - 1; // -1
{} + 1; // 1
String([]) + 1; // '1'
[] == []; // false 引用值比较地址是否相同
```

### 严格模式

变量赋值前必须声明，局部 this 必须被赋值。

```javascript
'use strict';
function test() {
    console.log(this); // 全局 this 指向 window
}
test(); // undefined
new test(); // test {} 的 constructor 指向
```

### 日期

let date = new Date(); date 记录了它出生时的年月日，时分秒，不会改变了。

setInterval：排列机制是基于红黑树的，并不是按照时间准确执行的，有差别。

let timer = setTnterval(function() { }, 1000); // timer 是一个数字，且是唯一的，是这个定时器的 id。

### JSON

Json 是一种传输数据的格式（以对象为样板，本质上就是对象，但用途有区别，对象就是本地用的，json 是用来传输的）。

Json.parse(); => string --> json 接收

Json.stringify(); => json --> string 发送

```json
{
    "name": "sky",
    "age": 123
}
```

传递给后端的是 JSON 字符串，

后端传递过来的也是 JSON 字符串。

eval("(" + jsonStr + ")"); ==> 将 Json 字符串转为 对象。

### 错误类型

- 语法解析错误（程序一行都不会执行）。
- 逻辑错误。执行到错误行时，程序停止。
- 一个 script 代码块里的错误，不会影响另一个 script 代码块的执行。

JS 执行队列 -> 轮转时间片，（类似于吃饭）

### 错误信息

Error.name 的六种值对应的信息：

1. EvalError: eval() 的使用与定义不一致
2. RangeError: 数值越界
3. ReferenceError：非法或不能识别的引用数值
4. SyntaxError：发生语法解析错误
5. TypeError：操作数据类型错误
6. URIError：URI处理函数使用不当

### 时间加载线

1. 创建 Document 对象，开始解析 web 页面。解析 HTML 元素和他们的文本内容后添加 Element 对象和 Text 节点到文档中。这个阶段 document.readyState = 'loading'。
2. 遇到 link 外部 css，创建线程加载，并继续解析文档。
3. 遇到 script 外部 js，并且没有设置 async、defer，浏览器加载，并阻塞，等待 js 加载完成并执行该脚本，然后继续解析文档。
4. 遇到 script 外部 js ，并且设置有 async、defer，浏览器创建线程加载，并继续解析文档。对于 async 属性的脚本，脚本加载完成后立即执行。（异步禁止使用 document.write()）
5. 遇到 img 等，先正常解析 dom 结构，然后浏览器异步加载 src，并继续解析文档。
6. 当文档解析完成，docment.readyState = 'interactive'。
7. 文档解析完成后，所有设置有 defer 的脚本会按照顺序执行。（注意与 async 的不同，但同样禁止使用 document.write()）
8. document 对象触发 DOMContentLoaded 事件，这也标志着 程序执行从同步脚本执行阶段，转化为事件驱动阶段。
9. 当所有 async 的脚本加载完成并执行后、img 等加载完成后，document.readyState = 'complete'，window 对象触发 load 事件。
10. 从此，以异步响应方式处理用户输入、网络事件等。

```javascript
console.log(document.readyState);

document.onreadystatechange = function() {
    console.log(document.readyState);
}

// DOMContentLoaded 只有这一种绑定的方式。
document.addEventListener('DOMContentLoaded', function() {
    console.log('aaa');
}, false);

window.onload = function () {
    console.log('加载完了');
}

// 打印的结果依次为：loading, interactive, aaa, complete, 加载完了

// jq 中 当 dom 解析完就执行的部分。比 window.onload 要提前。
$(document).ready(function() {
   // to do ... 
});
```



------



## ES6

### Symbol

新增 Symbol 数据类型（基本类型），Symbol 类型的值是通过 Symbol 函数调用生成的，相同 Symbol 函数返回的值是唯一的。

```javascript
let sy1 = Symbol('hell');
let sy2 = Symbol('hell');
console.log(sy1); // Symbol(hell)
console.log(typeof sy1); // symbol
console.log(sy1 === sy2); // false
```

如果 symbol 作为对象的 key，则该key for...in 遍历时，遍历不出来。

作用：

1. 属性私有化 - 数据保护



```javascript
let Person = (function() {
  let _gender = Symbol("gender");

  function P(name, gender) {
    this.name = name;
    this[_gender] = gender;
  }

  P.prototype.sayGender = function() {
    return this[_gender];
  };

  P.prototype.setGender = function(gender) {
    this[_gender] = gender;
  };

  return P;
})();

let p1 = new Person("sky", "男");
let p2 = new Person("小莫", "男");

console.log(p1);
console.log(p2);
console.log(p2.setGender("女"));
console.log(p2);
console.log(p2.sayGender());
```

### let, const

新增 作用域 (let, const)

let，const：

- 没有预解析，不存在变量提升。
- 同一个作用域里，不能重复定义变量。
- for 循环，for 循环里面是父作用域，里面又一个。

const 定义长量时，必须有值，不能不赋值，赋值完后不能修改。

### 解构赋值

允许按照一定模式，从数组和对象中提取值，并对变量进行赋值，这被称为 解构赋值。

* null 和 undefined 不能进行解构赋值
* 原始类型调用的是自己的包装类

```javascript
// 解构 null 和 undefined 的区别。
let [a, b, c='暂无'] = ['aa', 'bb', null];
console.log(a, b, c); // 'aa', 'bb', null

let [d, f, g='暂无'] = ['aa', 'bb', undefined];
console.log(d, f, g); // 'aa', 'bb', '暂无'

let [q, w, e='暂无'] = ['aa', 'bb'];
console.log(q, w, e); // 'aa', 'bb', '暂无'

let {a, b = 'bb'} = {a: 'aa', b: null};
console.log(a, b); // 'aa', null

let {a, b = 'bb'} = {a: 'aa', b: undefined};
console.log(a, b); // 'aa', 'bb'

let {a, b = 'bb'} = {a: 'aa'};
console.log(a, b); // 'aa', 'bb'

// 先定义变量，后解构
let a, b;
({a, b = 'bb'} = {a: 'aa'});
console.log(a, b); // 'aa', 'bb'
```

### 扩展运算符

含义：把迭代器里边的每一个迭代出来的对象数据通过逗号进行串联

```javascript
// 查找数组的最大值
let arr = [1, 3, 4, 5, 7, 9, 24];
console.log(Math.max(...arr));

// 合并数组 -- 开辟的是新空间
let arr2 = ['a', 'b', 'c'];
let arr3 = [...arr, ...arr2];
console.log(arr3); // [1, 3, 4, 5, 7, 9, 24, 'a', 'b', 'c']

let obj1 = { name: '小明', age: 18 };
let obj2 = { mail: '男' };

// 合并对象，相同的 key 会覆盖，后者覆盖前者 -- 开辟的是新空间
let obj3 = {
    ...obj1,
    ...obj2
};

console.log(obj3); // { name: '小明', age: 18, mail: '男' }
```

### 模板字符串

* 新增模板字符串 `` 反引号

- 数值扩展

    * 二进制    0b

    * 八进制    0o(ES6之前： 0 开头表示八进制)

    * 十进制    非 0 开头

    * 十六进制    0x开头

- 对象简写表示法

    当对象的 key 与对应的属性所引用的变量或函数同名的时候，可以简写成一个。

- 属性名表达式

    对象的属性名可以接收表达式做为 key，表达式计算的结果做为最终的 key。

### 迭代

* 迭代协议：规定了迭代与实现的逻辑

* 迭代器：具体的迭代实现逻辑 - [Symbol.iterator]

* 迭代对象：可被迭代的对象 - [Symbol.iterator] 方法。实现了迭代器的对象。

* 迭代语句

    for ... in：以原始插入的顺序迭代 对象 以及 对象原型链 上的所有可枚举属性 可用于 数组，对象

    for ... of：根据迭代对象的迭代器具体实现迭代对象数据 不可以用于 对象，因为对象没有可迭代的方法，不过可以手动实现。



```javascript
let arr = ["a", "b", "c"];
arr.names = "sku";
let k = Symbol("k");
arr[k] = "kkk"; // Symbol 属性不会被遍历出来。

for (const key in arr) {
  console.log(key, arr[key]);
  // 0 a
  // 1 b
  // 2 c
  // names sku
}

for (let attr of arr) {
  console.log(attr);
  // a
  // b
  // c
}

let obj2 = {
  c: 5,
  d: 6,
  [k]: "kkk"
};

// 实现迭代协议
obj2[Symbol.iterator] = () => {
  const keys = Object.keys(obj2); // 获取对象的所有 keys ['c', 'd']
  const len = keys.length;
  let n = 0;

  return {
    next() {
      if (n < len) {
        return {
          // value: keys[n++], // 每次循环得到 key 值
          // value: obj2[keys[n++]], // 每次循环得到 value 值
          value: {
            key: keys[n],
            value: obj2[keys[n++]]
          }, // 每次循环得到一个 对象，对象包括一个 key 和 key 对应 的 value
          done: false
        };
      } else {
        return {
          value: undefined,
          done: true
        };
      }
    }
  };
};

// obj[Symbol.iterator]().next() => {done: true} 终止
for (const iterator of obj2) {
  // obj2 必须是一个可迭代的对象。即：of 后边必须跟一个 可迭代的对象。
  console.log(iterator);
  // { key: 'c', value: 5 }
  // { key: 'd', value: 6 }
}
```

### 对象冻结

```javascript
const arr3 = Object.freeze([1, 2, 3]); // 冻结对象[1, 2, 3]
// arr3.push(4); // arr3 被冻结了，所以不可以再 push 内容了，如果 push 会报错
console.log(arr3);

console.log(Object.isFrozen(arr3)); // true 查看对象是否被冻结了, true or false 

// 数组里边嵌套数组， 里边嵌套的数组是不会被冻结的，可以用 递归实现 里边嵌套的数组也被冻结住
const arr4 = Object.freeze([1, 2, 3, [6, 8]]);
arr4[3].push(4); // push 不会报错,因为 没有使用递归冻结，所以不报错。
console.log(arr4);

console.log(Object.isFrozen(arr4));
```

### 函数扩展

1. 函数参数默认值

    ```javascript
    // 在形参中设置默认的值，如果形参中不是所有的参数都有默认值，那么有默认值的通常写在形参后面。
    function fn(x = 0, y = 0) {
        console.log(x, y);
    }
    fn(1, 3); // 1, 3
    fn(); // 0, 0
    
    function fn2(x, y = 0) { // y 写在参数列表的最后面
        console.log(x, y);
    }
    
    fn2(3); // 3, 0
    fn2(3, null); // 3, null
    
    function fn3(obj = {x: 1, y: 2}) {
        console.log(obj);
    }
    
    fn3(); // {x: 1, y: 2}
    fn3({x: 10, y: 20}); // {x: 10, y: 20}
    ```

2. rest 剩余参数

    ```javascript
    function arrayPush(arr, ...rest) {
        console.log(Array.isArray(rest)); // true
    
        // 1. 在原来的 空间上 push
    
        // rest.forEach(v => {
        //     arr.push(v);
        // })
        // return arr;
    
        // 2. 返回一个新的 空间
        return [...arr, ...rest];
    }
    
    let arr1 = [1, 2, 3];
    let arr3 = arrayPush(arr1, 'a', 'b', 'c');
    arr1.push(6);
    
    console.log(arr3); // [1, 2, 3, 'a', 'b', 'c']
    ```

### 箭头函数使用函数表达式

注意事项：

* 内部 this 对象指向创建初期上下文对象。箭头函数的 this 在函数创建期间就绑定好了，箭头函数的 this 指向 创建该箭头函数所在的作用域对象 (this)
* 不能作为构造函数
* 没有 arguments
* 不能作为生成器函数

### 内置对象

String, Number, Array, Object, Set, WeakSet, Map, WeakMap

Map与WeakMap 的区别，WeakMap 会有垃圾回收。Map 没有。

### Promise

ES6 中新增的异步编程解决方案，体现在代码中它是一个对象，可以通过 Promise 构造函数来实例化。

```javascript
typeof Promise.resolve('1'); // 'object'
```

#### 三种状态

- unresolved 等待任务完成（pending）
- resolved 任务完成，并且没有任何问题
- rejected 任务完成，但是出现问题



* 当 Promise 被实例化的时候，callback 的异步任务就会被执行
* 我们可以通过传入的 resolve, reject, 去改变当前 Promise 任务的状态
* resolve, reject 是两个函数，调用 resolve 这个函数，会把状态改成 resolved, 调用 reject 函数会把状态 改成 rejected
* Promise 对象下有一个方法： then, 该方法在 Promise 对象的状态发生改变的时候触发 then 的回调。then 方法默认返回 一个 promise 对象，并且该 promise 对象的默认状态 是成功的。
* 当对应的 promise 对象的状态变成了 resolved, 那么 then 的第一个 callback 就会被执行，如果 promise 对象的状态变成了 rejected 那么 then 的第二个 callback 就会被执行。
* catch 和 then 一样 也会返回一个默认的 resolved（成功的）状态的新的 promise 对象。

new Promise(cb) ===> 实例的基本使用 Pending Resolved Rejected
```javascript
// new Promise(cb)
// Pending (进行中) ===> Resolved (已完成)
// Pending (进行中) ===> Rejected (已失败)
```

两个原型方法：

```javascript
Promise.prototype.then()
Promise.prototype.catch()
```

```javascript
Promise.resolve(); // 立刻返回 成功的 promise 对象。
Promise.reject(); // 立刻返回 失败的 promise 对象。
```

#### 没有解决的问题

Promise 对后续的流程控制带来极大的麻烦！

```javascript
const p1 = new Promise((resolve, reject) => {
  console.log(1);
  setTimeout(() => {
    // resolve();
    reject();
  }, 1000);
});

// catch 和 then 一样 也会返回一个默认的 resolved（成功的）状态的新的 promise 对象。

// p1.then(
//   () => {
//     console.log("成功1.");
//   },
//   () => {
//     console.log("失败了1.");
//   }
// )
//   .then(() => {
//     console.log("成功2..");
//   })
//   .then(() => {
//     console.log("成功3...");
//   });

// p1.then(() => {
//   console.log("成功1.");
// })
//   .catch(err => {
//     console.log("失败了1.");
//   })
//   .then(() => {
//     console.log("成功2..");
//   })
//   .then(() => {
//     console.log("成功3...");
//   })

// 上边两种情况打印：1, 失败了1., 成功2.., 成功3...

// p1.then(() => {
//   console.log("成功1.");
// })
//   .then(() => {
//     console.log("成功2..");
//   })
//   .then(() => {
//     console.log("成功3...");
//   })
//   .catch(err => {
//     console.log("失败了1.");
//   });

// 这种情况打印：1, 失败了1.

p1.then(() => {
  console.log("成功1.");
})
  .catch(err => {
    console.log("失败了1.");
  })
  .then(() => {
    console.log("成功2..");
  })
  .then(() => {
    console.log("成功3...");
  })
  .catch(err => {
    console.log("失败了2.");
  });

// 上边两种情况打印：1, 失败了1., 成功2.., 成功3...
```

#### Promise.all & Promise.race

两个常用的静态方法：

```javascript
Promise.all();
Promise.race();
```

* Promise.all([p1, p2])

  两个都成功时才会执行 then 方法，返回 跟请求时顺序一样的成功的对象数组。

  但是只要有一个失败，立马执行 catch 方法，返回错误的那个对象。

* Promise.race([p1, p2])

  只要有一个请求返回时，就会立马执行 then 或者 catch 方法， 成功 执行 then 方法，失败执行 catch 方法。

#### 例子

```javascript
let p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('aaa');
  }, 1000);
});

let p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('bbb');
  }, 2000);
});

let p3 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('ccc');
  }, 3000);
});

// let p1 = Promise.reject('aa');
// let p2 = Promise.resolve('bb');
// let p3 = Promise.reject('cc');

Promise.all([p1, p2, p3])
  .then(res => {
    console.log('成功了1111');
    console.log(res);
  })
  .catch(err => {
    console.log('失败1111');
    console.log(err);
  });
// 打印：失败1111 bbb

// Promise.race([p1, p2, p3])
//   .then(res => {
//     console.log('成功了2222');
//     console.log(res);
//   })
//   .catch(err => {
//     console.log('失败22222');
//     console.log(err);
//   });
// 打印：失败22222 aaa
```

### Generator

generator 返回的是一个 迭代器函数。

generator 对后续的流程控制带来 便利。

co 函数：自动化 generator 函数调用器。自己实现的话，就递归调用 next 函数。

```javascript
// 例一
function* fn() {
	yield 3;
}

let f = fn();
let g = f.next();
console.log(g); // {value: 3, done: false}


// 例二
function* fn2() {
  console.log(1);
  
  let val = yield getData();
  console.log(val);
  
  console.log(3);
}

function getData() {
  setTimeout(() => {
    console.log(2);
    f2.next(4);
  }, 2000);
}

let f2 = fn2();
f2.next(); // 1, ...延迟2s..., 2, 4, 3


// 例三
function* fib(max) {
  let [a, b, n] = [0, 1, 0];

  while (n < max) {
    yield a;
    [a, b] = [b, a + b];
    n++;
  }
  return;
}

// let f = fib(5);
// console.log(f.next());

for (const x of fib(12)) {
  console.log(x);
}
```

### async & await

把异步的写法变成了同步的写法，但是本质还是异步编程。

await 后边跟的是 一个 promise 对象。

```javascript
const p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve(1);
    // reject();
  }, 1000);
});

async function fn() {
  console.time("aa");
  let arr = [];
  for (let i = 0; i < 500; i++) {
    let val = await p1;
    arr.push(val);
  }
  console.log(arr);
  console.timeEnd("aa");
  // arr => [1, ...498个1..., 1]
  // aa: 1008.8447265625ms
}

fn();
```

### class

class 没有定义提升功能。

```javascript
class Person {
    constructor(name) {
        this.name = name;
        this.showName = this.showName.bind(this); // 矫正 this
    }
    showName() {
        return `名字为：${this.name}`;
    }
}
let p1 = new Person(‘Strive’);
let { showName } = p1; // 不建议这么用
console.log(showName()); // 不用 bind this 会不存在，用了，this 指向实例对象。
```

### Web Workers

#### 介绍

Web Workers 是 H5 提供的一个 js 多线程解决方案。

我们可以将一些大计算量的代码交由 Web Worker 运行 而 不冻结用户界面。

但是子线程完全受主线程控制，且不得操作 DOM，所以，这个新标准并没有改变 js 单线程的本质。

#### 使用

创建在分线程执行的 js 文件

```javascript
function fibonacci(n) {
  return n <= 2 ? 1 : fibonacci(n - 1) + fibonacci(n - 2);
}

onmessage = event => {
  const number = event.data;
  console.log("分线程接收到主线程发送的数据： " + number);

  // 计算
  const result = fibonacci(number);

  // 通知主线程 计算的结果。
  postMessage(result);
  console.log("分线程向主线程返回数据：" + result);
};

```

在主线程中的 js 中发消息并设置回调

```html
<body>
  <input type="text" placeholder="输入数值" id='input'>
  <button id="btn">计算</button>


  <script>
    const input = document.getElementById('input');
    const btn = document.getElementById('btn');

    btn.onclick = () => {
      const number = input.value;

      // 创建一个 worker 对象
      const worker = new Worker('./worker.js');

      // 绑定接收消息的监听
      worker.onmessage = (event) => {
        console.log('主线程接收到分线程返回的数据：' + event.data);
      }

      // 向分线程发送消息
      worker.postMessage(number);
      console.log('主线程向分线程发送数据：' + number);
    }
  </script>
</body>
```

#### 不足

1. 慢。
2. 不能跨域加载 js，并且需要在服务器的环境下启动项目。
3. worker 内代码不能访问 DOM（更新 UI ）。
4. 不是每个浏览器都支持这个新特性。（目前支持的有：谷歌。）

#### 参考文章

- > <https://www.jianshu.com/p/9ea0801886f2>

------



## 函数式编程

### 函数时 JavaScript 的一等公民

所谓 “第一个公民”，指的是函数与其他数据类型一样，处于平等地位，可以赋值给其他变量，也可以作为参数，传入另一个函数，或者作为别的函数的返回值。

- sort
- map
- forEach
- ...



------



# JS面试题

## 语法

#### 闭包

```javascript
// 
function fun(n, k) {
    console.log(k);
    
    return {
        fun: function(m) {
            return fun(m, n);
        }
    }
}

let a = fun(0); a.fun(1); a.fun(2); a.fun(3); // undefined, 0, 0, 0
let b = fun(0).fun(1).fun(2).fun(3); // undefined, 0, 1, 2
let c = fun(0).fun(1); c.fun(2); c.fun(3); // undefined, 0, 1, 1
```

#### 作用域

```javascript
// 
let x = 10;
function fn() {
    console.log(x); // 10
}
function show(f) {
    let x = 20;
    f();
}
show(fn);

```



------

