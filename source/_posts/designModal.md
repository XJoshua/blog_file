---
title: 设计模式
date: 2017-12-21 18:01:01
tags: [javascript, 设计模式]
---

## 简单工厂设计模式

  简单工厂设计模式的使用场景就是用来创建一些相似的对象;

  ```
  function createBook(name, time, type) {
    var o = new Object();
    o.name = name;
    o.time = time;
    o.type = type;
    o.getName = function() {
      return this.name;
    }

    return o;
  }

  var book1 = crerateBook('js book', '2017/11/16', 'JS');
  var book2 = createBook('css book', '2017/11/13', 'CSS');

  book1.getName();
  book2.getName();
  ```
  
  ```javascript
  var Basketball = function() {
    this.info = '美国篮球';
  }
  Basketball.prototype = {
    constructor: Basketball,
    getMember: function() {
      console.log('每队需要五个成员');
    },
    getBallSize:function() {
      console.log('这个篮球不是很大');
    }
  };

  var Football = function() {
    this.info = '这是足球';
  }
  Football.prototype = {
    constructor: Football,
    getMember: function() {
      console.log('足球每队需要十一个人');
    },
    getBallSize: function() {
      console.log('我不喜欢足球');
    }
  };
  
  var sportsFactory = function(name) {
    switch (name) {
      case 'NBA':
        return new Basketball();
        break;
      case 'wordCup':
        return new Football();
        break;
    }
  }
  ```
## 工厂方法模式

  ```javascript
  var Factory = function(type, content) {
    if (this instanceof Factory) {
      var temp = new this[type](content);
    } else {
      return new Factory(type, content);
    }
  }
  
  Factory.prototype = {
    constructor: Factory,
    Java: function(content) {},
    JavaScript: function(content) {},
    UI: function(content) {
      this.content = content;
      (function(content) {
        var div = document.createElement('div');
        div.innerHTML = content;
        div.style.border = '1px solid red';
        document.getElementById('app').appendChild(div);
      })(content);
    }
  };
  ```
  创建多个类：
  
  ```javascript
  var data = [
    {type: 'JavaScript', content: 'JavaScript很重要'},
    {type: 'Java', content: 'Java is great'},
    {type: 'UI', content: 'UI...'}
  ];

  for(var i = 0, length = data.length; i < data.length; i++){
    Factory(data[i].type, data[i].content);
  }
  ```

    
