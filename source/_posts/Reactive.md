---
title: Reactive
date: 2017-09-19 14:11:52
tags: [vue, javascript, MVVM, reactive]
---

# 响应式原理
  何为响应式？对于前端MVVM来说，修改数据相应的视图更新即为响应式。接触Vue时，尤大大在文档中说过，vue的响应式原理依赖于Object.defineProperty,这也是vue不支持IE8及更低版本浏览器的原因。vue通过设定对象属性的setter／getter方法来监听数据的变化，通过getter进行依赖收集，每个setter方法就是一个观察者，在数据变更的时候通知订阅者更新视图。

<!--more-->

## vue如何将实例配置项设置为可观察的？
   ```javascript
      function observer(obj, cb) {
        Object.keys(obj).forEach((key) => {
          defineReactive(obj, key, config, cb)
        })
      }
      
      function defineReactive(obj, key, config, cb) {
        Object.defineProperty(obj, key, {
          enumerable: true,
          configurable: true,
          get: () => {
            return obj[key]; //返回值要根据该值的依赖决定
          },
          set: newVal => {
            obj[key] = newVal; //有可能会影响到他所依赖数据的值
            cb();
          }
        });
      }
      
      class Vue {
        constructor(options) {
          this._data = options.data;
          observer(this._data, options.render);
        }
      }
      
      let app = new Vue({
        el: '#app',
        data: {
          text: 'test'
        },
        render(){
          console.log('Hello Vue!');
        }
      });
   ```
   所以当修改this._data中的数据时就会触发set，订阅者就会被触发，vue里面就是实例的render函数；
   但是这时候修改数据就要调用app._data.text,如何实现操作app.text就会实现预期效果呢？vue内部实际实现了一层代理：
   
## Proxy
   在vue的构造函数Constructor中为data执行一个代理函数，就可以把data对象上的属性代理到vue实例上：
   ```javascript
      function _proxy(data) {
        const that = this;
        Object.keys(data).forEach(key => {
          Object.defineProperty(that, key, {
            configurable: true,
            enumerable: true,
            get: function proxyGetter(){
              return that._data[key];
            },
            set: function proxySetter(val){
              that._data[key] = val;
            }
          });
        });
      }
   ```
    
   
