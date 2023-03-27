---
title: MDN文档笔记
date: 2023-03-08 21:08:58
tags:
---

# 前言

背了八股文后去牛客网上做了下笔试题然后被吊打了，感觉光背八股文还是有很多点会遗漏，

所以打算看一遍 MDN 的 JavaScript 教程，并且在这里做笔记，方面后面回顾复习。

# JavaScript 指南

## 语法和数据类型

- JavaScript 是**区分大小写**，变量 `früh` 和 `Früh ` 则是两个不同的变量

- 在代码执行过程中，注释将被**自动跳过（不执行）**

### 标识符

使用变量来作为值的符号名。变量的名字又叫做**标识符**，或者是**字面量**。

- 标识符只能包含字母或数字或下划线('\_')或美元符号("$")，且不能以数字开头。

- JavaScript 无法将标识符转换为字符串，但有时可以将字符串解析为标识符。

- JavaScript 语言是区分大小写的，所以字母可以是从“A”到“Z”的大写字

  母和从“a”到“z”的小写字母

### 变量求值

- 用 `var` 或 `let` 语句声明的变量，如果没有赋初始值，则其值为 `undefined`.

- 如果访问一个未声明的变量会导致抛出一个引用错误（ReferenceError）异常.

- 可以使用 undefined 来判断一个变量是否已赋值.

- `undefined` 值在布尔类型环境中会被当作 `false `

- 数值类型环境中 `undefined ` 值会被转换为 `NaN`。

  ```javascript
  var a;
  a + 2; // 计算为 NaN
  ```

- 当对一个`null`变量求值时，空值`null`在数值类型环境中会被当作 0 来对待，而布尔类型

  环境中会被当作`false`

  ```javascript
  var n = null;
  console.log(n * 32); // 在控制台中会显示 0
  ```

### 变量的作用域

- 在函数之外声明的变量，叫做全局变量。在函数内部声明的变量，叫做局部变量。

- 语句块中声明的变量将成为语句块所在函数（或全局作用域）的局部变量。

### 变量提升

JavaScript 变量的另一个不同寻常的地方是，你可以先使用变量稍后再声明变量

而不会引发异常。这一概念称为变量提升；JavaScript 变量感觉上是被“提升”或移

到了函数或语句的最前面。但是，提升后的变量将返回 undefined 值。因此在使用

或引用某个变量之后进行声明和初始化操作，这个被提升的变量仍将返回 undefined 值。

```javascript
/**
 * 例子 1
 */
console.log(x === undefined); // true
var x = 3;

/**
 * 例子 2
 */
// will return a value of undefined
var myvar = "my value";

(function () {
  console.log(myvar); // undefined
  var myvar = "local value";
})();
```

上面的例子，也可以写作：

```javascript
/**
 * 例子 1
 */
var x;
console.log(x === undefined); // true
x = 3;

/**
 * 例子 2
 */
var myvar = "my value";

(function () {
  var myvar;
  console.log(myvar); // undefined
  myvar = "local value";
})();
```

- 在 ECMAScript 6 中，let 和 const 同样会被提升变量到代码块的顶部但是

  不会被赋予初始值。在变量声明之前引用这个变量，将抛出引用错误

  （ReferenceError）。这个变量将从代码块一开始的时候就处在一个“

  暂时性死区”，直到这个变量被声明为止。

  ```javascript
  console.log(x); // ReferenceError
  let x = 3;
  ```

### 函数提升

对于函数来说，只有函数声明会被提升到顶部，而函数表达式不会被提升。

```javascript
/* 函数声明 */

foo(); // "bar"

function foo() {
  console.log("bar");
}

/* 函数表达式 */

baz(); // 类型错误：baz 不是一个函数

var baz = function () {
  console.log("bar2");
};
```

### 全局变量

- 全局变量是*全局对象*的属性，全局对象是`window`

### 常量

- 常量标识符的命名规则和变量相同。

- 常量不可以通过重新赋值改变其值，也不可以在代码运行时重新声明。

  它必须被初始化为某个值。

- 常量的作用域规则与`let`块级作用域变量相同。

- **对象属性被赋值为常量是不受保护的，下面语句执行时不会产生错误。**
  ```javascript
  const MY_OBJECT = { key: "value" };
  MY_OBJECT.key = "otherValue";
  ```
- **同样，数组被定义为常量也是不受保护的**，所以下面的语句执行时也不会产生错误。
  ```javascript
  const MY_ARRAY = ["HTML", "CSS"];
  MY_ARRAY.push("JAVASCRIPT");
  console.log(MY_ARRAY); //logs ['HTML','CSS','JAVASCRIPT'];
  ```

## 数据结构和类型

### 数据类型

最新的 ECMAScript 标准定义了 8 种数据类型：

- 7 种基本数据类型：

  - Boolean：布尔值，有两个值分别是：`true`，`false`。

  - null，一个表明 null 值的特殊关键字。JavaScript 是大小写敏感的，

    因此 null 与 Null、NULL 或变体完全不同。

  - undefined, 和 null 一样是一个特殊的关键字，undefined 表示变量未赋值时的属性。

  - Number, 数字，整数或浮点数，例如：`42`或`3.1415926`

  - BigInt，任意精度的整数，可以安全地存储和操作大整数，甚至可以超过数字的安全整数限制。

  - String，字符串是一串表示文本值的字符序列，例如："Howdy" 。

  - Symbol（代表），ES6 新增加的类型。一种实例是唯一且不可改变的数据类型。

- 1 种引用类型->对象（Object）
