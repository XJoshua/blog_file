---
title: functionCurrie
date: 2017-04-27 13:43:07
tags: [javascript, function, currie, arguments]
---

# javascript函数柯里化
  
  在计算机语言中，柯里化，就是把接受多个参数的函数变换成接受一个单一参数的函数，并且返回接受余下的参数而且返回结果的新函数的技术。
  eg:
  ```javascript
     function add(a, b) {
        return a + b;
     }
     function curryingAdd(a, b){
        return function(b) {
            return a + b;
        }
     }
     add(1, 2); // 3
     curryingAdd(1)(2); // 3
  ```
  上面的函数add是一般的函数，将传进来的参数进行相加；函数curryingAdd就是对函数add进行柯里化的函数；
  
  函数的柯里化能够让我们重新组合自己的应用，可以把复杂功能拆分成一个一个小的部分，每一个小的部分都是简单的，便于理解的，而且是容易测试的；
  
## 如何对函数进行柯里化？(涉及知识：闭包，高阶函数，不完全函数等)

* ### 蠢萌皮卡丘
  ```javascript
     function printInfo(name, song) {
        console.log(name + '最喜欢的歌曲是：' + song);
     }
     printInfo('Tom', '七里香');
     printInfo('Jerry', '雅俗共赏');
     function curryingPrintInfo(name) {
         return function(song) {
             console.log(name + '最喜欢的歌曲是：' + song);
         }
     }
     curryingPrintInfo('Tom')('七里香');
     curryingPrintInfo('Jerry')('雅俗共赏');
  ```
* ### 皮卡丘进化
  上面虽然实现了函数柯里化，但是当我们需要柯里化的时候，都要像上面那样不断地进行函数嵌套，那就是噩梦；所以下面就提供一个帮助其他函数进行柯里化的函数；
  ```javascript
     // 高阶函数
     function curryingHelper(fn) {
         var _args = Array.prototype.slice.call(arguments, 1);
         return function() {
             var _newArgs = Array.prototype.slice.call(arguments);
             var _totalArgs = _args.concat(_newArgs);
             return fn.apply(this, _totalArgs);
         }
     }
  ```
  检测上面辅助柯里化函数的正确性：
  ```javascript
     function showMsg(name, age, fruit) {
         console.log('My name is ' + name + ', I\'m ' + age + ' years old, ' + ' and I like eat ' + fruit);
     }
     curryingHelper(showMsg, 'zys')(22, 'apple'); // My name is zys, I'm 22 years old, and I like eat apple
     curryingHelper(showMsg, 'glj')(22, 'banana'); // My name is glj, I'm 22 years old, and I like eat banana
  ```
<!--more-->

* ### 皮卡丘超级进化
  上面的函数柯里化基本能满足我们的一般性需求了，但是如果我们希望那些经过柯里化的函数可以每次只传递一个参数，然后可以进行多次参数的传递，该怎么办呢？
  ```javascript
     function betterCurryingHelper(fn, len) {
         var length = len || fn.length;
         return function() {
             var allArgsFulfilled = (arguments.length >= length);
             // 如果参数全部满足，终止递归调用
             if (allArgsFulfilled) {
                 return fn.apply(this, arguments);
             } else {
                 var argsNeedFulFilled = [fn].concat(Array.prototype.slice.call(arguments));
                 return betterCurryingHelper(curryingHelper.apply(this, argsNeedFulFilled), length - arguments.length);
             }
         }
     }
  ```
  fn.length代表这个函数定义时的参数长度;下面检测一下上面更屌的辅助柯里化函数；
  ```javascript
     betterCurryingHelper(showMsg)('zys', 22, 'apple');
     betterCurryingHelper(showMsg)('zys')(22)('apple');
     betterCurryingHelper(showMsg)('zys', 22)('apple');
     betterCurryingHelper(showMsg)('zys')(22, 'apple');
  ```
* ### 皮卡丘究极进化（让人炸毛）
  最刺激的柯里化函数莫过于传参时不按照顺序了：
  ```javascript
     var _ = {};
     function crazyCurryingHelper(fn, length, args, holes) {
         var length = length || fn.length;
         var args = args || [];
         var holes = holes || [];
         
         return function() {
             var _args = args.slice();
             var _holes = holes.slice();
             
             var argLength = _args.length;
             var holeLength = _holes.length;
             
             var allArgumentsSpecified = false;
             
             var arg = null,
                 i = 0,
                 aLength = arguments.length;
             
             for(; i < aLength; i++) {
                 arg = arguments[i];
                 
                 if (arg === _ && holeLength) {
                     holeLength--;
                     _holes.push(_holes.shift());
                 } else if (arg === _) {
                     _holes.push(argLength + i);
                 } else if (holeLength) {
                     holeLength--;
                     _args.splice(_holes.shift(), 0, arg);
                 } else {
                     _args.push(arg);
                 }
             }
             
             allArgumentsSpecified = (_args.length >= length);
             if (allArgumentsSpecified) {
                 return fn.apply(this, _args);
             }
             
             return crazyCurryingHelper.call(this, fn, length, _args, _holes);
         };
     }
  ```
  检测一下上面比较疯狂的辅助柯里化函数：
  ```javascript
     crazyCurryingHelper(showMsg)(_, 22)('zys')('apple');
     crazyCurryingHelper(showMsg)(_, 22, 'apple')('zys');
     crazyCurryingHelper(showMsg)(_, 22, _)('zys', _, 'apple');
     crazyCurryingHelper(showMsg)('zys', _, _)(22)('apple');
     crazyCurryingHelper(showMsg)('zys')(22)('apple');
  ```
## 关于柯里化的性能

  使用柯里化意味着有一些额外的开销;这些开销一般涉及到这些方面,首先是关于函数参数的调用,操作arguments对象通常会比操作命名的参数要慢一点; 还有,在一
些老的版本的浏览器中arguments.length的实现是很慢的;直接调用函数fn要比使用fn.apply()或者fn.call()要快一点;产生大量的嵌套,作用域还有闭包会带来
一些性能还有速度的降低.但是,大多数的web应用的性能瓶颈时发生在操作DOM上的,所以上面的那些开销比起DOM操作的开销还是比较小的.
  
## 琐碎知识点

* fn.length:表示函数fn定义时参数的个数；
* arguments.callee:指当前运行的函数；
* fn.caller: 返回调用指定函数的函数；