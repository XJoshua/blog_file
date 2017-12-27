---
title: promise
date: 2017-12-27 14:26:37
tags: [promise, javascript, async]
---
## 静态方法

1. Promise.resolve()

* 将现有的对象转化为Promise对象;
* 如果参数是promise实例,则直接返回这个实例;
* 如果参数是thenabled对象,则先将其转化为promise对象,然后立即执行这个对象的then方法;
* 如果参数是一个原始值,则返回一个promise对象,状态为resolved,这个原始值会传递给回调;
* 没有参数,直接返回一个resolved的promise对象;

2. Promise.reject()

* 同上,但是返回的是rejected状态的promise对象;

3. Promise.all()

* 接收一个Promise实例的数组或者具有Iterator接口的对象;
* 如果元素不是Promise对象,则使用Promise.resolve转成Promise对象;
* 如果全部成功,状态变成resolved,返回值将组成一个数组传给回调;
* 只要有一个失败,状态就变为rejected,返回值将直接传递给回调;
* all()的返回值也是新的Promise对象;

4. Promise.race()

* 同上,区别是只要有一个Promise实例率先发生变化(无论状态变成resolved还是rejected)都将触发then中的回调,返回值将传递给回调

<!--more-->

## catch()和then(null, fn)

在有些情况下catch与then(null, fn)并不相同:
```javascript
  ajaxLoad1().then(res => { return ajaxLoad2()}).catch(err => console.log(err))
```
此时,catch捕获的并不是ajaxLoad1的错误,而是ajaxLoad2的错误,所以需要结合两者一起来用:
```javascript
  ajaxLoad1().then(res => { return ajaxLoad2() }, err => console.log(err)).catch(err => console.log(err))
```

## 断链

```javascript
  function loadAsyncFnX(){ return Promise.resolve(1) }
  function doSth(){ return 2; }

  function asyncFn(){
    var promise = loadAsyncFnX();
    promise.then(() => { return doSth(); })
    return promise;
  }

  asyncFn().then(res => console.log(res)).catch(err => console.log(err));
```
上面这种用法,从执行结果来看,then中回调的参数并不是doSth()返回的结果,而是loadAsyncFnX()返回的结果,catch到的错误也是loadAsyncFnX()中的错误,所以doSth()的结果和错误将不会被后面的then中的回调捕获到,因而形成了断链,因为then方法将返回一个新的Promise对象,而不是原来的Promise对象;
```javascript
  function loadAsyncFnX(){ return Promise.resolve(1); }
  function doSth(){ return 2; }

  function asyncFn(){
    var promise = loadAsyncFnX();
    return promise.then(function(){
      return doSth();
    })
  }

  asyncFn().then(res => console.log(res)).catch(err => console.log(err))
```

## 穿透

如果then或catch接收到的不是函数,那么就会发生穿透行为,所以在应用过程中,应该保证then接收到的参数始终是一个函数:
```javascript
  new Promise(resolve => resolve(8)).then(1).catch(null).then(Promise.resolve(9)).then(res => console.log(res))
```

## 长度未知的串行与并行

* 并行
```javascript
  getAsyncArr().then(promiseArr => {
    var resArr = [];
    promiseArr.forEach(v => { 
      v().then(res => resArr.push(res));
    })
    return resArr;
  }).then(res => console.log(res));
```
使用forEach遍历执行promiseArr但是第二个then有可能拿到的是不完整的结果,因为promise是异步的,所以第二个then的回调无法预知promiseArr中的每一个promise是否都执行完成.可以结合Promise.all改善:
```javascript
  getAsyncArr().then(promiseArr => { return Promise.all(promiseArr); }).then(res => console.log(res))
```

* 串行

如果需要串行执行,可以利用数据的reduce来处理:
```javascript
  var promiseArr = [
    function() { return new Promise(resolve => resolve(1)) },
    function(data) { return new Promise(resolve => resolve(1 + data))},
    function(data) { return new Promise(resolve => resolve(1 + data))}
  ];

  promiseArr.reduce((prev, next) => prev.then(next).then(res => res), Promise.resolve()).then(res => console.log(res))
```