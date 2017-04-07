---
title: prototype && __proto__
date: 2017-04-07 17:30:26
tags:
---
## 概念

1. prototype是函数(function)的一个属性，他指向函数的原型；
2. __proto__是对象内部的一个属性，他只想构造器的原型，对象依赖他进行原型链查询；

所以，prototype只有函数才有，其他对象(非函数)不具有该属性，而__proto__是对象的内部属性，任何对象都拥有该属性；

### 上个栗子吧

```javascript
    function Person(name) {
        this.name = name;
    }
    var p1 = new Person('zys');
    console.log(Person.prototype);//Person原型{constructor: Person(name),__proto__: Object}
    console.log(p1.prototype);//undefined
    
    console.log(Person.__proto__);//空函数，function(){}
    console.log(p1.__proto__ === Person.prototype);//true
```
剥栗子的过程中我们发现，Person.prototype(原型)默认两个属性：
* constructor属性，指向构造器，即Person本身
* __proto__属性指向一个空的Object对象

p1是非函数对象，自然就没有prototype属性；

* Person.__proto__属性指向的是一个空函数(function(){});
* p1__proto__属性指向的是构造器(Person)的原型，即Person.prototype;

对象的原型链查询时正是通过这个属性(__proto__)链接到构造器的原型，从而实现查询的层层深入；

### __proto__

Person.__proto__指向的是一个空函数，但是这个空函数究竟是什么：
```javascript
    console.log(Person.__proto__ === Function.prototype);//true
```
Person是构造器也是函数，Person的__proto__属性自然就指向函数(function)的原型，即Function.prototype.
这说明所有的构造器都继承于Function.prototype，甚至包括根构造器Object及Function自身，所有的构造器都继承了Function.prototype的属性及方法。如
length、call、apply、bind(ES5)等。如下:
```javascript
    console.log(Number.__proto__   === Function.prototype); // true
    console.log(Boolean.__proto__  === Function.prototype); // true
    console.log(String.__proto__   === Function.prototype); // true
    console.log(Object.__proto__   === Function.prototype); // true
    console.log(Function.__proto__ === Function.prototype); // true
    console.log(Array.__proto__    === Function.prototype); // true
    console.log(RegExp.__proto__   === Function.prototype); // true
    console.log(Error.__proto__    === Function.prototype); // true
    console.log(Date.__proto__     === Function.prototype); // true
```
javascript中内置构造器/对象共计13个，上面是可直接访问的9个构造器。还有Global不能直接访问，Arguments仅在函数调用时由js引擎创建，Math、JSON
是以对象的形式存在的，无需new。他们的__proto__属性指向构造器函数的原型(Object.prototype);
```javascript
    console.log(Math.__proto__ === Object.prototype);  // true
    console.log(JSON.__proto__ === Object.prototype);  // true
```

### Function.prototype
```javascript
    console.log(typeof Function.prototype)//function
```
Function.prototype是唯一一个prototype的类型为function的prototype,其他构造器的prototype都是一个对象；如下:
```javascript
   console.log(typeof Number.prototype)   // object
   console.log(typeof Boolean.prototype)  // object
   console.log(typeof String.prototype)   // object
   console.log(typeof Object.prototype)   // object
   console.log(typeof Array.prototype)    // object
   console.log(typeof RegExp.prototype)   // object
   console.log(typeof Error.prototype)    // object
   console.log(typeof Date.prototype)     // object 
```
<!--more-->

### JS函数是一等公民
```javascript
    console.log(Function.Prototype.__proto__ === Object.prototype)//true
```
所以，所有的构造器既是函数又是普通JS对象，可以给构造器添加删除属性；同时他也继承了Object.prototype上的所有方法：toString、valueOf、hasOwnProperty等；

### Object.prototype

Person.__proto__ -> Function.prototype;
Function.prototype.__proto__ -> Object.prototype;
Object.prototype.__proto__ -> null;
所以javascript的源头是null，果然"万物皆空啊！"
