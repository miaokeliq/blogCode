---
title: JavaScript手写代码题汇总
date: 2023-02-23 15:11:22
tags: [前端, 闲聊分享, JavaScript]
categories:
  - 经验分享
---

# 前言

主要记录在准备秋招的过程中遇到的前端手写代码题，因为我也是个学生，我在看其他博客总结的手写题时会有一些难以理解的点但博客里并没有说明，所以我在解决每一个问题就会记录在这篇博客，也许我之前不明白的问题刚好也是你不明白的问题，所以这篇博客适合校招基础不扎实的同学学习，如果觉得对你有帮助，麻烦点个 star，谢谢！

> 本文引用博客[高频前端面试题汇总](https://juejin.cn/post/6946136940164939813#heading-14),
> 并以我的理解解释了代码中一些不太容易理解的点,方便同学们能够更好地理解每一道题。

# JavaScript 基础

## 手写 `Object.create`

思路：将传入的对象作为原型

```javascript
function create(obj) {
  function F() {}
  F.prototype = obj;
  return new F();
}
```

## 手写 instanceof 方法

instanceof 运算符用于判断构造函数的`prototype`属性是否出现在对象的原型链中的任何位置。

`instanceof`主要的实现原理就是右边变量的`prototype`在左边变量的原型链上即可。`instanceof`在查找的过程中会遍历左边变量的原型链，直到找到右边变量的`prototype`,如果查找失败，则会返回 false，告诉我们左边变量并非是右边变量的实例。

实现步骤：

1. 首先获取类型的原型
2. 然后获取对象的原型
3. 然后一直循环判断对象的类型是否等于类型的原型，知道对象原型为`null`，因为原型链最终为`null`

具体代码：

```javascript
function new_instance_of(leftValue, rightValue) {
  let rightProto = rightValue.prototype; // 取右表达式的 prototype 值
  leftValue = leftValue.__proto__; // 取左表达式的 __proto__ 值
  while (true) {
    if (leftValue === null) {
      return false;
    }
    if (leftValue === rightProto) {
      return true;
    }
    leftValue = leftValue.__proto__;
  }
}
```

以下是一些例子：

```javascript
function Foo() {}

Object instanceof Object; // true
Function instanceof Function; // true
Function instanceof Object; // true
Foo instanceof Foo; // false
Foo instanceof Object; // true
Foo instanceof Function; // true
```

## 手写 new 操作符

在调用 `new` 的过程中会发生以下四件事情：

（1）首先创建了一个新的空对象
（2）设置原型，将对象的原型设置为函数的`prototype`对象
（3）让函数的`this`指向这个对象，执行构造函数的代码（为这个新对象添加属性）
（4）判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，则返回这个引用类型的对象

```javascript
      function _new(ctor) {
        if (typeof ctor !== "function") {
          throw "newOperator function the first param must be a function";
        }
        // ES6 new.target 是指向构造函数
        newOperator.target = ctor;
        // 1.创建一个全新的对象，
        // 2.并且执行[[Prototype]]链接
        // 4.通过`new`创建的每个对象将最终被`[[Prototype]]`链接到这个函数的`prototype`对象上。
        var newObj = Object.create(ctor.prototype);
        // ES5 arguments转成数组 当然也可以用ES6 [...arguments], Aarry.from(arguments);
        // 除去ctor构造函数的其余参数
        var argsArr = [].slice.call(arguments, 1);
        // 3.生成的新对象会绑定到函数调用的`this`。
        // 获取到ctor函数返回结果
        var ctorReturnResult = ctor.apply(newObj, argsArr);
        // 小结4 中这些类型中合并起来只有Object和Function两种类型 typeof null 也是'object'所以要不等于null，排除null
        var isObject =
          typeof ctorReturnResult === "object" && ctorReturnResult !== null;
        var isFunction = typeof ctorReturnResult === "function";
        if (isObject || isFunction) {
          return ctorReturnResult;
        }
        // 5.如果函数没有返回对象类型`Object`(包含`Functoin`, `Array`, `Date`, `RegExg`, `Error`)，那么`new`表达式中的函数调用会自动返回这个新的对象。
        return newObj;
```

## 手写 防抖 代码

```javascript
function debounce(func, delay) {
  let timer;
  return function () {
    clearTimeout(timer);
    // console.log("this", this);
    let context = this;
    // console.log("args", arguments);
    let args = arguments;
    timer = setTimeout(function () {
      // console.log(arguments) // 这个函数没有默认传进来的arguments，因为它并不是被监听事件调用的。
      // console.log(this); // this指向windows
      func.apply(context, args);
    }, delay);
  };
}
```

## 手写 节流 代码

```javascript
function throttle(func, delay) {
  let pre = 0;
  return function () {
    let now = new Date();
    let context = this;
    let args = arguments;
    if (now - pre > delay) {
      func.apply(context, args);
      pre = now;
    }
  };
}
```

## 手写 call 函数

`call`函数的实现步骤：

1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用`call`等方式调用的情况。

2. 判断传入上下文对象`context`对象是否存在，如果不存在，则设置为`window`。

3. 处理传入的参数，截取第一个参数后的所有参数。

4. 将函数作为上下文对象的一个属性。

5. 使用上下文对象来调用这个方法，并保存返回结果。

6. 删除刚才新增的属性。

7. 返回结果。

```javascript
Function.prototype.myCall = function (context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
  }
  // 获取参数
  let args = [...arguments].slice(1);
  let result = null;
  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;
  // 将调用函数设为对象的方法
  context.fn = this;
  // 调用函数
  result = context.fn(...args);
  // 将属性删除
  delete context.fn;
  return result;
};
```

## 手写 apply 函数

`apply` 函数的实现步骤：

1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用`call`等方式的调用。

2. 判断传入上下文对象是否存在，如果不存在，设置为`window`。

3. 将函数作为上下文对象的一个属性。

4. 判断参数值是否传入

5. 使用上下文对象来调用这个方法，并保存返回结果。

6. 删除刚才新增的属性。

7. 返回结果。

```javascript
Function.prototype.myApply = function (context) {
  if (typeof context !== "function") {
    console.error("Error");
  }
  let result = null;
  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;
  // 将函数设为对象的方法
  context.fn = this;
  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn;
  }

  // 将属性删除
  delete context.fn;
  return result;
};
```

## 手写 bind 函数

`bind` 函数的实现步骤：

1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用`call`等方式的调用。

2. 保存当前函数的引用，获取其余传入参数值。

3. 创建一个函数返回

4. 函数内部使用`apply`来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的`this`给`apply`调用，其余情况都传入指定的上下文对象。

```javascript
Function.prototype.myBind = function (context) {
  // 判断调用对象是否为函数
  if (typeof context !== "function") {
    console.error("Error");
  }

  // 获取参数
  let args = [...arguments].slice(1);
  let fn = this;
  return function Fn() {
    // 根据调用方式，传入不同绑定值
    return fn.apply(
      this instanceof Fn ? this : context,
      args.concat(...arguments)
    );
  };
};
```

**为什么会有第四条的判断呢?**接下来我们举个例子：

假设有这样一个构造函数：

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
```

现在我们想创建一个新的 Person 对象，并把它绑定到一个特定的上下文对象上：

```javascript
const context = { city: "Beijing" };
const fn = Person.myBind(context, "Alice", 20);
const person = new fn();
```

在这个例子中，我们使用 myBind 函数将 Person 函数绑定到 context 对象上，并传入 "Alice" 和 20 两个参数。接着，我们创建一个新的函数 fn，并使用 new 关键字调用它，创建了一个新的 Person 对象 person。

由于在这里使用了 new 关键字，我们期望 person 对象的上下文是它本身。因此，如果在绑定函数中使用 context 作为上下文对象，那么 person 对象将无法正确地被创建。

因此，在这个例子中，使用 this instanceof Fn ? this : context 来判断应该使用哪个上下文对象。如果 this 指向新创建的对象，就使用它自己作为上下文对象；否则，就使用传入的 context 作为上下文对象。这就保证了 person 对象的正确创建，以及绑定函数的灵活性。

**为什么 context 会多出 name 和 age 属性呢？**

如果最后不是 new 一个新的 person，而是直接`fn()`

在 myBind 函数中，我们使用 args.concat(...arguments) 将绑定函数 fn 的参数列表和调用 fn() 函数时传入的参数合并为一个新的参数数组。在调用 fn() 函数时，我们并没有传入任何参数，所以 args.concat(...arguments) 返回的数组中只包含了两个元素："Alice" 和 20。

在绑定函数 fn 的执行过程中，args 数组中的两个元素 "Alice" 和 20 将被作为 Person 函数的参数，然后传递给 Person 函数进行调用。因此，Person 函数的执行结果将会返回一个对象，该对象具有 name 和 age 两个属性，这两个属性的值分别为 "Alice" 和 20。

当我们在调用 fn() 函数时，实际上是在调用 Person 函数，并且在调用时使用了 context 对象作为 this 对象。因此，在 Person 函数中，this 对象将会指向 context 对象。在 Person 函数中，我们为 this 对象添加了两个属性 name 和 age，因此在 context 对象中也会包含这两个属性，并且它们的值分别为 "Alice" 和 20。

## 实现深拷贝

- 浅拷贝： 浅拷贝指的是将一个对象的属性值复制到另一个对象，如果有的属性的值为引用类型的话，
  那么会将这个引用的地址复制给对象，因此两个对象会有同一个引用类型的引用。浅拷贝可以使用  
  Object.assign 和展开运算符来实现。

- 深拷贝： 深拷贝相对浅拷贝而言，如果遇到属性值为引用类型的时候，它新建一个引用类型并将对
  应的值复制给它，因此对象获得的一个新的引用类型而不是一个原有类型的引用。深拷贝对于一些对
  象可以使用 JSON 的两个函数来实现，但是由于 JSON 的对象格式比 js 的对象格式更加严格，
  所以如果属性值里边出现函数或者 Symbol 类型的值时，会转换失败

1. JSON.stringify()

- `JSON.parse(JSON.stringify(obj))`是目前比较常用的深拷贝方法之一，它的原理就是利用`JSON.stringify`将`js`对象序列化（JSON 字符串）,再使用`JSON.parse`来反序列化（还原）`js`对象。

- 这个可以简单粗暴的实现深拷贝，但是还存在问题，拷贝的对象中如果有函数，`undefined`，`symbol`，当使用过`JSON.stringify()`进行处理之后，都会消失。

```javascript
let obj1 = {
  a: 0,
  b: {
    c: 0,
  },
};
let obj2 = JSON.parse(JSON.stringify(obj1));
obj1.a = 1;
obj1.b.c = 1;
console.log(obj1); // {a: 1, b: {c: 1}}
console.log(obj2); // {a: 0, b: {c: 0}}
```

2. 函数库 lodash 的\_.cloneDeep 方法

该函数库也有提供\_.cloneDeep 用来做 Deep Copy

```javascript
var _ = require("lodash");
var obj1 = {
  a: 1,
  b: { f: { g: 1 } },
  c: [1, 2, 3],
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f); // false
```

3. 手写实现深拷贝函数

```javascript
// 深拷贝的实现
function deepCopy(object) {
  if (!object || typeof object !== "object") return;

  let newObject = Array.isArray(object) ? [] : {};

  for (let key in object) {
    if (object.hasOwnProperty(key)) {
      newObject[key] =
        typeof object[key] === "object" ? deepCopy(object[key]) : object[key];
    }
  }
  return newObject;
}
```

for-in 循环会枚举对象自身及其原型链上的所有可枚举属性，因此在循环遍历对象的属性时，

我们通常需要使用 object.hasOwnProperty(key) 来过滤掉原型链上的属性，只保留对象本

身拥有的属性。

在这个 deepCopy 函数中，使用 object.hasOwnProperty(key) 来确保只复制对象自身的属性

。如果不使用这个判断，那么在遍历对象时，除了复制对象自身的属性，还会复制其原型链

上的属性，导致最终的结果可能不符合预期。

## 手写 Promise.all

`Promise.all`用来处理多个并发请求，为了页面数据构造的方便，将一个页面所用到的在不同接口的数据一起请求过来，如果其中一个接口失败了，
多个请求也就失败了。

```javascript
function promiseAll(promises) {
  return new Promise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      throw new TypeError("argument must be a array");
    }
    let resolvedCounter = 0;
    let promiseNum = promises.length;
    let resolvedResult = [];
    for (let i = 0; i < promiseNum; i++) {
      Promise.resolve(promises[i]).then(
        (res) => {
          resolvedCounter++;
          resolvedResult[i] = res;
          if (resolvedCounter === promiseNum) {
            resolve(resolvedResult);
          }
        },
        (error) => {
          return reject(error);
        }
      );
    }
  });
}

