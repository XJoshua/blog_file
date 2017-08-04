---
title: 'Debounce And Throttle '
date: 2017-08-04 09:38:25
tags: [javascript, Function, front-end]
---

## debounce

调用某函数一段时间后才会执行，如果在这段期间再调用该函数则重新计算执行时间：

```javascript
function debounce(time, func) {
  var previous;
  return function() {
    var that = this, args = arguments;
    clearTimeout(previous);
    previous = setTimeout(function(){
      func.apply(that, args);
    }, time);
  }
}
```

## throttle

设定一个函数执行时间间隔，只有当前执行时间与上次执行时间的时间间隔大于设定的执行时间间隔，函数才会执行：

```javascript
function throttle(delay, func) {
  var last = 0;
  return function() {
    var current = +(new Date());
    if (current - last > delay) {
      func.apply(this, func);
      last = current;
    }
  }
}
```