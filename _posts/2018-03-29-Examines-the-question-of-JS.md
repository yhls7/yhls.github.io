---
title: JS作用域的探讨
description: 
categories: technology
tags: JS
---

> 最近在看JS作用域和闭包相关知识，找了几个问题试着解析看看···

原题：
```javascript
function Foo() {
    getName = function () { alert (1); };
    return this;
}
Foo.getName = function () { alert (2);};
Foo.prototype.getName = function () { alert (3);};
var getName = function () { alert (4);};
function getName() { alert (5);}
 
//请写出以下输出结果：
Foo.getName();
getName();
Foo().getName();
getName();
new Foo.getName();
new Foo().getName();
new new Foo().getName();
```

解析：
```javascript
Foo.getName(); //2
```
//Foo.getName()指向的是实例的方法。Foo在这里只作为一个普通函数，很显然它从原型链上继承了getName方法，但在实例上又对方法进行重写，覆盖了继承的方法。JS引擎在查找对象的属性（方法）时，会先从对象本身开始查找，只有在本身找不到的情况下，才会去对象的原型上查找。Foo.getName()在执行的时候，在其本身查到了```Foo.getName = function () { alert (2);};```,就不会再去原型上查找了。
```javascript
getName(); //4
```
//JS代码执行前，会对全局作用域内的函数和变量进行声明（先是函数声明，然后才是变量声明），注意赋值操作符右侧的函数不会进行声明提升。综上，代码执行前，先对```function getName(){...}```进行声明，再对```var getName...```进行声明，最后对变量```getName ```进行赋值，同时覆盖了最先声明的```function getName(){...}```。
```javascript
Foo().getName(); //1
```
//Foo()返回的是函数的执行上下文，该作用域中有一个变量的赋值操作，在赋值前，引擎会执行对变量的声明查询，而在该作用域中并没有对getName的声明，引擎会向Foo()的上一级作用域即全局作用域查找（作用域链由内到外），在全局作用域内找到getName的声明，即```var getName...```，查找停止，然后对变量进行赋值操作。***对变量getName进行赋值之后，将会覆盖原有方法***。
//getName()将会执行Foo()返回的上下文中的方法。
```javascript
getName(); //1
```
//经过上一步之后，现在getName指向的是```function () { alert (1); }```。
```javascript
new Foo.getName();  //2 同时返回一个实例
```
//new操作符会创建一个实例，并将作用域链指向原型。（构造函数会默认返回一个新对象实例）
```javascript
new Foo().getName();  //3
```
//同上，区别在于上一步是以Foo.getName()作为构造函数，而本例是以Foo作为构造函数。
//new Foo()返回一个新创建的实例，继承Foo()的原型（这是一个新的实例，没有指定任何属性和方法）。```new Foo().getName()```指向的是原型方法```Foo.prototype.getName = function () { alert (3);};```。
//可分解为new Foo()-->(new Foo()).getName()
```javascript
new new Foo().getName(); //3 同时返回一个实例
```
//这是以Foo的原型方法```Foo.prototype.getName = function () { alert (3);};```作为构造函数创建的一个实例。
//可分解为new Foo()-->(new Foo()).getName()-->new ((new Foo()).getName())

以上均为个人理解，如有错误，欢迎指正···