---
title: JavaScript高频之防抖
date: 2023-02-14 02:54:00
categories: 
- 前端
- JavaScript高级
tags: [JavaScript, 前端, 手写代码]
---

# 前言

最近在阅读大佬mqyqingfeng的防抖博客，打算系统地学习一下防抖，并在大佬的讲解基础上加入一些自己的理解，废话不多说，现在开始吧！

# 防抖

**防抖的原理：** 你尽管触发事件，但是我一定在事件触发n秒后才执行，如果你在一个事件触发的n秒内又触发了这个事件，那我就以新的事件的事件为准，n秒后才执行，总之，就是要等你触发完事件n秒内不再触发事件，我才执行。

# 第一版

根据这段表述，我们可以写第一版的代码：

``````javascript
// 第一版
// debounce : 防抖动
function debounce(func, wait) {
 	let timeout;
  return function(){
    clearTimeout(timeout)
    timeout = setTimeout(func, wait);
  }
}
``````

> clearTimeout() 方法可取消由 setTimeout()方法设置的timeout
>
> clearTimeout() 方法的参数必须是由setTimeout()返回的ID值。

如果我们要使用它，以最一开始的例子为例：

``````javascript
container.onmousemove = debounce(getUserAction,1000);

// 这样的话 container.onmousemove 就变成以下的形式;

container.onmousemove = function(){
 	clearTimeout(timeout)
  timeout = setTimeout(func, wait);
}
``````

现在随你怎么移动，反正移动完1000ms内不再触发，我才执行事件。

顿时就从165次降低到1次！

接下来接着完善它。

# this

如果我们在`getUserAction`函数中`console.log(this)`，在不使用`debounce`函数的时候，`this`的值为：

``````HTML
<div id="container"></div>
``````

但是如果使用我们的`debounce`函数，`this`就会指向Window对象！

***但是我们需要将`this`指向正确的对象***。

我们修改下代码：

```````javascript
// 第二版
function debounce(func, wait) {
 	let timeout;
  return function(){
    let context = this;
    
    clearTimeout(timeout)
    timeout = setTimeout(function(){
      func.apply(context)
    },wait);
  }
```````

现在`this`已经可以正确指向了。让我们看下个问题:

# event对象

