---
title: nodeSpider
date: 2017-02-08 14:32:28
tags: [node, spider, javascript, es6]
---
# 爬虫爬取豆瓣电影(Node.js)

初学Node.js，实现对豆瓣电影top250进行爬取，参照[Node.js爬取豆瓣电影](http://kuka.im/2016/05/16/nodejs-spider/)

## 引入模块
```javascript
        'use strict';
        let https = require('https'),
            fs = require('fs'),
            path = require('path'),
            cheerio = require('cheerio');
```

## 创建https get请求
```javascript
    function spiderMovie(index) {
        https.get('https://movie.douban.com/top250?start=' + index, (res) => {
            let pageSize = 25;
            let html = '';
            let movies = [];
            res.setEncoding('utf-8');
    
            res.on('data', (chunk) => { html += chunk; });
    
            res.on('end', () => {
                let $ = cheerio.load(html);
                $('.item').each(() => {
                    let picUrl = $('.pic img', this).attr('src');
                    let movie = {
                        title: $('.title', this).text(),
                        start: $('.info .star .rating_num', this).text(),
                        link: $('a', this).attr('href'),
                        picUrl: picUrl
                    };
    
                    if (movie) {
                        movies.push(movie);
                    }
    
                    downloadImg('./img/', movie.picUrl);
                });
    
                saveData('./data' + (index / pageSize) + '.json', movies);
            });
        }).on('error', (err) => {
            console.log(err);
        });
    }
```
<!--more-->

## 下载图片
```javascript
        function downloadImg(imgDir, url) {
        https.get(url, (res) => {
            let data = '';
            res.setEncoding('binary');
            res.on('data', (chunk) => {
            data += chunk;
        });
    
        res.on('end', () => {
            fs.writeFile(imgDir + path.basename(url), data, 'binary', (err) => {
                if (err) {
                    return console.log(err);
                }
                console.log('Image downloaded: ', path.basename(url));
            });
        }).on('error', (err) => {
                console.log(err);
            });
        });
    }
```

## 保存数据到本地
```javascript
        function saveData(path, movies) {
            console.log(movies);
        
            fs.writeFile(path, JSON.stringify(movies, null, ' '), (err) => {
                if (err) {
                    return console.log(err);
                }
                console.log('Data saved');
            });
        }
```

## 创建爬取生成器
```javascript
        function *doSpider(x) {
            let start = 0;
            console.log(start + '_________');
            while (start < x) {
                yield start;
                spiderMovie(start);
                start += 25;
            }
        }
```

## 执行爬取方法
```javascript
    for (let x of doSpider(250)) {
        console.log(x);
    }
```

## 详细代码已上传到本人[github](https://github.com/barefootChild/nodeSpider)