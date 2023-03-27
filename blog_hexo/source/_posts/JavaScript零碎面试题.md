---
title: JavaScript零碎面试题
date: 2023-03-25 14:35:24
tags:
---

# 函数表达式和函数声明有什么区别？

## 函数声明

语法：

```javascript
function 函数名(参数) {
  要执行的代码;
}
```

## 函数表达式

语法：

```javascript
var/let/const 变量=function(参数){
    要执行的代码
}
```

## 函数声明和函数表达式的区别？

函数声明必须带有标识符（函数名称），函数表达式则可以省略

表达式里的名字不能在函数外面用，只能在函数内部用
函数有一个 name 属性，指向紧跟在 function 关键字之后的那个函数名。如果函数表达式没有名字，那 name 属性指向变量名
函数声明会被预解析，函数表达式不会

```javascript
//1、名字
//函数声明必需带名字
function fn() {}
//function(){};        //报错，没有名字

//函数表达式可以没有名字
let fn1 = function () {};
(function () {});
!function () {};

//表达式名字的作用
let fn2 = function newFn() {
  console.log(newFn); //可以在这里面用。有一个作用就是在这里用递归
};
fn2();
//newFn();    //报错，不能在外面用

//name属性
console.log(
  fn.name, //fn
  fn1.name, //fn1    表达式没有名字，name属性指向表达式变量名
  fn2.name //newFn
);

//2、预解析
fn3();
function fn3() {
  console.log("fn3");
}

//fn4();    //报错，不会被预解析
let fn4 = function () {
  console.log("fn4");
};
```

# 讲一下变量提升（预编译），为什么会有变量提升？

javaScript 在执行之前会先进行预编译，主要做两个工作：

1. 将全局作用域或者函数作用域内所有函数声明提前。

2. 将全局作用域或者函数作用域内所有 var 声明的变量提前声明，并赋值为 undefined。

这就是变量提升。
