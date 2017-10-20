---
title: async
date: 2017-10-09 14:00:45
tags: [async, promise, callback, generator]
---

# Async

## 异步操作处理方法

1. Callback
2. Promise
3. Generator
4. Async await

## Callback

回调很简单就是，就是把一个函数cb作为参数传入另一个函数A里面，然后在函数A里面调用函数cb：
```javascript
   function func(x, cb) {
      cb(x);
   }
```
下面举个通过node读取文件内容的例子：
```javascript
   var fs = require('fs');
   fs.readFile('hello.txt', function(err, data) {
     if (err) {
       return console.log(err);
     }
     
     console.log(data);
   });
   
   console.log('Finished');
```
但是有时候我们可能会在callback里面处理callback，也就是说在回调里面接着写回调。。。这样代码就会越写越深，代码的耦合度会很高，维护性会很差这也就是常说的`回调地狱`:
```javascript
   a(function(resultFromA){
     b(resultFromA, function(resultFromB){
       c(resultFromB, function(resultFromC){
         d(resultFromC, function(resultFromD){
           e(resultFromD, function(resultFromE){
             f(resultFromE, function(resultFromF){
               console.log(resultFromF);
             });
           });
         });
       });
     });
   });
```
<!--more-->

## Promise

Promise就是说你做了A事情，成功了就做B，不成功就做C，你好可以继续做D事情，然后进行成功不成功的处理，如下图：
![Promise](https://i.imgur.com/w9BxjmL.png)
eg:
```javascript
   var fire = new Promise(function(resolve, reject) {
     setTimeout(function() {
       resolve('执行成功！');
     }, 3000);
   });
   
   fire.then(function(result) {
     console.log(result);
   });
```

## Generator

Generator是一个状态机，内部保存了机器的运行状态。我们可以通过获取机器的完成状态（done），我们就能够重复调用机器。我们可以使用yield暂停一个函数，并跳出函数：
eg:
```javascript
   function* gen() {
      yield 1;
      yield 2;
      yield 3;
   }
   var g = gen();
   g.next(); // {value: 1, done: false}
   g.next(); // {value: 2, done: false}
   g.next(); // {value: 3, done: false}
   g.next(); // {value: undefined, done: true}
```

## Async/Await

利用Async／Await可以写出更优雅的异步调用代码：
```javascript
   async const getPosts = () => {
      await posts = axios.get('https://calpa.me/posts');
      await accountInfo = axios.get('https://calpa.me/about');
   }
```
要使用Async/Await需要安装Node.js7.6或以上的版本；
