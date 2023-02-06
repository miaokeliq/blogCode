---
title: JavaScript面试题学习汇总
date: 2023-02-06 09:47:14
tags: [前端,闲聊分享,JavaScript]
categories: 
  - 经验分享
---

# 前言

本篇文章主要总结一些常见的HTML面试题,会不断更新。

# Object.is() 与比较操作符 “===”、“==” 的区别？

- 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行**强制类型转化**后再进行比较。
- 使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型转换，直接返回false。
- 使用`Object.is`来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如`-0`和`+0`不再相等，两个`NaN`是相等的。

# 判断数组的方式有哪些

- 通过`Object.prototype.toString.call()`做判断

  ``````javascript
  Object.prototype.toString.call(obj).slice(8,-1) === 'Array';
  ``````

  > [`Object.prototype.toString.call()`原理](https://blog.csdn.net/WCBandCTZ/article/details/115536202)

- 通过原型链做判断

  ``````javascript
  obj.__proto__ === Array.prototype;
  ``````

- 通过ES6的`Array.isArray()`做判断

  ``````javascript
  Array.isArray(obj); //用于确定传递的值是否是一个Array
  ``````

- 通过`instanceof`做判断

  ``````javascript
  obj instanceof Array
  ``````

- 通过`Array.prototype.isPrototypeOf(obj)`

  ``````javascript
  Array.prototype.isPrototypeOf(obj) //用于测试一个对象(Array)是否存在于另一个对象(obj)的原型链上
  ``````

  # 数据类型检测的方式有哪些

  **（1）typeof**

``````javascript
console.log(typeof 2);               // number
console.log(typeof true);            // boolean
console.log(typeof 'str');           // string
console.log(typeof []);              // object    
console.log(typeof function(){});    // function
console.log(typeof {});              // object
console.log(typeof undefined);       // undefined
console.log(typeof null);            // object
``````

其中数组、对象、null都会被判断为`Object`，其他判断都正确

**（2）instanceof**

`instanceof`可以正确判断对象的类型，**其内部运行机制是判断在其原型链中能否找到该类型的原型。**

``````javascript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
``````

可以看到，`instanceof`**只能正确判断引用数据类型**，而不能判断基本数据类型。`instanceof`运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的`prototype`属性。

**（3）constructor**

``````javascript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
``````

`constructor`有两个作用，一是判断数据的类型，二是对象实例通过`constructor`对象访问它的构造函数。*需要注意的是，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了*：

``````javascript
function Fn(){};

Fn.prototype = new Array();

var f = new Fn();

console.log(f.constructor === Fn); // false
console.log(f.constructor === Array); // true
``````

**（4）Object.prototype.toString.call()**

`Object.prototype.toString.call()`使用`Object`对象的原型方法`toString`来判断数据类型：

``````javascript
var a = Object.prototype.toString;
 
console.log(a.call(2)); // [object Number]
console.log(a.call(true)); // [object Boolean]
console.log(a.call('str')); // [object String]
console.log(a.call([]));	// [object Array]
console.log(a.call(function(){}));// [object Function]
console.log(a.call({}));// [object Object]
console.log(a.call(undefined));// [object Undefined]
console.log(a.call(null));// [object Null]
``````

同样是检测对象`obj`调用`toString`方法，`obj.toString()`的结果和`Object.prototype.toString.call(obj)`的结果不一样，这是为什么？

这是因为`toString`是`Object`的原型方法，而`Array`、`Function`等类型**作为Object的实例，都重写了toString方法**。不同的对象类型调用`toString`方法时，根据原型链的知识，调用的是对应的重写之后的`toString`方法（`Function`类型返回内容为函数体的字符串，`Array`类型返回元素组成的字符串...），而不会去调用`Object`上原型`toString`方法（返回对象的具体类型），所以采用`obj.toString()`不能得到其对象类型，只能将`obj`转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用`Object`原型上的`toString`方法。

# null和undefined区别

首先`undefined`和`null`都是基本数据类型，这两个基本数据类型分别都只有一个值，就是`undefined`和`null`。

`undefined`代表的含义是**未定义**，`null`代表的含义是**空对象**。一般变量声明了但还没有定义的时候会返回`undefined`，`null`主要用于赋值给一些可能会返回对象的变量，作为初始化。

`undefined` 在 JavaScript 中不是一个保留字，这意味着可以使用 `undefined` 来作为一个变量名，但是这样的做法是非常危险的，它会影响对 `undefined` 值的判断。我们可以通过一些方法获得安全的 `undefined` 值，比如说 void 0。

当对这两种类型使用 typeof 进行判断时，Null 类型化会返回 “object”，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

# typeof null的结果是什么，为什么？

typeof null 的结果是 Object。

在JavaScript第一个版本中，所有值都存储在32位的单元中，每个单元包含一个小的`类型标签（1-3bits）`以及当前要存储值的真实数据。类型标签存储在每个单元的低位中，共有5种类型：

``````javascript
000: object   - 当前存储的数据指向一个对象。
  1: int      - 当前存储的数据是一个 31 位的有符号整数。
010: double   - 当前存储的数据指向一个双精度的浮点数。
100: string   - 当前存储的数据指向一个字符串。
110: boolean  - 当前存储的数据是布尔值。
``````

如果最低位是 1，则类型标签标志位的长度只有一位；如果最低位是 0，则类型标签标志位的长度占三位，为存储其他四种数据类型提供了额外两个 bit 的长度。

有两种特殊数据类型：

- undefined的值是 (-2)30(一个超出整数范围的数字)；
- null 的值是机器码 NULL 指针(null 指针的值全是 0)

那也就是说null的类型标签也是000，和Object的类型标签一样，所以会被判定为Object。

