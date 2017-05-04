---
title: cookie
date: 2017-04-18 20:06:36
tags: [javascript, cookie]
---

# Cookie

  cookie是存储于访问者计算机中的变量，当该台计算机通过浏览器请求某个界面时，就会发送这个cookie。你可以使用JavaScript来创建和取回cookie的值；
cookie是访问过的网站创建文件，用于存储浏览信息，例如个人信息，账号密码等；
  从JavaScript的角度来看，cookie就是一些字符串信息。这些信息存在客户端的计算机中，用于客户端和服务器之间传递信息。在JavaScript中可以通过
document.cookie来读取或者设置这些信息。

## 基础知识

1. cookie是有大小限制的。每个cookie所存放的数据不能超过4k，如果cookie字符串的长度超过4k，则该属性将返回空字符串；
2. cookie最终是以文件形式存放在客户端计算机中的，可以对cookie进行查看和修改，所以cookie不能存放重要的信息；
3. cookie存在有效期，默认关闭浏览器，cookie的生命周期结束；当然cookie的有效期可以自定义；
4. cookie有域和路径的概念；一般情况下cookie不能跨域访问(可以通过一些手段达到二般情况)；路径问题就是指：一个网页创建的cookie只能被这个网页同一目录或者
子目录下的所有页面访问，而不能被其他目录下的网页访问。
5. 创建cookie和创建变量的方式相似，都需要使用cookie的名称和cookie值；同个网站可以创建多个cookie，多个cookie可以存放在同一个cookie文件中。

<!--more-->

## 常见问题

* cookie存在两种类型
    * 浏览的当前网站本身设置的cookie
    * 来自网页嵌入广告或图片等其他来源的，第三方cookie(网页通过使用这些cookie跟踪你的使用信息)
* cookie的生命周期
    * 临时性的cookie。使用过程中存储一些个人信息，浏览器关闭后cookie信息将会被删除；
    * 设置失效时间的cookie。失效时间根据自己需求设定(登录名和密码)；
* cookie的两种清除方式
    * 通过浏览器工具清除cookie(浏览器自身功能，有第三方工具)；
    * 设置cookie的有效期自动清除cookie；
    * 删除cookie有时候可能导致某些网页无法运行；
* 浏览器通过设置来接受和拒绝访问cookie；
* 尽量减少cookie的使用量，并且尽量使用小cookie，考虑到功能和性能问题；

## 设置cookie的有效期
   
   通过expires设置cookie的有效期：
   ```javascript
      document.cookie = 'name=zys;expires=date';
   ```
   上面代码的date的值为GMT格式的日期型字符串，生成方式：
   ```javascript
      var _date = new Date();
      _date.setDate(_date.getDate() + 30); // cookie有效期为30天
      _date.toGMTString();
   ```

# 高级篇

## 路径概念

   cookie一般是由用户访问的界面创建的，可是并不是只有在创建cookie的页面才可以访问该cookie，默认情况下同目录或者子目录的网页才可以访问；
   通过设置cookie的路径就可以实现该cookie被其他目录或者父级目录访问；
   eg：设置路径为根目录
   ```javascript
      document.cookie = 'name=zys;path=/'
   ```
   
## 域概念

   cookie实现同域访问的问题：
   使cookie在a:www.qq.com和b:sports.qq.com同时可以被访问
   ```javascript
      document.cookie = 'name=zys;path=/;domain=qq.com'
   ```
## 安全性
   
   一般情况下cookie的信息都是使用http连接传递数据的，这种方式很容易被查看，cookie存储的信息容易被获取；如果一个cookie的属性为secure，它就会通过
   https或者其他安全协议进行传输；
   ```javascript
      document.cookie = 'name=zys;secure';
   ```
   把cookie设置为secure，只保证cookie在与服务器间数据传输过程加密，保存在本地的cookie文件并不加密(可以自己加密)；
   
## 编码细节

   在输入cookie信息时，不能包含空格、分号、逗号等特殊符号，一般情况下，cookie信息的存储都是采用为编码的形式。所以在设置cookie信息之前，要先使用
   escape()函数将cookie信息进行编码，在取得到cookie值的时候，再使用unescape()函数把值转换回来：
   ```javascript
      document.cookie = name = '=' + escape(value);
      return unescape(document.cookie.substring(start, end));
   ```
