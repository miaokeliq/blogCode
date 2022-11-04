---
title: JavaScript高级(二) 继承
date: 2022-10-12 14:42:52
tags: [JavaScript, 前端]
categories:
- 前端
- JavaScript高级
---

## 1. 原型链继承

```javascript
function Parent(){
    this.name = '缪克立';
}

Parent.prototype.getName = function(){
    return this.name
}

function Child(){

}

Child.prototype = new Parent();
var child1 = new Child();
console.log(child1.getName()) // 缪克立
```

问题：

1. 引用类型的属性被所有实例共享，举个例子：

```javascript
function Parent(){
    this.names = ['缪克立', '缪立克'];
}

function Child(){
    this.age = [18]
}

Child.prototype = new Parent();

var child1 = new Child();

child1.names.push('mkl');
child1.age.push(19)

console.log(child1.names);	//['缪克立', '缪立克', 'mkl']
console.log(child1.age)		//[18, 19]


var child2 = new Child();

console.log(child2.names);	//['缪克立', '缪立克', 'mkl']
console.log(child2.age);	//[18]


console.log(Child)				//Child(){this.age = [18]}
console.log(Child.prototype)   //['缪克立', '缪立克', 'mkl']
```

`child1`和`child2`都是`new`的两个新的对象，双方都有各自的`age`属性和`names`属性，但是它们的原型都是`Parent`即定义成了一个对象，当修改`child1`的`names`属性时，由于`child1`没有这个属性，就往原型上找，于是找到了`Parent`，并且`Parent`是引用类型，`child1`保存的原型就是`parent`的地址值，所以修改`names`属性就顺着地址值找到`Parent`的`names`属性更改。`child1`和`child2`的`age`属性是基本数据类型，双方保存的都是原本的值，所以没有共享。



2. 在创建`Child`的实例时，不能向`Parent`传参 

   

##   2. 借用构造函数（经典继承）

废话不多说，直接上代码：

```javascript
function Parent(){
    this.names = ['缪克立', '缪立克'];
}


function Child(){
    Parent.call(this);
}

var child1 = new Child();

child1.names.push('mkl');

console.log(child1.names);

var child2 = new Child();

console.log(child2.names);
```

上面的`Parent.call(this)`就是借用构造函数的调用。通过`call()`或`apply()`方法，`Parent`构造函数在为`Child`的实例创建的新对象的上下文中执行了。**相当于在新的`Child`对象上运行了`Parent()`函数中的所有初始化代码**。结果就是每个`child`实例都会有自己的`names`属性。

***优点：***

1. 避免了引用类型的属性被所有实例共享。

2. 可以在Child中向Parent传参。

   举个例子：

   ```javascript
   function Parent(name){
       this.name = name;
   }
   
   
   function Child(name){
       Parent.call(this,name);
   }
   
   var child1 = new Child('kevin');
   
   
   console.log(child1.name);	// kevin
   
   var child2 = new Child('daisy');
   
   console.log(child2.name);   // daisy
   ```

   

***缺点***：

方法都在构造函数中定义，每次创建实例都会创建一遍方法。



## 3. 组合继承

结合原型链继承和经典继承。

```javascript
function Parent(name){
    this.name = name;
    this.colors = ['red','blue','green'];
}

Parent.prototype.getName = function(){
    return this.name
}


function Child(name, age){
    Parent.call(this,name);
    this.age = age;
}

Child.prototype = new Parent();
Child.prototype.constructor = Child;

var child1 = new Child('kevin', '18');

child1.colors.push('black');

console.log(child1.name); // kevin
console.log(child1.age);  // 18
console.log(child1.colors); //['red', 'blue', 'green', 'black']
console.log(child1.getName()) // kevin
var child2 = new Child('daisy', '20')
console.log(child2.name); // daisy
console.log(child2.age);  // 20
console.log(child2.colors);//['red', 'blue', 'green']
console.log(child2.getName()) // daisy
```

