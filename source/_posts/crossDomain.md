---
title: crossDomain
date: 2017-12-23 02:37:03
tags: [cors, http, front-end]
---
## 什么是跨域
跨域是指浏览器不能执行其他网站的脚本。是由浏览器的同源策略造成的，是浏览器对JavaScript实施的安全限制；

同源策略（域名、协议名、端口号均为相同）限制了以下行为：
* Cookie、LocalStorage和IndexDB无法读取；
* DOM和JS对象无法获取；
* Ajax请求发送不出去；

<!--more-->

### jsonp跨域
jsonp跨域其实是Javascript设计模式中的一种代理模式；在html页面中通过相应的标签从不同域名下加载静态资源文件是被允许的；
```javascript
let script = document.createElement('script');
script.src = 'http://www.baidu.com/login?username=zys&callback=callback';
document.body.appendChild(script);
function callback(res) {
    console.log(res));
}
```

### document.domain + iframe跨域
要求主域名必须相同；

### postMessage跨域
这是由H5提出来的一个炫酷的API；这个API很简单，其中包括接受信息的Message事件和发送消息的postMessage方法，发送消息的postMessage方法是向外界窗口发送消息：
```javascript
otherWindow.postMessage(message,targetOrigin);
```
targetOrigin是目标窗口，也就是要给哪一个window发送消息；是window.frames属性的成员或者是window.open方法创建的窗口。message是要发送的消息，类型为String，Object，targetOrigin是限定消息的接受范围，可以使用＊表示不限制
```javascript
var onmessage = function(event) {
    var data = event.data;
    var origin = event.origin;
}
if (typeof window.addEventListener != 'undefined') {
    window.addEventListener('message', onmessage, false);
} else if (typeof window.attachEvent != 'undefined') {
    window.attachEvent('onmessage', onmessage);
}
```

### 跨域资源共享CORS
CORS是一个W3C标准，允许浏览器向跨源服务器，发出XMLHttpRequest请求；整个CORS通信过程都是浏览器自动完成的，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码是完全一样的。浏览器一旦发现AJAX请求跨域，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

### WebSocket协议跨域
WebSocket是H5一种新的协议，它实现了浏览器与服务器全双工通信，同时还允许跨域通讯；

### node代理跨域
node中间件实现跨域代理，是通过启用一个代理服务器实现数据的转发；
