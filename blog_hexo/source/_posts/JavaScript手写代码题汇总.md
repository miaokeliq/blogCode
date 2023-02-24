---
title: JavaScript手写代码题汇总
date: 2023-02-23 15:11:22
tags: [前端, 闲聊分享, JavaScript]
categories:
  - 经验分享
---

# 前言

主要记录在准备秋招的过程中遇到的前端手写代码题。

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