// test
let p1 = new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve(1);
  }, 1000);
});
let p2 = new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve(2);
  }, 2000);
});
let p3 = new Promise(function (resolve, reject) {
  setTimeout(function () {
    resolve(3);
  }, 3000);
});
promiseAll([p3, p1, p2]).then((res) => {
  console.log(res); // [3, 1, 2]
});
```

# 数据处理

## 实现数组的乱序输出

主要的实现思路就是：

- 取出数组的第一个元素，随机产生一个索引值，将该第一个元素和这个索引对应的元素进行交换。

- 第二次取出数据数组第二个元素，随机产生一个除了索引为 1 的之外的索引值，并将第二个元素与该索引值对应的元素进行交换

- 按照上面的规律执行，直到遍历完成。

```javascript
function shuffleArray(arr) {
  for (let i = arr.length - 1; i >= 0; i--) {
    // Math.floor() 总是返回小于等于一个给定数字的最大整数
    const randomIndex = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[randomIndex]] = [arr[randomIndex], arr[i]];
  }
  return arr;
}

const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const shuffledArr = shuffleArray(arr);
console.log(shuffledArr);
```

`Math.random() * (i + 1) `生成一个 0 到 i+1 之间的随机小数，因为在第 i 轮迭代中，前 i 个元素已经随机交换并放在数组的前面位置，因此下一个随机选取的元素只能从剩余的 n-i 个元素中选择，所以要在 n-i 个元素中随机生成一个索引，而 Math.random() \_ (i + 1) 可以保证随机生成的索引在 0 到 i+1 的范围内，从而实现了在剩余元素中随机选取一个元素的目的。

**生成的随机小数包括 i+1 吗?**

不包括 i+1。Math.random() 生成的随机数是一个大于等于 0 且小于 1 的小数，因此乘以 (i+1) 后得到的随机数范围是 0 到 i+1 之间的一个小数，但不包括 i+1。因为数组的下标是从 0 开始的，所以最后的随机下标应该在 0 到 i 之间，包括 0 和 i。

## 交换 a,b 的值，不能用临时变量

1. 巧妙的利用两个数的和、差：

```javascript
a = a + b;
b = a - b;
a = a - b;
```

2. ES6 解构赋值

```javascript
[a, b] = [b, a];
```

## 实现数组的扁平化

1. ES6 的 flat

使用 ES6 的 `flat` 方法来实现数组扁平化。 `flat` 方法的语法：

`arr.flat([depth])`

其中 depth 是 flat 的参数，depth 是可以传递数组的展开深度（默认不填、数值是 1），即展开一层数组。如果层数不确定，参数可以传进 Infinity，代表不论多少层都要展开：

```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
  return arr.flat(Infinity);
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```

可以看出，一个嵌套了两层的数组，通过将 flat 方法的参数设置为 Infinity，达到了我们预期的效果。其实同样也可以设置成 2，也能实现这样的效果。在编程过程中，如果数组的嵌套层数不确定，最好直接使用 Infinity，可以达到扁平化。

2. `split` 和 `toString`

可以通过 `split` 和 `toString` 两个方法来共同实现数组扁平化，由于数组会默认带一个 `toString` 的方法，所以可以把数组直接转换成逗号分隔的字符串，然后在用 `split` 方法把字符串重新转换为数组，如下面的代码所示：

```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
  //return arr.toString(); // 1,2,3,4
  return arr.toString().split(",");
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```

通过这两个方法可以将多维数组直接转换成逗号连接的字符串，然后再重新分隔成数组。

3. 扩展运算符实现

采用扩展运算符和 `some` 的方法，达到数组扁平化的目的：

```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
  while (arr.some((item) => Array.isArray(item))) {
    arr = [].concat(...arr);
  }
  return arr;
}
console.log(flatten(arr)); //  [1, 2, 3, 4，5]
```

4. `reduce` 函数迭代

用`reduce`来实现数组的拼接，从而简化递归的代码

```javascript
let arr = [1, [2, [3, 4]]];
function flatten(arr) {
  return arr.reduce(function (prev, next) {
    return prev.concat(Array.isArray(next) ? flatten(next) : next);
  }, []);
}
console.log(flatten(arr)); //  [1, 2, 3, 4]
```

在这个例子中，reduce 的第二个参数([])是用来传递累加器的初始值，也就是函数第一个参数 prev 的初始值。在这个例子中，我们希望最终返回一个扁平化的数组，因此我们的累加器初始值应该是一个空数组。这个空数组会在每一次迭代中作为 prev 的初始值传入，然后将 prev 和下一个元素(next)合并后返回一个新的数组，这个新的数组又会在下一次迭代中作为 prev 的值被传入，如此往复，直到 reduce 结束。最终，我们得到的就是一个扁平化的数组。

## 实现数组去重

给定某无序数组，要求去除数组中的重复数字并且返回新的无重复数组。

ES6 方法（使用数据结构集合）

```javascript
const array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];

