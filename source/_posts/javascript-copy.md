---
title: javascript-copy
date: 2016-10-25 19:33:22
tags: [javascript,copy]
---

## <i class="icon-home"></i>该篇博客的由来

1. 需求第一,有这个需求;
2. 网上`copy`不到可用的代码,如果能找到傻子才自己写呢;
3. 一直想搞个博客,终于将想法付诸于实践了,第一次撸博客,很开心有木有;

  为了实现这个需求,我这个前端小菜在网上看了很多博文,查了很多资料,奈何很多都是用的`window.clipboardData.setData()`之类的,现在的浏览器好像都不支持这个属性的;
另外有些就是写得很杂很乱,没有封装成一个方法,并且兼容性不好;所以,为了自己以后用得方便,就封装了一个自认为不错的方法(至少满足了自己的需求):`copyText`代码奉上,欢迎指正:

## <i class="icon-pencil"></i>代码奉上

<!--more-->

```
function copyText(ele){
          function otherEle(element){
              if (document.selection) {
                  var range = document.body.createTextRange();
                  range.moveToElementText(element);
                  range.select();
              }else{
                  window.getSelection().removeAllRanges();
                  var range = document.createRange();
                 range.selectNode(element);
                 window.getSelection().addRange(range);
             }
         }
         if(ele.select){
             ele.select();
         }else{
             otherEle(ele);
         }
         document.execCommand('Copy');
         window.getSelection().removeAllRanges();
     }

```

该方法只需要传入一个参数(你需要复制内容的DOM元素);
