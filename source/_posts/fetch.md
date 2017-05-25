---
title: fetch
date: 2017-05-25 10:12:24
tags: [javascript, fetch, request, front-end]
---
## fetch

   fetch是web异步通信的未来，但是目前部分浏览器对它还不太支持，其中chrome42-45，对中文支持有问题，建议从chrome46起使用fetch；对于IE8+浏览器，建议
依次安装下列垫片：[原文地址](http://louiszhai.github.io/2016/11/02/fetch)

* es5的polyfill － es5-shim，es5-sham.
* Promise的polyfill - es6-promise.
* fetch的polyfill － fetch－ie8.

## Promise特性

   fetch方法返回一个Promise对象，所以fetch可以方便地使用then方法将各个处理逻辑串起来，使用Promise.resolve()或者Promise.reject()方法将分别返回肯定结果
的Promise或否定结果的Promise，从而调用下一个then或者catch。一旦then中的语句出现错误，也将跳到catch中。
<!--more-->

## response type

   一个fetch请求的响应类型(response.type)为如下三种之一：
   * basic
   * cors
   * opaque
   同域下，响应类型为basic；
   跨域下，服务器没有返回CORS响应头，响应类型为opaque。此时我们不能查看任何有价值的信息，比如不能查看response，status，url等等。
   同样是跨域下，如果服务器返回了CORS响应头，那么响应信息将为cors。此时响应头中除Cache-Control,Content-Language,Content-Type,Expores,Last-Modified和
Progma之外的字段都不可见；
   但是，无论是同域还是跨域，fetch请求都到达了服务器；
   
## mode

   fetch可以设置不同模式使得请求有效，该值可以在fetch方法第二个参数对象中定义：
   ```javascript
      fetch(url, {mode: 'cors'});
   ```
   可定义的模式如下：
   * same-origin: 表示同域下可请求成功；反之，浏览器将拒绝发送本次fetch，同时抛出错误TypeError:Failed to fetch(...).
   * cors: 表示同域和带有CORS响应头的跨域下可请求成功。其他请求将被拒绝。
   * cors-with-forced-preflight: 表示在发出请求之前，将进行preflight检查。
   * no-cors: 常用于跨域请求不带CORS响应头场景，此时响应类型为opaque。

## header

   * fetch获取http响应头：
   ```javascript
      fetch(url).then(function(response) {
          console.log(response.headers.get('Content-Type'));
      });
   ```
   * 设置http请求：
   ```javascript
      var headers = new Headers();
      headers.append('Content-Type', 'text/html');
      fetch(url, {
          headers: headers
      })
   ```
   * 检索header的内容：
   ```javascript
      var header = new Headers({
        "Content-Type": 'text/plain'
      });
      console.log(header.has('Content-Type'));
      console.log(header.has('Content-Length'));
   ```
## post
    
   在fetch中发送post请求，在fetch方法第二个参数对象中设置：
   ```javascript
      var headers = new Headers();
      headers.append('Content-Type', 'application/json;charset=UTF-8');
      fetch(url, {
          method: 'post',
          headers: headers,
          body: JSON.stringify({
            date: '2016-10-08',
            time: '15:16:00'
          })
      });
   ```
   
## credentials
    
   跨域请求中需要带有cookie时，需要在fetch方法的第二个参数对象中添加credentials属性(include)：
   * omit: 默认值；
   * same-origin: 同源，表示同域请求才发送cookie；
   
## catch

   同XMLHttpRequest一样，无论服务器返回什么样的状态码，都不会进入错误捕获。也就是说，XMLHttpRequest实例是不会触发onerror事件回调的，fetch不会触发reject。
通常只在网络出现问题或者ERR_CONNECTION_RESET时，他们才会进入到相应的错误捕获里。

## cache
    
   cache表示如何处理缓存，遵守http规范，拥有如下几种值：
   * default: 表示fetch请求之前将检查一下http的缓存；
   * no-store: 表示fetch请求将完全忽略http缓存的存在。即请求之前不再检查http缓存，拿到响应后也不会更新http缓存；
   * no-cache: 如果存在缓存，那么fetch将发送一个条件查询request和一个正常的request，拿到响应后，会更新http缓存；
   * reload: 表示fetch请求之前将忽略http缓存的存在，但是请求拿到后它将主动更新http缓存；
   * force-cache: 表示不顾一切的依赖缓存，即使缓存过期了，他依然从缓存中读取。除非没有缓存，那么他将发送一个正常的request；
   * only-if-cached: 表示fetch请求不顾一切地依赖缓存，即使缓存过期了，如果没有缓存，它将抛出网络错误（该设置只在mode为same-origin时有效）；
   如果fetch请求的header里包含If-Modified-Since,If-None-Match,If-Unmodified-Since,If-Match,或者If-Range之一，且cache的值为default，
那么fetch将自动把cache的值设置为no-store
   
   