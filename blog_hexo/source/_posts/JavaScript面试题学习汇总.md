---
title: JavaScript面试题学习汇总
date: 2023-02-06 09:47:14
tags: [前端, 闲聊分享, JavaScript]
categories:
  - 经验分享
---

# 前言

本篇文章主要总结一些常见的 HTML 面试题,会不断更新。

# Object.is() 与比较操作符 “===”、“==” 的区别？

- 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行**强制类型转化**后再进行比较。
- 使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型转换，直接返回 false。
- 使用`Object.is`来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如`-0`和`+0`不再相等，两个`NaN`是相等的。

# 判断数组的方式有哪些

- 通过`Object.prototype.toString.call()`做判断

  ```javascript
  Object.prototype.toString.call(obj).slice(8, -1) === "Array";
  ```

  > [`Object.prototype.toString.call()`原理](https://blog.csdn.net/WCBandCTZ/article/details/115536202)

- 通过原型链做判断

  ```javascript
  obj.__proto__ === Array.prototype;
  ```

- 通过 ES6 的`Array.isArray()`做判断

  ```javascript
  Array.isArray(obj); //用于确定传递的值是否是一个Array
  ```

- 通过`instanceof`做判断

  ```javascript
  obj instanceof Array;
  ```

- 通过`Array.prototype.isPrototypeOf(obj)`

  ```javascript
  Array.prototype.isPrototypeOf(obj); //用于测试一个对象(Array)是否存在于另一个对象(obj)的原型链上
  ```

# 数据类型检测的方式有哪些

**（1）typeof**

```javascript
console.log(typeof 2); // number
console.log(typeof true); // boolean
console.log(typeof "str"); // string
console.log(typeof []); // object
console.log(typeof function () {}); // function
console.log(typeof {}); // object
console.log(typeof undefined); // undefined
console.log(typeof null); // object
```

其中数组、对象、null 都会被判断为`Object`，其他判断都正确

**（2）instanceof**

`instanceof`可以正确判断对象的类型，**其内部运行机制是判断在其原型链中能否找到该类型的原型。**

```javascript
console.log(2 instanceof Number); // false
console.log(true instanceof Boolean); // false
console.log("str" instanceof String); // false

console.log([] instanceof Array); // true
console.log(function () {} instanceof Function); // true
console.log({} instanceof Object); // true
```

可以看到，`instanceof`**只能正确判断引用数据类型**，而不能判断基本数据类型。`instanceof`运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的`prototype`属性。

**（3）constructor**

```javascript
console.log((2).constructor === Number); // true
console.log(true.constructor === Boolean); // true
console.log("str".constructor === String); // true
console.log([].constructor === Array); // true
console.log(function () {}.constructor === Function); // true
console.log({}.constructor === Object); // true
```

`constructor`有两个作用，一是判断数据的类型，二是对象实例通过`constructor`对象访问它的构造函数。_需要注意的是，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了_：

```javascript
function Fn() {}

Fn.prototype = new Array();

var f = new Fn();

console.log(f.constructor === Fn); // false
console.log(f.constructor === Array); // true
```

**（4）Object.prototype.toString.call()**

`Object.prototype.toString.call()`使用`Object`对象的原型方法`toString`来判断数据类型：

```javascript
var a = Object.prototype.toString;

console.log(a.call(2)); // [object Number]
console.log(a.call(true)); // [object Boolean]
console.log(a.call("str")); // [object String]
console.log(a.call([])); // [object Array]
console.log(a.call(function () {})); // [object Function]
console.log(a.call({})); // [object Object]
console.log(a.call(undefined)); // [object Undefined]
console.log(a.call(null)); // [object Null]
```

同样是检测对象`obj`调用`toString`方法，`obj.toString()`的结果和`Object.prototype.toString.call(obj)`的结果不一样，这是为什么？

这是因为`toString`是`Object`的原型方法，而`Array`、`Function`等类型**作为 Object 的实例，都重写了 toString 方法**。不同的对象类型调用`toString`方法时，根据原型链的知识，调用的是对应的重写之后的`toString`方法（`Function`类型返回内容为函数体的字符串，`Array`类型返回元素组成的字符串...），而不会去调用`Object`上原型`toString`方法（返回对象的具体类型），所以采用`obj.toString()`不能得到其对象类型，只能将`obj`转换为字符串类型；因此，在想要得到对象的具体类型时，应该调用`Object`原型上的`toString`方法。

# null 和 undefined 区别

首先`undefined`和`null`都是基本数据类型，这两个基本数据类型分别都只有一个值，就是`undefined`和`null`。

`undefined`代表的含义是**未定义**，`null`代表的含义是**空对象**。一般变量声明了但还没有定义的时候会返回`undefined`，`null`主要用于赋值给一些可能会返回对象的变量，作为初始化。

`undefined` 在 JavaScript 中不是一个保留字，这意味着可以使用 `undefined` 来作为一个变量名，但是这样的做法是非常危险的，它会影响对 `undefined` 值的判断。我们可以通过一些方法获得安全的 `undefined` 值，比如说 void 0。

当对这两种类型使用 typeof 进行判断时，Null 类型化会返回 “object”，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

# typeof null 的结果是什么，为什么？

typeof null 的结果是 Object。

在 JavaScript 第一个版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的`类型标签（1-3bits）`以及当前要存储值的真实数据。类型标签存储在每个单元的低位中，共有 5 种类型：

```javascript
000: object   - 当前存储的数据指向一个对象。
  1: int      - 当前存储的数据是一个 31 位的有符号整数。
010: double   - 当前存储的数据指向一个双精度的浮点数。
100: string   - 当前存储的数据指向一个字符串。
110: boolean  - 当前存储的数据是布尔值。
```

如果最低位是 1，则类型标签标志位的长度只有一位；如果最低位是 0，则类型标签标志位的长度占三位，为存储其他四种数据类型提供了额外两个 bit 的长度。

有两种特殊数据类型：

- undefined 的值是 (-2)30(一个超出整数范围的数字)；
- null 的值是机器码 NULL 指针(null 指针的值全是 0)

那也就是说 null 的类型标签也是 000，和 Object 的类型标签一样，所以会被判定为 Object。

# 箭头函数和普通函数的区别

1. 语法更加简洁、清晰

2. 箭头函数**没有`prototype`（原型),所以箭头函数本身没有`this`**

