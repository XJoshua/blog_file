---
title: constructor
date: 2017-03-17 13:53:33
tags: [constructor, prototype, this, javascript]
---

# 构造函数
    在`JavaScript`中，new关键字可以让一个函数变得与众不同；eg:
```javascript
    function demo() {
        console.log(this);
    }
    demo(); // window
    new demo(); // demo
```
    很显然，使用new之后，函数内部发生了一些微妙的变化，改变了this的指向；但是new关键字到底做了什么呢？可以简单地用四句话表述：

1. 声明一个中间对象Object；
2. 将中间对象的原型指向构造函数的原型；
3. 将构造函数的this指向该中间对象,并执行构造函数内部的代码；
4. 返回该中间对象，即返回实例对象；

这四句话该如何用代码表示呢？

<!--more-->

```javascript
   //首先创建一个构造函数
   var Person = function(name, age) {
        this.name = name;
        this.age = age;
        this.getName = function () {
            return this.name;
        }
   }
   //模拟new关键字的执行过程，将构造函数以参数的形式传入
   function New(func) {
    //声明一个中间对象，该对象为最终返回的实例
        var res = {};
        if (func.prototype !== null) {
            //将该中间对象的原型指向该函数的原型
            res.__proto__ = func.prototype;
        }
        //ret为构造函数执行的结果，这里运用apply，将构造函数内部的this指向res，并执行构造函数内部的代码
        var ret = func.apply(res, Array.prototype.slice.call(arguments, 1));
        //如果我们在构造函数中指明了返回对象时，那么new的执行结果就是该返回对象
        if ((typeof ret === 'object' || typeof ret === 'function') && ret !== null) {
            return ret;
        }
        //如果没有明确指明返回对象，则默认返回res，这个res就是实例对象
        return res;
   }
   //通过new声明创建实例，这里的p1，实际接受的就是new返回的res
   var p1 = New(Person, 'zys', 23);
   console.log(p1.getName());   //zys
   console.log(p1 instanceof Person);   //true
```
    JavaScript内部再通过其他的特殊处理，将New(Person, 'zys', 23);等效于new Person('zys', 23);
    这样new关键字的神秘面纱就这样被揭开了！！！
    