---
title: getElementPosition
date: 2017-02-07 15:05:53
tags: [javascript,elementPosition]
---
# 2017年以来的第一篇博客
  开春以来事情比较少就搞起来自己的博客吧！浏览了阮一峰老师关于如何用javascript获取界面元素位置一篇文章为了加深印象并做一个笔记，就取其精华把
知识写在自己的博客里方便自己查阅！
## 一、获取网页大小：
  网页上的每个元素都有`clientHeight`和`clientWidth`属性。这两个属性表示元素的`content`内容部分再加上`padding`的所占据的视觉面积，不包括
`border`和滚动条占用的空间。
  因此，document与纳素的clientHeight和clientWidth属性就代表了网页的大小：
  ```javascript
        function getViewport() {
            if (document.compatMode === 'BackCompat') {
                return {
                    width: document.body.clientWidth,
                    height: document.body.clientHeight
                }
            } else {
                return {
                    width: document.documentElement.clientWidth,
                    height: document.documentElement.clientHeight
                }
            }
        }
  ```
## 二、获取网页大小的另一种方法：
  网页上的每个元素还有scrollHeight和scrollWidth属性，只包含滚动条在内的该元素的视觉面积。
  所以，document对象的scrollHeight和scrollWidth属性就是网页的大小，但是当界面不出现滚动条时clientWidth和scrollWidth应该相等，但是实际上
不同的浏览器有不同的处理；所以，我们要取它们之间最大的那个：
  ```javascript
        function getPageArea() {
            if (document.compatMode === 'BackCompat') {
                return {
                    width: Math.max(document.body.scrollWidth, document.body.clientWidth),
                    height: Math.max(document.body.scrollHeight, document.body.clientHeight)
                }
            } else {
                return {
                    width: Math.max(document.documentElement.scrollWidth, document.documentElement.clientWidth),
                    height: Math.max(document.documentElement.scrollHeight, document.documentElement.clientHeight)
                }
            }
        }
  ```
  <!--more-->
## 三、获取网页的绝对位置：
  每个元素都有offsetTop和offsetLeft属性，表示该元素的左上角与父容器（offsetParent）左上角的距离；
  所以，只要对这两个元素进行迭代就可以得到该元素的绝对坐标；
  ```javascript
        function getElementLeft(ele) {
            let actualLeft = ele.offsetLeft;
            let current = ele.offsetParent;
            
            while (current) {
                actualLeft += current.offsetLeft;
                current = current.offsetParent;
            }
            
            return actualLeft;
        }
        
        function getElementTop(ele) {
            let actualTop = ele.offsetTop;
            let current = ele.offsetParent;
            
            while (current) {
                actualTop += current.offsetTop;
                current = current.offsetParent;
            }
            
            return actualTop;
        }
  ```
## 四、获取网页元素的相对位置：
  获取到绝对位置后，获取相对位置就很容易了，减去界面滚动条滚动的距离就可以了：
  ```javascript
        function getElementViewLeft(ele) {
            let actualLeft = ele.offsetLeft;
            let current = ele.offsetParent;
            
            while (current) {
                actualLeft += current.offsetLeft;
                current = current.offsetParent;
            }
            
            let elementScrollLeft = document.compatMode === 'BackCompat' ? document.body.scrollLeft : document.documentElement.scrollLeft;
            
            return actualLeft - elementScrollLeft;
        }
        
        function getElementViewTop(ele) {
            let actualTop = ele.offsetTop;
            let current = ele.offsetParent;
            
            while (current) {
                actualTop += current.offsetTop;
                current = current.offsetParent;
            }
            
            let elementScrollTop = document.compatMode === 'BackCompat' ? document.body.scrollTop : document.documentElement.scrollTop;
            
            return actualTop - elementScrollTop;
        }
  ```
## 五、获取元素位置的快速方法：
  getBoundingClientRect()方法，返回一个对象，其中包含了left,right,top,bottom四个属性，为相对位置，获取到相对位置之后就很容易获取绝对位置了！