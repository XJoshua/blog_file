---
title: arithmetic-js
date: 2016-11-01 14:00:54
tags: [javascript, arithmetic, methods]
---

# <i class="icon-home"></i>javascript中常见的算法问题
  关于这些方法的总结,是本人在浏览前端干货时发现的有趣的东西,您可以点击[阅读原文](http://www.jackpu.com/qian-duan-mian-shi-zhong-de-chang-jian-de-suan-fa-wen-ti)
去查看原文<i class="icon-alarm"></i>(`原文有些方法不对,可亲测`);

## 判断一个单词是否是回文?

  >回文单词就是指该单词倒置后和原单词完全一样;
  
 ```
 function checkPalindrom(str){
   return str == str.split('').reverse().join(''); 
 }
 ```
 
## 去掉一组整型数组重复的值

 ```
  function unique(arr){
    let obj = {};
    let data = [];
    for (let i = 0, l = arr.length; i < l; i++) {
        if (!obj[arr[i]]) {
            obj[arr[i]] = true;
            data.push(arr[i]);
        }
    }
    return data;
  }
 ```
 
## 统计一个字符串出现最多的字母

```
    function findMaxDuplicateChar(str) {
        if (str.length === 1) {
            return str;
        }
        let obj = {};
        for (let i = 0, l = str.length; i < l; i++) {
            if (!obj[str.charAt(i)]) {
                obj[str.charAt(i)] = 1;
            } else {
                obj[str.charAt(i)] +=1;
            }
        }
        let maxChar = '',
            maxValue = 1;    
        for (let k in obj) {
            if (obj[k] >= maxValue) {
                maxValue = obj[k];
                maxChar = k;
            }
        }
        
        return maxChar;
    }
```

## 排序算法
<!--more-->
### 冒泡排序
```
    function bubbleSort(arr) {
      for (let i = 0, l = arr.length; i < l; i++) {
        for (let j = i+1; j < l; j++) {
          if (arr[i] > arr[j]) {
            let tem = arr[i];
            arr[i] = arr[j];
            arr[j] = tem;
          }
        }
      }
      
      return arr;
    }
```

### 快速排序
```
    function quickSort(arr) {
      if (arr.length <= 1) {
        return arr;
      }
      
      let leftArr = [];
      let rightArr = [];
      let q = arr[0];
      for (let i = 1, l = arr.length; i < l; i++) {
        if (arr[i] > q) {
          rightArr.push(arr[i]);
        } else {
          leftArr.push(arr[i]);
        }
      }
      
      return [].concat(auickSort(leftArr), [q], quickSort(rightArr));
    }
```

## 不借助中间变量,进行两个整数的变换

```
    function swap(a, b) {
      b = b - a;
      a = a + b;
      b = a - b;
      return [a,b];
    }
```
> es6(so easy!)

```
[a, b] = [b, a]
```

## 随机生成指定长度的字符串

```
function randomString(n) {
  let str = 'abcdefghijklmnopqrstuvwxyz9876543210';
  let tmp = '',
      i = 0,
      l = str.length;
  for (i = 0; i < n; i++) {
    tmp += str.charAt(Math.floor(Math.random() * 1));
  }
  
  return tmp;
}
```

## 找出正数数组的最大差值(原文对于该方法的书写是错误的,亲测,其它方法都很棒!值得追捧)

```
    function getMaxProfit(arr) {
        let minValue = Math.min(...arr);
        let maxValue = Math.max(...arr);
        return maxValue - minValue;
    }
```