---
title: Ajax
date: 2017-05-18 16:19:47
tags: [Ajax, javascript, front-end]
---
## 前后端分离的基石Ajax

   现代浏览器几乎都支持ajax，但是实现的技术分为两种：[原文地址](http://louiszhai.github.io/2016/11/02/ajax)
   * 标准浏览器通过XMLHttpRequest对象实现了ajax功能，创建一个用于发送ajax请求的对象只需要一条语句：
   ```javascript
      var xhr = new XMLHttpRequest();
   ```
   * IE浏览器通过XMLHttpRequest或者ActiveXObject对象实现ajax功能；
   ```javascript
      function getXHR() {
          var xhr;
          if (window.XMLHttpRequest) {
              xhr = new XMLHttpRequest();
          } else if (window.ActiveXObject) {
              try {
                  xhr = new ActiveXObject("Msxml2.XMLHTTP");
              } catch (e) {
                  try {
                      xhr = new ActiveXObject("Microsoft.XMLHTTP");
                  } catch (e) {
                      alert('您的浏览器暂时不支持Ajax！');
                  }
              }
          }
          return xhr;
      }
   ```
   所以，为了迎合各大浏览器刁钻的口味，我们不得不提供兼容的方法; 
<!--more-->
   
## ajax有没有破坏js单线程机制

   关于这个问题我们就要先研究一下浏览器的单线程机制。一般情况下的，浏览器四线程：
   * GUI渲染线程
   * javascript引擎线程
   * 浏览器事件触发线程
   * HTTP请求线程
   这么多线程，他们是如何同js引擎线程交互的呢？
   通常，线程之间交互以事件的方式发生，通过事件回调的方式予以通知，事件回调又是以先进先出的方式添加到任务队列末尾的，等js引擎空闲时，任务队列中排队的
任务将会依次被执行。这些事件回调包括setTimeout,setInterval,click,ajax异步请求等回调。

### 浏览器中，js引擎线程会循环从任务队列中读取事件并执行，这种运行机制称为Event Loop（事件循环）
   
   详细讲一下，发送一个ajax请求，浏览器内部线程的执行情况：
   对于一个ajax请求，js引擎首先生成XMLHttpRequest实例对象，open过后在调用send方法。至此，所有的语句都是同步执行的，但是从send方法内部开始，浏
览器就为即将发生的网络请求创建了新的http请求线程，这个线程独立于js引擎线程，于是网络请求异步被发送出去了。js引擎并不会等待ajax发起的http请求收到结果
而是直接顺序往下执行；
   当ajax请求被服务器响应并且接受到response后，浏览器事件触发线程捕获到了ajax的回调事件onreadystatechange。该回调事件并没有被立即执行，而是先被
添加到任务队列的末尾。直到js引擎空闲了，任务队列的任务才会被捞出来按照先来后到的顺序，依次执行（包括刚刚append到队列末尾的onreadystatechange事件）
   在onreadystatechange事件内部，有可能对dom进行操作。此时浏览器便会挂起js引擎线程，转而执行GUI渲染线程，进行UI repaint者reflow。当js引擎重新
执行时，GUI渲染线程又会被挂起，GUI更新将会被保存起来，等到js引擎空闲时立即被执行；
   所以整个ajax请求过程中，有涉及到浏览器的四种线程，只有GUI渲染线程和js引擎线程是互斥的。其他线程之间都是可以并行执行的。通过这种方式，ajax并没有破坏
js的单线程机制。

## ajax与setTimeout排队问题

   通常，ajax和setTimeout的事件回调都是被同等的对待，按照顺序自动被添加到任务队列的末尾，等待js引擎空闲时执行。但是，并非所有的回调执行都滞后于setTimeout
的回调;
   由于ajax异步，setTimeout回调本应该最先被执行，然而实际上，一次ajax请求，并非所有的部分都是异步的，至少'readyState == 1'的onreadystatechange回调
以及onloadstart回调就是同步执行的.

## XHR一级
    
   XHR1时，xhr对象具有如下缺点：
   * 仅支持文本数据传输，无法传输二进制数据；
   * 传输数据时，没有进度信息提示，只能提示是否完成；
   * 受浏览器同源策略限制，只能请求同源资源；
   * 没有超时机制，不方便掌握ajax请求节奏；
   
## XHR二级(IE10及更高版本)

   * 支持二进制数据，可以上传文件，可以使用FormData对象管理表单；
   * 提供进度提示，可以通过xhr.upload.onprogress事件回调函数获取传输进度；
   * 依然受同源策略限制，这个安全机制不会变。XHR2新提供Access-Control-Allow-Origin等headers，设置为＊时表示允许任何域名请求，从而实现跨域CORS访问；
   * 可以设置timeout及ontimeout，方便设置超时时长和超时后续处理；
   
## $.ajax
   
   $.ajax是jquery对原生ajax的一次封装。通过封装ajax，jquery抹平了不同浏览器异步http的差异性，取而代之的是高度统一的api。$.ajax()只有一个参数，该参数为
key－value设置对象。实际上，jquery发送的所有ajax请求，都是通过该ajax方法实现的。

## Axios
   
   实际上，如果仅仅是想使用一个不错的http库，精致的Axios相比较于庞大臃肿的jquery会更合适：
   * Axios支持node，jquery并不支持；
   * Axios基于promise语法；
   * Axios更适合http场景，jquery大而全，加载较慢；
   
## Fetch

   说到ajax，就不得不说一下fetch，具体内容请看下篇博客；
   
## 什么是CORS

   CORS是一个W3C标准，跨域资源共享。允许浏览器向跨域服务器，发出异步http请求，从而克服了ajax受同源策略的限制。实际上，浏览器并不会拦截不合法的跨域请求，而是拦截了
他们的响应。

### HTML启用CORS

   http-equiv相当于http的响应头，他回应给浏览器一些有用的信息，以帮助正确和精确地显示网页内容。下面的html将允许任意域名下的网页跨域访问：
    `<meta http-equiv="Access-Control-Allow-Origin" content="*">`
