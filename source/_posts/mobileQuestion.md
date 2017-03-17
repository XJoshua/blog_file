---
title: mobileQuestion
date: 2017-03-17 16:10:03
tags: [mobile]
---

# 移动端屏幕适配的解决

   随着手机硬件配置的飞速发展、屏幕尺寸的越来越大和网络带宽的逐渐提升，越来越多的PC业务和服务在向移动端转移。然而在这种移动端的时代，该如何处理各终端
   屏幕的适配呢？
   
1. 设置meta标签
   
   meta之viewport，其主要用来告诉浏览器如何规范的渲染Web页面，而你则需要告诉它视窗有多大。在开发移动端页面，我们需要在html中设置meta标签如下：
   
```html
   <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```
2. 手淘的flexible

   flexible是一个制作H5适配的开源库，需要在html中引入，可以直接使用阿里CDN：
   
```html
   <script src="http://g.tbcdn.cn/mtb/lib-flexible/0.3.4/??flexible_css.js,flexible.js"></script>
```
    执行这个JS后，会在元素上增加一个data－dpr属性，以及一个font-size样式。JS会根据不同的设备添加不同的data－dpr值，比如说1、2、3，同时会给html加上对应的font-size
    的值，比如说37.5px
    
3. 放弃px拥抱rem
    
    em的计算是基于父级元素的，在实际中给我们的计算带来了很大的不方便。但是rem是始终基于根元素html的；刚刚我们引入了手淘的flexible，并给html加了font-size，如此以来，页面
    中的元素就可以通过rem单位来设置了。会根据html元素的font-size值做相应的计算，从而实现屏幕的适配效果。
    
4. css处理器(SASS)
    
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

1. what is 1px question?
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
    