3. **箭头函数不会创建自己的`this`**

   - 箭头函数没有自己的`this`,箭头函数的`this`指向在定义的时候继承自**外层第一个普通函数的`this`**

4. `call|apply|bind`无法改变箭头函数中`this`的指向

5. 箭头函数**不能作为构造函数使用**

6. 箭头函数**不绑定`arguments`,取而代之用`restl`参数`...`代替`arguments`对象，来访问箭头函数的参数列表**

7. 箭头函数不能用作`Generator`函数，不能使用`yield`关键字

# 数组有哪些原生方法？

- 数组转字符串：`toString()`、`toLocalString()`、`join()`,其中`join()`方法可以指定转换为字符串时的分隔符。

- 数组尾部操作：

  - `pop()`:从数组中删除最后一个元素，并返回该元素的值。此方法会更改数组的长度。
  - `push()`:将一个或多个元素添加到数组的末尾，**并返回该数组的新长度**

- 数组首部操作：

  - `shift()`:从数组中删除第一个元素，并返回该元素的值。此方法更改数组的长度。
  - `unshift()`:将一个或多个元素添加到数组的开头，并返回该数组的新长度

- 数组拼接：`concat()` 方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组

```javascript
const array1 = ["a", "b", "c"];
const array2 = ["d", "e", "f"];
const array3 = array1.concat(array2);

console.log(array3);
// Expected output: Array ["a", "b", "c", "d", "e", "f"]
```

- 数组截取: `slice()`,返回一个新的数组对象，这一对象是一个由 begin 和 end 决定的原数组的浅拷贝（包括 begin，不包括 end）。原始数组不会被改变

```javascript
const animals = ["ant", "bison", "camel", "duck", "elephant"];

console.log(animals.slice(2));
// Expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// Expected output: Array ["camel", "duck"]
```

- 数组插入方法 `splice()`,通过删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容。此方法会改变原数组