通过这种方式就可以创建两个`child`实例，让这两个实例都有自己的属性，包括`colors` ，同时**还共享相同的方法**，弥补了*借用构造函数*继承的缺点，实现函数服用。

***优点***：融合原型链继承和经典继承的优点，是JavaScript中最常用的继承模式。



## 4. 原型式继承

```javascript
function createObj(o){
    function F(){}
    F.prototype = o;
    return new F();
}
```

实际上就是 ES5 `Object.create`的模拟实现，将传入的对象作为创建的对象的原型。

***缺点***：包含引用类型的属性值始终都会共享相同的值，这点跟原型链继承一样。

```javascript
function createObj(o){
    function F(){}
    F.prototype = o;
    return new F();
}
var person = {
    name: 'kevin',
    friends: ['daisy', 'kelly']
}

var person1 = createObj(person)
var person2 = createObj(person)


person1.name = 'person1';
console.log(person2.name) //kevin

person1.friends.push('taylor');
console.log(person2.friends); //['daisy', 'kelly', 'taylor']
```

 注意：修改`person1.name`的值，`person2.name`的值并未发生改变，并不是因为`person1`和`person2`有独立的`name`值，而是因为`person1.name = 'person1'`，给`person1`添加了`name`值，并非修改了原型上的`name`值。



## 5. 寄生式继承

创建一个仅用于封装继承过程的函数，该函数在内部以某种形式来做增强对象，最后返回对象。

```javascript
function createObj (o) {
    var clone = Object.create(o);
    clone.sayName = function () {
        console.log('hi');
    }
    return clone;
}
```

***缺点：***跟借用构造函数模式一样，每次创建对象都会创建一遍方法。



## 6.寄生组合式继承

重复一下组合继承的代码：

```javascript
function Parent(name){
    this.name = name;
    this.colors = ['red','blue','green'];
}

Parent.prototype.getName = function(){
    return this.name
}


function Child(name, age){
    Parent.call(this,name);
    this.age = age;
}

Child.prototype = new Parent();
Child.prototype.constructor = Child;

var child1 = new Child('kevin', '18');

child1.colors.push('black');

console.log(child1); 
```

组合继承最大的缺点是会嗲用两次父构造函数。

一次是设置子类型实例的原型的时候：

```javascript
Child.prototype = new Parent();
```

一次在创建子类型实例的时候：

```javascript
var child1 = new Child('kevin', '18')
```

回想一下new的模拟实现，在这句中，我们就会执行：

```javascript
Parent.call(this.name);
```

在这里，我们又会调用了以此Parent构造函数。

所以在这里例子中，如果我们打印`child1`对象，我们会发现`child1.prototype`和`child1`都有一个属性为`colors`，属性值为`['red','blue','green']`。

那么该如何避免这一次重复调用呢？

如果不适用`Child.prototype = new Parent()`，而是间接的让`Child.prototype`访问到`Parent.prototype`呢？

以下代码：

```javascript
function Parent (name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Parent.prototype.getName = function () {
    console.log(this.name)
}

function Child (name, age) {
    Parent.call(this, name);
    this.age = age;
}

// 关键的三步
var F = function () {};

F.prototype = Parent.prototype;

Child.prototype = new F();


var child1 = new Child('kevin', '18');

console.log(child1);
```

最后封装一下这个继承方法：

```javascript
function object(o) {
    function F() {}
    F.prototype = o;
    return new F();
}

function prototype(child, parent) {
    var prototype = object(parent.prototype);
    prototype.constructor = child;
    child.prototype = prototype;
}

// 当我们使用的时候：
prototype(Child, Parent);
```

> 引用《JavaScript高级程序设计》：
>
> > 这种方式的高效率体现它只调用了一次 Parent 构造函数，并且因此避免了在 Parent.prototype 上面创建不必要的、多余的属性。与此同时，原型链还能保持不变；因此，还能够正常使用 instanceof 和 isPrototypeOf。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。