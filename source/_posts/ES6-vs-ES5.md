---
title: ES6_vs_ES5
date: 2016-11-07 16:15:52
tags: [javascript, ES6]
---

1. 原文地址: [Overview of JavaScript ES6 features (a.k.a ECMAScript 6 and ES2015+)](http://adrianmejia.com/blog/2016/10/19/Overview-of-JavaScript-ES6-features-a-k-a-ECMAScript-6-and-ES2015/)
2. 原文作者: [Adrian Mejia](http://adrianmejia.com/#about)
3. 译文地址: [L9m](http://damonare.github.io/2016/11/03/JavaScript%20ES6%20%E6%A0%B8%E5%BF%83%E5%8A%9F%E8%83%BD%E4%B8%80%E8%A7%88/#变量的块级作用域)

## JavaScript ES6 核心功能一览（ES6 亦作 ECMAScript 6 或 ES2015+）
  JavaScript 在过去几年里发生了很大的变化。我仅从原文里拿来几个比较实用的新功能(作为笔记<i class="icon-pencil"></i>来用),当然比较基础的如`let const`等,没有介绍,想了解可以点击原文地址或译文地址。
  
## 变量的块级作用域  
  这个特性是结合`let`,`const`实现的,极大地解决了`var`带来的麻烦,这里关于它们就不细细介绍了;
  <!--more-->
## 模版字符串
  有了模版字符串就不必使用赘余的字符串嵌套了,看例子:
  ```
  // ES5
  var first = 'Adrian';
  var last = 'Mejia';
  console.log('Your name is ' + first + ' ' + last + '.');
  ```
  ES6中我们可以使用反引号`和字符串插值${}
  ```
  //ES6
  const first = 'Adrian', last = 'Mejia'; //这里没有使用var
  console.log(`Your name is ${first} ${last}.`);
  ```
## 多行字符串
  我们也没必要用+\n来拼接字符串了:
  ```
  //ES5
  var template = '<li *ngFor="let todo of todos" [ngClass]="{completed: todo.isDone}" >\n' +
  '  <div class="view">\n' +
  '    <input class="toggle" type="checkbox" [checked]="todo.isDone">\n' +
  '    <label></label>\n' +
  '    <button class="destroy"></button>\n' +
  '  </div>\n' +
  '  <input class="edit" value="">\n' +
  '</li>';
  console.log(template);
  ```
  我们依旧可以用ES6的反引号来解决这个问题:
  ```
  //ES6
  const template = `<li *ngFor="let todo of todos" [ngClass]="{completed: todo.isDone}" >
    <div class="view">
      <input class="toggle" type="checkbox" [checked]="todo.isDone">
      <label></label>
      <button class="destroy"></button>
    </div>
    <input class="edit" value="">
  </li>`;
  console.log(template);
  ```
  上面两行代码的执行结果完全一样.
## 解构赋值
  ES6的解构赋值超级实用并且十分简洁!贴个例子:
  ### 从数组中获取元素的值
  ```
  // ES5
  var array = [1, 2, 3, 4];
  var first = array[0];
  var third = array[2];
  console.log(first, third); // 1 3
  ```
  等同于:
  ```
  //ES6
  const array = [1, 2, 3, 4];
  const [first, , third] = array;
  console.log(first, third); // 1 3
  ```
  ### 交换值
  ```
  //ES5
  var a = 1;
  var b = 2;
  var tmp = a;
  a = b;
  b = tmp;
  console.log(a, b); // 2 1
  ```
  等同于:
  ```
  //ES6
  let a = 1, b = 2;
  [a, b] = [b, a]
  console.log(a, b); // 2 1
  ```
  ### 多个返回值的解构
  ```
  // ES5
  function margin() {
    var left=1, right=2, top=3, bottom=4;
    return { left: left, right: right, top: top, bottom: bottom };
  }
  var data = margin();
  var left = data.left;
  var bottom = data.bottom;
  console.log(left, bottom); // 1 4
  ```
  等同于:
  ```
  // ES6
  function margin() {
    const left=1, right=2, top=3, bottom=4;
    return { left, right, top, bottom };
  }
  const { left, bottom } = margin();
  console.log(left, bottom); // 1 4
  ```
  这其中用到了ES6的其他语法,将`{left: left}`简化为`{left}`;
  ### 参数匹配的解构
  ```
  // ES5
  var user = {firstName: 'Adrian', lastName: 'Mejia'};
  function getFullName(user) {
    var firstName = user.firstName;
    var lastName = user.lastName;
    return firstName + ' ' + lastName;
  }
  console.log(getFullName(user)); // Adrian Mejia
  ```
  等同于:
  ```
  const user = {firstName: 'Adrian', lastName: 'Mejia'};
  function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
  }
  console.log(getFullName(user)); // Adrian Mejia
  ```
## 原生Promises
  从回调地狱到promises
  ```
  // ES5
  function printAfterTimeout(string, timeout, done){
    setTimeout(function(){
      done(string);
    }, timeout);
  }
  printAfterTimeout('Hello ', 2e3, function(result){
    console.log(result);
    // 嵌套回调
    printAfterTimeout(result + 'Reader', 2e3, function(result){
      console.log(result);
    });
  });
  ```
  回调的噩梦就不多说了,来看一下promises的写法:
  ```
  // ES6
  function printAfterTimeout(string, timeout){
    return new Promise((resolve, reject) => {
      setTimeout(function(){
        resolve(string);
      }, timeout);
    });
  }
  printAfterTimeout('Hello ', 2e3).then((result) => {
    console.log(result);
    return printAfterTimeout(result + 'Reader', 2e3);
  }).then((result) => {
    console.log(result);
  });
  ```
## 箭头函数
  ES6没有新增了一种函数表达式,箭头函数:
  ```
  // ES5
  var _this = this; // 保持一个引用
  $('.btn').click(function(event){
    _this.sendData(); // 引用的是外层的 this
  });
  $('.input').on('change',function(event){
    this.sendData(); // 引用的是外层的 this
  }.bind(this)); // 绑定到外层的 this
  ```
  我们需要一个临时的`this`在函数内部引用或用`bind`绑定,在ES6中我们可以用箭头函数:
  ```
  // ES6
  // 引用的是外部的那个 this
  $('.btn').click((event) =>  this.sendData());
  // 隐式返回
  const ids = [291, 288, 984];
  const messages = ids.map(value => `ID is ${value}`);
  ```
## For...of
  从`for`到`forEach`再到`for...of`:
  ```
  // ES5
  // for
  var array = ['a', 'b', 'c', 'd'];
  for (var i = 0; i < array.length; i++) {
    var element = array[i];
    console.log(element);
  }
  // forEach
  array.forEach(function (element) {
    console.log(element);
  });
  ```
  ES6的`for...of`:
  ```
  // ES6
  // for ...of
  const array = ['a', 'b', 'c', 'd'];
  for (const element of array) {
      console.log(element);
  }
  ```
## 默认参数
  从检查一个变量是否被定义到重新指定一个值再到`default parameters`。
  之前或许我们会这样写:
  ```
  // ES5
  function point(x, y, isFlag){
    x = x || 0;
    y = y || -1;
    isFlag = isFlag || true;
    console.log(x,y, isFlag);
  }
  point(0, 0) // 0 -1 true
  point(0, 0, false) // 0 -1 true
  point(1) // 1 -1 true
  point() // 0 -1 true
  ```
  如果你写过这样的代码,那么当然你也会知道这里面的坑(当然有办法解决),但利用ES6我们可以用更简洁的代码实现同样的功能:
  ```
  // ES6
  function point(x = 0, y = -1, isFlag = true){
    console.log(x,y, isFlag);
  }
  point(0, 0) // 0 0 true
  point(0, 0, false) // 0 0 false
  point(1) // 1 -1 true
  point() // 0 -1 true
  ```
## 剩余参数
  从参数到剩余参数和扩展操作符:
  在ES5中,获取任意数量的参数是非常麻烦的:
  ```
  // ES5
  function printf(format) {
    var params = [].slice.call(arguments, 1);
    console.log('params: ', params);
    console.log('format: ', format);
  }
  printf('%s %d %.2f', 'adrian', 321, Math.PI);
  ```
  我们可以利用ES6中的`rest`操作符`...`来实现同样的功能:
  ```
  // ES6
  function printf(format, ...params) {
    console.log('params: ', params);
    console.log('format: ', format);
  }
  printf('%s %d %.2f', 'adrian', 321, Math.PI);
  ```
## 展开运算符
  从`apply()`到展开运算符,我们还是用`...`来解决:
  ```
  // ES5
  Math.max.apply(Math, [2,100,1,6,43]) // 100
  ```
  在ES6中,我们可以运用`...`展开运算符:
  ```
  // ES6
  Math.max(...[2,100,1,6,43]) // 100
  ```
  ### 从`concat`到展开运算符:
  ```
  // ES5
  var array1 = [2,100,1,6,43];
  var array2 = ['a', 'b', 'c', 'd'];
  var array3 = [false, true, null, undefined];
  console.log(array1.concat(array2, array3));
  ```
  在ES6中,我们可以运用展开运算符来取代嵌套:
  ```
  // ES6
  const array1 = [2,100,1,6,43];
  const array2 = ['a', 'b', 'c', 'd'];
  const array3 = [false, true, null, undefined];
  console.log([...array1, ...array2, ...array3]);
  ```
## 屌屌的ES6,前端的路好长!
  
  