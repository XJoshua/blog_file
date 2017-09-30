---
title: createStore
date: 2017-08-24 17:32:25
tags: [React, Redux]
---

# What is Store

* 用过Redux，Vuex等状态管理机之后，肯定对Store的理念有所了解；Store就是单页面应用中保存公用数据的容器；
* 应用中经常实用的state就是某个时间点Store生成的快照，可以通过store.getState()获得；
* 用户只能通过View层发送Action来更改Store中的公用数据，而store.dispatch(),是View发出Action的唯一办法；
* Store通过Reducer在接收到一个Action后，更新相应的公用数据；
* 数据更新后，对应的View也要做相应的更新，Store.subscribe(listener)设置监听函数，一旦数据变化,就自动执行这个函数,所以只要将View的更新函数放入listener，就会实现View层的自动渲染；
* store.subscribe()返回一个函数，执行此函数就可以解除监听:
  ```javascript
     var unsubscribe = store.subscribe();
     unsubscribe();
  ```  
<!--more-->

# What is createStore doing?

* 实际应用中要通过createStore函数生成一个Store，那这个函数到底做了什么呢？阮一峰老师解释的很清晰，我这边搬来做一下笔记:
  ```javascript
     const createStore = function(reducer){
     let state;
     let listeners = [];
      
     const getState = () => state;
     const dispatch = (action) => {
       state = reducer(state, action);
       listeners.forEach(listener => listener());
     };
      
     const subscribe = (listener) => {
       listeners.push(listener);
       return () => {
         listeners = listeners.filter(l => l !== listener);
       }
     };
      
     dispatch({});
      
     return {getState,dispatch,subscribe};
    }
  ```
    