Array.from(new Set(array)); // [1, 2, 3, 5, 9, 8]
```

ES5 方法：使用 map 存储不重复的数字

```javascript
const array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];

let res = uniqueArray(array);

function uniqueArray(arr) {
  let map = {};
  let res = [];
  for (let item of arr) {
    if (!map.hasOwnProperty(item)) {
      map[item] = 1;
      res.push(item);
    }
  }

  return res;
}
console.log(res); // 1, 2, 3, 5, 9, 8
```

## 实现数组的 map 方法

`map()`方法创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成

```javascript
Array.prototype._map = function (fn) {
  if (typeof fn !== "function") {
    throw Error("参数必须是一个函数");
  }

  let res = [];
  for (let i = 0; i < this.length; i++) {
    res.push(fn(this[i]));
  }

  return res;
};

let arr = [1, 2, 3, 4, 5, 6];

let arr1 = arr._map((item) => {
  return item + 1;
});
console.log(arr1); // 2, 3, 4, 5, 6, 7
```

这个太简单，看看上面的代码就能懂了～

## 实现数组的 filter 方法

`filter()` 方法创建给定数组一部分的浅拷贝，其包含通过所提供函数来实现的测试的所有元素

```javascript
Array.prototype._filter = function (fn) {
  if (typeof fn !== "function") {
    throw Error("参数必须是一个函数");
  }

  let res = [];
  for (let i = 0; i < this.length; i++) {
    let itemRes = fn(this[i]);
    itemRes && res.push(this[i]);
  }

  return res;
};
const words = [
  "spray",
  "limit",
  "elite",
  "exuberant",
  "destruction",
  "present",
];

