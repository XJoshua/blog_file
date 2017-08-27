---
title: react-lifecycle
date: 2017-03-16 15:28:20
tags: [react, component, lifecycle]
---

# react生命周期详解
  我们在使用react时，往往需要在组件渲染之前进行一些数据操作，来保证我们视图中展示的信息是正确的；那么，在哪里或者在什么时候操作数据才会被视图渲染时展示我们
修改后的数据信息呢？操作数据的代码写在哪里才是正确的呢？这些问题在我们了解了组件的生命周期后就会豁然开朗了；

<!--more-->

## 组件的各个生命周期
* componentWillMount(){}

  Mounting安装阶段，在客户端和服务器上，在初始渲染发生之前立即调用一次，在这个方法中可以调用setState，render()函数将会渲染最新的的状态，但是只会执行一次；
    
* componentDidMount(){}

  Mounting安装阶段，只在客户端调用一次，在初始渲染发生后立即执行；
    
* componentWillReceiveProps(nexProps){}

  Updating更新阶段，在组件接收新props时调用，初始渲染不调用此方法，旧的props可以使用this.props查看，新的prop使用nextProps查看，在此函数中调用
this.setState()不会触发附加的渲染；
    
* shouldComponentUpdate(nextProps, nextState){}

  Updating更新阶段，当正在接收新的状态时，在渲染之前调用；此方法必须返回false或者true，否则报错，true则渲染，false则不渲染；在此生命周期中考虑是否进行渲染，避免不必要的性能浪费；
如果返回false，render()将会被完全跳过，直到下一个状态改变。另外，不会调用componentWillUpdate和componentDidUpdate

* componentWillUpdate(nextProps, nextState){}

  Updating更新阶段，当正在接收新的props或state时立即调用，初始渲染不调用此方法；

* componentDidUpdate(nextProps, nextState){}
    
  Updating更新阶段，在组件的更新刷新到DOM后立即调用，初始渲染不调用此方法；
    
* componentWillUnmount(){}

  Unmounting卸载阶段，在组件从DOM卸载前立即调用，在此方法中执行任何必要的清理；
