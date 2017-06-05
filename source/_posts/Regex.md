---
title: Regex
date: 2017-06-05 16:39:47
tags: [Javascript, Regex]
---
## 正则表达式
   
   之前学习正则表达式时，还在犯嘀咕：学这玩意有什么用？验证邮箱，手机号？除了这些地方丝毫感觉不到用武之地；所以粗略地学了一下，现在忘得差不多了，遇到问题
直接就去百度了，直到我看到下面的这些用法，才发现自己的无知，不禁感叹果然正则大法好！

### 获取链接`http://www.baidu.com?name=jawil&age=23`name的value的值

   非正则实现：
   ```javascript
      function getParamName(attr) {
        var search = window.location.search;
        var param_str = search.split('?')[1];
        var param_arr = param_str.split('&');
        var filter_arr = param_arr.filter(function(ele) {
            return ele.split('=')[0] === attr;
        });
        
        return decodeURIComponent(filter_arr[0].split('=')[1]);
      }
      console.log(getParamName('name'));
   ```
   正则实现：
   ```javascript
      function getParamName(attr) {
        var match = RegExp(`[?&]${attr}=([^&]*)`).exec(window.location.search);
        
        return match && decodeURIComponent(match[1].replace(/\+/g, ' '));
      }
      console.log(getParamName('name'));
   ```
<!--more-->
   
### 数字格式化问题， 1234567890 --> 1,234,567,890
   
   非正则实现：
   ```javascript
        var test = '1234567890';
        function formatCash(str) {
            var arr = [];
            
            for (var i = 1; i < str.length; i++) {
                if (str.length % 3 && i === 1) {
                    arr.push(str.substr(0, str.length % 3));
                }
                if (i % 3 === 0) {
                    arr.push(str.substr(i - 2, 3));
                }
            }
            return arr.join(',');
        }
        console.log(formatCash(test));
   ```
   正则实现：
   ```javascript
        var test1 = '1234567890';
        var format = test1.replace(/\B(?=(\d{3})+(?!\d))/g, ',');
        console.log(format)
   ```
   下面简单分析一下正则/\B(?=(\d{3})+(?!\d))/g:
   1. /\B(?=(\d{3})+(?!\d))/g：正则匹配边界\B，边界后面必需跟着(\d{3})+(?!\d)；
   2. (\d{3})+：必需是1个或多个的3个连续数字；
   3. (?!\d): 第2步中的3个数字不允许后面跟着数字；
   4. (\d{3})+(?!\d): 所以匹配的边界后面必须跟着3*n(n>=1)的数字；
   最终把匹配到的所有边界换成,即可达成目标。
   
### 用正则去掉字符串左右两边的空格， ' zys cq' --> 'zys cq'(字符串原生的trim方法，也没毛病啊，不知道，原文作者为什么单独拿出来用正则实现了一下)
   
   ```javascript
        function regexTrim(str) {
            return str.replace(/(^\s*)|(\s*$)/g, '');
        }
        
        var str = ' zys cq ';
        console.log(regexTrim(str));
   ```
   
   看完大牛们的正则使用后，得给自己安排点时间重新学习Regex了！
   
   