const result = words._filter((word) => word.length > 6);

console.log(result); //Array ["exuberant", "destruction", "present"]
```

这个太简单，看看上面的代码就能懂了～

## 实现数组的 push 方法

`push`方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度。

```javascript
Array.prototype._push = function (item) {
  for (let i = 0; i < arguments.length; i++) {
    this[this.length] = arguments[i];
  }
  return this.length;
};

const animals = ["pigs", "goats", "sheep"];
let length = animals._push("chickens", "cats", "dogs");
console.log(length); // 6
console.log(animals); //Array ["pigs", "goats", "sheep", "cows", "chickens", "cats", "dogs"]
```

## 实现字符串的 repeat 方法

`repeart()`构造并返回一个新字符串，该字符串包含被连接在一起的指定数量的字符串的副本

```javascript
function repeat(s, n) {
  return new Array(n + 1).join(s);
}
let str = "abc";
let res = repeat(str, 2);
console.log(res);
```

## 实现字符串的翻转

```javascript
function _reverse(a) {
  return a.split("").__reverse().join("");
}

Array.prototype.__reverse = function () {
  console.log(this);
  if (!Array.isArray(this)) {
    throw Error("参数必须是一个函数");
  }
  for (let i = 0, j = this.length - 1; i < j; i++, j--) {
    [this[i], this[j]] = [this[j], this[i]];
  }
  return this;
};

let a = "abcdefg";
console.log(_reverse(a)); // gfedcba
```

## 使用 ES5 和 ES6 求函数参数的和

- ES5

```javascript
// es5
function sum() {
  let sum = 0;
  Array.prototype.forEach.call(arguments, function (item) {
    sum += item * 1; // *1 是为了将item强制转换为 number 类型
  });
  return sum;
}

// es6
function sum(...nums) {
  let sum = 0;
  nums.forEach((item) => {
    sum += item * 1;
  });

  return sum;
}

console.log(sum(1, 2, 3, 4, 5, 6));
```

# 场景应用

## 实现双向数据绑定

```javascript
let obj = {};
let input = document.getElementById("input");
let span = document.getElementById("span");

let _text = "";
Object.defineProperty(obj, "text", {
  enumerable: true, // 表示该属性是否可被for...in 和 Object.keys 方法枚举
  configurable: true,
  get() {
    console.log("获取数据了");
    return _text;
  },
  set(newVal) {
    console.log("数据更新了");
    input.value = newVal;
    span.innerHTML = newVal;
    console.log(obj);
    _text = newVal;
  },
});

input.addEventListener("keyup", (e) => {
  console.log(e.target.value);
  obj.text = e.target.value;
  console.log(obj.text);
});
```