```javascript
const months = ["Jan", "March", "April", "June"];
//months.splice(1, 1, 'Feb');
// Inserts at index 1
console.log(months);
// Expected output: Array ["Jan", "Feb", "March", "April", "June"]

months.splice(4, 1, "May");
// Replaces 1 element at index 4
console.log(months);
// Expected output: Array ["Jan", "Feb", "March", "April", "May"]
```

- 数组元素查找 `indexOf`,返回在数组中可以找到给定元素的第一个索引，如果不存在，则返回 -1

```javascript
const beasts = ["ant", "bison", "camel", "duck", "bison"];

console.log(beasts.indexOf("bison"));
// Expected output: 1

// Start from index 2
console.log(beasts.indexOf("bison", 2));
// Expected output: 4

console.log(beasts.indexOf("giraffe"));
// Expected output: -1
```

- 数组元素查找 `lastIndexOf()`方法返回指定元素（也即有效的 JavaScript 值或变量）在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 fromIndex 处开始。

```javascript
const animals = ["Dodo", "Tiger", "Penguin", "Dodo"];

console.log(animals.lastIndexOf("Dodo"));
// Expected output: 3

console.log(animals.lastIndexOf("Tiger"));
// Expected output: 1
```

- 数组迭代方法：

  - `every()`测试一个数组内的所有元素是否都能通过某个指定函数的测试。它返回一个布尔值

  ```javascript
  const isBelowThreshold = (currentValue) => currentValue < 40;

  const array1 = [1, 30, 39, 29, 10, 13];

  console.log(array1.every(isBelowThreshold));
  // Expected output: true
  ```

  - `some()`测试数组中是不是至少有 1 个元素通过了被提供的函数测试。它返回的是一个 Boolean 类型的值。

  ```javascript
  const array = [1, 2, 3, 4, 5];

  // Checks whether an element is even
  const even = (element) => element % 2 === 0;

  console.log(array.some(even));
  // Expected output: true
  ```

  - `filter()` 方法创建给定数组一部分的浅拷贝，其包含通过所提供函数实现的测试的所有元素。

  ```javascript
  const words = [
    "spray",
    "limit",
    "elite",
    "exuberant",
    "destruction",
    "present",
  ];

  const result = words.filter((word) => word.length > 6);

  console.log(result);
  // Expected output: Array ["exuberant", "destruction", "present"]
  ```

  - `map()` 方法创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成

  ```javascript
  const array1 = [1, 4, 9, 16];

  // Pass a function to map
  const map1 = array1.map((x) => x * 2);

  console.log(map1);
  // Expected output: Array [2, 8, 18, 32]
  ```

  - `forEach()`方法对数组的每个元素执行一次给定的函数。

  ```javascript
  const array1 = ["a", "b", "c"];

  array1.forEach((element) => console.log(element));
  ```

  - `reduce()`方法对数组中的每个元素按序执行一个由您提供的 reducer 函数，每一次运行 reducer 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值

  > 详细解释见[mdn 官网](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

# 什么是 DOM 和 BOM?

- DOM 指的是文档对象模型，它指的是把文档当做一个对象，这个对象主要定义了处理网页内容的方法和接口。

- BOM 指的是浏览器对象模型，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口
  。BOM 的核心是 window，而 window 对象具有双重角色，它既是通过 js 访问浏览器窗口的一个接口，又是一个 Global
  （全局）对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。window 对象
  含有 location 对象、navigator 对象、screen 对象等子对象，并且 DOM 的最根本的对象 document 对象也是 BOM 的 window 对象的子对象。

# 对类数组对象的理解，如何转化为数组

一个拥有 length 属性和若干索引属性的对象就可以被称为类数组对象，类数组对象和数组类似，但是不能调用数组的方法。
常见的类数组对象有 arguments 和 DOM 方法的返回结果，函数参数也可以被看作是类数组对象，因为它含有 length 属性
值，代表可接收的参数个数。

常见的类数组转换为数组的方法：

- 通过 call 调用数组的 slice 方法来实现转换

```javascript
Array.prototype.slice.call(arrayLike);
```

- 通过 call 调用数组的 splice 方法来实现转换

```javascript
Array.prototype.splice.call(arrayLike, 0);
```

- 通过 apply 调用数组的 concat 方法来实现转换

```javascript
Array.prototype.concat.apply([], arrayLike);
```

- 通过 Array.from 方法来实现转换

```javascript
Array.from(arrayLike);
```

# JavaScript 为什么要进行变量提升，它导致了什么问题？

（1）提高性能

在 JS 代码执行之前，会进行语法检查和预编译，并且这一操作只进行一次。这么做就是为了提高性能，如果没有这一步
，那么每次执行代码前都必须重新解析一遍该变量（函数），而这是没有必要的，因为变量（函数）的代码并不会改变，解析一遍就够了。
在解析的过程中，还会为函数生成预编译代码。在预编译时，会统计声明了哪些变量、创建了哪些函数，并对函数的代码
进行压缩，去除注释、不必要的空白等。这样做的好处就是每次执行函数时都可以直接为该函数分配栈空间（不需要再
解析一遍去获取代码中声明了哪些变量，创建了哪些函数），并且因为代码压缩的原因，代码执行也更快了。

（2）容错性更好

变量提升可以在一定程度上提高 JS 的容错性，看下面的代码：

```javascript
a = 1;
var a;
console.log(a);
```

如果没有变量提升，这两行代码就会报错，但是因为有了变量提升，这段代码就可以正常执行。

虽然，在可以开发过程中，可以完全避免这样写，但是有时代码很复杂的时候。可能因为疏忽而先使用后定义了，

这样也不会影响正常使用。由于变量提升的存在，而会正常运行。

**总结：**

- 解析和预编译过程中的变量提升可以提高性能，让函数可以在执行时预先为变量分配栈空间

- 变量提升还可以提高 JS 代码的容错性，使一些不规范的代码也可以正常执行

变量提升虽然有一些优点，但是他也会造成一定的问题，在 ES6 中提出了 let、const 来定义变量，它们就没有变量提升的机制。下面看一下变量提升可能会导致的问题：

```javascript
var tmp = new Date();
function fn() {
  console.log(tmp);
  if (false) {
    var tmp = "hello world";
  }
}
fn(); // undefined
```

在这个函数中，原本是要打印出外层的 tmp 变量，但是因为变量提升的问题，内层定义的 tmp 被提到函数内部的最顶部，相当于覆盖了外层的 tmp，所以打印结果为 undefined。

```javascript
var tmp = "hello world";
for (var i = 0; i < tmp.length; i++) {
  console.log(tmp[i]);
}
console.log(i); // 11
```

由于遍历时定义的 i 会变量提升成为一个全局变量，在函数结束之后不会被销毁，所以打印出来 11。

# 原型与原型链

## 原型链的终点是什么？如何打印出原型链的终点？

由于 Object 是构造函数，原型链终点是 Object.prototype.**proto**，而 Object.prototype.**proto**=== null // true，
所以，原型链的终点是 null。原型链上的所有原型都是对象，所有的对象最终都是由 Object 构造的，而 Object.prototype 的
下一级是 Object.prototype.**proto**。

## 如何获得对象非原型链上的属性？

使用`hasOwnProperty()`方法来判断属性是否属于原型链的属性：

```javascript
function iterate(obj) {
  var res = [];
  for (var key in obj) {
    if (obj.hasOwnProperty(key)) res.push(key + ": " + obj[key]);
  }
  return res;
}
```

# 垃圾回收与内存泄漏

## 哪些情况会导致内存泄漏

以下四种情况会造成内存的泄漏：

- 意外的全局变量： 由于使用未声明的变量，而意外的创建了一个全局变量，而使这个变量一直留在内存中无法被回收。

- 被遗忘的计时器或回调函数： 设置了 setInterval 定时器，而忘记取消它，如果循环函数有对外部变量的引用的话，那么这个变量会被一直留在内存中，而无法被回收。

- 脱离 DOM 的引用： 获取一个 DOM 元素的引用，而后面这个元素被删除，由于一直保留了对这个元素的引用，所以它也无法被回收。

- 闭包： 不合理的使用闭包，从而导致某些变量一直被留在内存当中。
