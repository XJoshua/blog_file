---
title: mobileQuestion
date: 2017-03-17 16:10:03
tags: [mobile]
---

# 移动端屏幕适配的解决

   随着手机硬件配置的飞速发展、屏幕尺寸的越来越大和网络带宽的逐渐提升，越来越多的PC业务和服务在向移动端转移。然而在这种移动端的时代，该如何处理各终端
   屏幕的适配呢？
   
* 设置meta标签
  meta之viewport，其主要用来告诉浏览器如何规范的渲染Web页面，而你则需要告诉它视窗有多大。在开发移动端页面，我们需要在html中设置meta标签如下：
   
```html
   <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```
  该设置不允许用户通过点击或者缩放等操作对屏幕放大浏览，但是该设置已经在ios10以上的版本失效了，可以额外通过js事件进行控制：
```javascript
   window.onload=function() {
      document.addEventListener('touchstart', function(event) {
        if (event.touches.length>1) {
          event.preventDefault();
        }
      });
      var lastTouchEnd = 0;
      document.addEventListener('touchend', function(event) {
        var now=(new Date()).getTime();
        if (now - lastTouchEnd <= 300) {
          event.preventDefault();
        }
        lastTouchEnd=now;
      },false)
   }
```
   禁止ios上自动识别电话：
```html
   <meta content="telephone=no" name="format-detection" />
```
   禁止android上自动识别邮箱：
```html
   <meta content="email=no" name="format-detection" />
```
   解决ios上的safari上地址栏和顶端样式条：
```html
   <meta name="apple-mobile-web-app-capable" content="yes" />
   <meta name="apple-mobile-web-app-status-bar-style" content="black" />
```
* 打电话发短信
```html
   <a href="tel:1008611"></a>
   <a href="sms:10086"></a> 
```
* 手淘的flexible
  flexible是一个制作H5适配的开源库，需要在html中引入，可以直接使用阿里CDN：
```html
   <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script>
```
    执行这个JS后，会在元素上增加一个data－dpr属性，以及一个font-size样式。JS会根据不同的设备添加不同的data－dpr值，比如说1、2、3，同时会给html加上对应的font-size
    的值，比如说37.5px
    
* 放弃px拥抱rem
  em的计算是基于父级元素的，在实际中给我们的计算带来了很大的不方便。但是rem是始终基于根元素html的；刚刚我们引入了手淘的flexible，并给html加了font-size，如此以来，页面
中的元素就可以通过rem单位来设置了。会根据html元素的font-size值做相应的计算，从而实现屏幕的适配效果。
    
* css处理器(SASS)
  可以使用Sass的函数、混合宏来实现：

```sass
    @function px2rem($pr, $base-font-size: 75px){
        @return ($px / $base-font-size) * 1rem
    }
```
    这里的这个参数$base-font-size: 75px,可以通过设计稿宽度／10来计算；假如宽为750，则$base-font-size为75px；
    
    使用：
```scss
    .box {
      width: px2rem(190px);
      height: px2rem(190px);
    }
```
<!--more-->

# 移动端1px的解决方法

* what is 1px question?
  Retine屏的分辨率始终是普通屏幕的2倍，1px的边框在devicePixelRatio = 2的retina屏幕下会显示为2px。
    
    解决方案：
    `transform: scaleY()`
```html
    <div class="border-1px"></div>
    <style type="text/scss">
        .border-1px {
            position: relative;
            &:after {
                display: block;
                position: absolute;
                left: 0;
                bottom: 0;
                width: 100%;
                border-top: 1px solid #000;
                content: '';
            }
        }
        
        @media (-webkit-min-device-pixel-ratio: 1.5),(min-device-pixel-ratio: 1.5) {
            .border-1px{
                &::after {
                    -webkit-transform: scaleY(0.7);
                    transform: scaleY(0.7);
                }
            }
        }
        
        @media (-webkit-min-device-pixel-ratio: 2),(min-device-pixel-ratio: 2) {
            .border-1px {
                &::after {
                    -webkit-transform: scaleY(0.5);
                    transform: scaleY(0.5);
                }
            }
        }
    </style>
```
* css3过渡动画开启硬件加速
```sass
   .translate3d {
      -webkit-transform: translate3d(0, 0, 0);
      -moz-transform: translate3d(0, 0, 0);
      -ms-transform: translate3d(0, 0, 0);
      transform: translate3d(0, 0, 0);
   }
```
1. css3动画或者过渡尽量使用transform和opacity来实现动画；
2. 动画和过度尽量使用css3解决；

* 移动端click屏幕会产生200-300ms的延迟响应，所以尽量使用touchstart或者touchend替代click；

* 快速回弹滚动
  在ios上，如果存在局部滚动，就要加上下面的属性：
```sass
   -webkit-overflow-scrolling: touch;
```

* 谨慎使用fixed
  
  ios下fixed定位元素容易出错。软键盘弹出时，会影响fixed元素定位，会发生元素错位，滚动一下就会恢复，有时候会出现闪屏效果；

* 消除transition闪屏

```sass
   .no-flash {
      -webkit-transform-style: preserve-3d;
      -webkit-backface-visibility: hidden;
      -webkit-perspective: 1000;
   }
```
* ios系统去掉元素触摸时产生的半透明灰色遮罩,去掉默认input样式：

```sass
   a,button,input,textarea {
      -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
   }
   input, button, textarea {
      -webkit-appearance: none;
   }
```

* 左右滑动，避免界面跟着滑动：

1. body加上样式：touch-action: none;
2. touchstart时候可以阻止默认事件：event.preventDefault()

* vue-router与微信分享的问题

  分享发送链接是下面这样：
  http://www.example.com/dist/html#/index?utm_source=share
  http://www.example.com/dist/html#/index.html/index?utm_source=share
  但是分享出去的实际链接会有所变化：
  http://www.example.com/dist/html?from=***#/index&utm_source=share
  http://www.example.com/dist/html#/index.html?from=***#/index&utm_source=share
  
  解决方案：
1. 自定义分享URL地址
2. 避免使用但文件应用

* iPhoneX的齐刘海问题：

  [iPhoneX的缺口和css](https://www.w3cplus.com/css/the-notch-and-css.html)
  [借助CSS Shapes实现元素滚动自动环绕iPhoneX的刘海](http://www.zhangxinxu.com/wordpress/2017/09/css-shapes-outside-iphone-x-head/)
  [剖析ios11网页适配问题](https://mp.weixin.qq.com/s/6YSN3g86jcU22xwcloNk3A)
   
    