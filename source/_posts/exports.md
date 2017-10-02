---
title: exports
date: 2017-10-02 17:30:15
tags: [node, ES6, CommonJS]
---

## js模块化
   
   js之前书写很混乱，各写各的代码，没有模块的概念，后来出现了CommonJS规范，该规范就是对js模块化的一个定义;CommonJS定义的模块分为：模块标识(module)、模块定义(exports)、模块引用(require)

## exports、module.exports、export、export default分别用在哪里？

1. require: node和ES6都支持的引入；
2. export/import: 只有ES6支持的导出和引入；
3. module.exports/exports: 只有node支持的导出；

<!--more-->

## node模块

  在node执行一个文件时，会在这个文件内生成一个exports和module对象，而module又有一个exports属性，它们指向同一块内存区域：
  `exports = module.exports = {}`
  ![exports&&module.exports](https://user-gold-cdn.xitu.io/2017/7/31/6227d4e0917f4af649d9f9e750eddb09?imageView2/0/w/1280/h/960)
  但是，使用require导入的内容是module.exports指向的内存块内容，所以使用exports时一定要注意它指向的引用和module.exports指向的是同一个引用，否则通过exports导出的内容将无效；
  
## ES6模块中的export和export default

1. export和export default均可以导出常量、函数、文件、模块等；
2. 在一个文件或模块中，export、import可以有多个，export default仅有一个；
3. 通过export方式导出，在导入时要加{},export default则不需要；
4. export可以直接导出变量表达式，export default不行;
eg:
```javascript
   export const a = 10; //export default 不允许这样使用
```
