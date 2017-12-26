---
title: 关于this
date: 2017-12-26 16:49:36
tags: [javascript, this]
---

## this

this关键字是javascript中比较复杂的机制之一,它不是一个特殊的关键字,被自动定义在所有的函数作用域中;this不是在编写的时候绑定的,而是在运行时候绑定的上下文执行环境;this绑定和函数申明无关,反而和函数被调用的方式有关.当一个函数被调用时,会创建一个  活动记录,被称为执行环境;这个记录会包含函数是从何处被调用的,函数是如何被调用的,传递了什么参数等信息;这个记录的属性之一就是函数执行期间被使用的this引用;

<!--more-->

## 如何确定this的指向

要确定this只要搞清楚调用的位置就好了,我们称调用的位置为调用点,即一个函数被调用的位置;

## 默认绑定

所谓默认绑定就是指独立函数的调用形式;当我们以独立函数的形式调用时,javascript就会采用默认绑定,将this指向全局对象;但是对于严格模式来说,默认绑定全局对象是不合法的,this被置为undefined;

## 隐含绑定

调用点是否有一个环境变量,也称为拥有者或容器对象;
```javascript
  function foo() {
    console.log(this.a);
  }
  var obj = {a: 2, foo: foo};
  obj.foo();
```
foo被申明,然后被obj添加到其属性上,无论foo是否一开始就在obj上被申明,还是后来作为引用添加,obj都是这个函数的拥有者或包含者;

### 隐含绑定丢失

```javascript
  function foo() {
    console.log(this.a);
  }
  var obj = {a: 2, foo: foo};
  var bar = obj.foo;
  bar();
```
上面的调用模式又回到了默认绑定模式;

## 明确绑定

在javascript中我们可以强制制定一个函数在运行时候this的指向;可以通过call、apply、bind来扩充函数赖以生存的作用域;

## new绑定

这个绑定涉及new关键字调用时候的工作机制,此时函数被当作构造函数使用,在内部完成了一系列事情:

* 一个新的对象会被创建
* 这个新建的对象会被接入原型链
* 找个对象会被设置为函数调用的this绑定
* 除非函数返回一个自己的其他对象或函数,否则被new调用的函数将自动返回那个新建的对象

## 绑定例外

* 将null、undefined传给apply、call、bind等函数,会采用默认绑定规则;
* 一个例子:
```javascript
  function foo() {
    console.log(this.a);
  }
  var a = 2;
  var o = {a; 3, foo: foo};
  var p = {a; 4};
  (p.foo = o.foo)();
```
这个也是采用的默认绑定,p.foo = o.foo的返回值是目标函数的引用,所以即是foo();
* es6中的箭头函数
