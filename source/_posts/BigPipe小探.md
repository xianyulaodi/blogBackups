---
title: BigPipe小探
date: 2018-02-10 12:18
categories:
tags: [BigPipe,node]
toc: true
---

## 背景： 
BigPipe其实是很久之前就出现过的概念，只是最近在好一些文章中有看到，所以打算研究一波。
2010年初的时候，Facebook将他们的个人主页加载由原来的5秒减为了2.5秒，带来了很好的用户体验，并且在这次的优化项目中，提出了一种新的页面加载技术，称为BigPipe

<!-- more-->

## 1. 什么是BigPipe

   在介绍BigPipe之前，我们先来看看传统页面渲染的一个问题：

   传统页面交互模型是按照一定顺序的，即，当服务器端获取数据并生成页面的时候，客户端被闲置，等待服务器端生成数据；当客户端接收到服务前端返回的页面并开始下载资源，解析页面的时候，服务器又在等待来自客户端的下一次请求。空闲时间造成资源的浪费。

   也就是说，正常的一个网页，是按照下面的顺序进行执行的：
1. 浏览器发送HTTP请求给服务器
2. 服务器解析请求，从存储层（缓存、数据库等）获取到数据，生成HTML页面，给客户端发送HTTP响应。
3. 客户端解析响应，开始构建DOM tree，然后开始下载CSS和JavaScript。
4. CSS下载完毕，解析CSS并继续生成DOM tree。
5. JavaScript下载完毕，被浏览器解析并执行。

BigPipe是为了解决上面的问题应用而生的，它把网页分割成多个PageLet的小块，然后分段输出到浏览器，前后端并行处理。

举个例子：在我们平时去饭店吃饭的时候，我们先选好桌子点好菜(确定页面布局和要展示的模块)，下单到厨房后，多个厨师开始炒菜(服务端并发)，做好一样端上来一样吃一样(客户端并发)。

![](/img/bigPipe1.jpg)

传统页面的渲染模式：
![](/img/BigPipe2.png)

使用BigPipe后的渲染模式
![](/img/BigPipe3.png)


## 2. BigPipe的原理
&nbsp;&nbsp;BigPipe的主要思想是实现浏览器和服务器的并发执行，实现页面的异步加载，从而提高页面的访问速度。
为了达到这个目的，它首先根据页面的功能或者位置，将页面分成若干个模块，这些模块的名字也被称为PageLet，并对这些分解的模块进行唯一的标识。然后通过Web服务器和浏览器之间建立管道，进行分段输出 （减少请求数）。

下面来看一个简单的例子：
  我们来看下面的代码：layout.html
  ```html
  <!DOCTYPE html>
<html>
<head>
  <script>
    var BigPipe = {
      view: function(selector,temp) {
        document.querySelector(selector).innerHTML= temp;
      }
    }
  </script>
</head>
<body>
    <div id="moduleA"></div>
    <div id="moduleB"></div>
    <div id="moduleC"></div>
  ```
  
  服务端代码,基于express
  ```javascript
var express = require('express');
var app = express();
var fs = require('fs');

app.get('/', function (req, res) {
  var layoutHtml = fs.readFileSync(__dirname + "/layout.html").toString();
  res.write(layoutHtml);
  
  // setTimeout只是模拟异步返回
  setTimeout(function() {
    res.write('<script>BigPipe.view("#moduleA","moduleA");</script>');
  100);

  setTimeout(function() {
    res.write('<script>BigPipe.view("#moduleC","moduleC");</script>');
  },200);

  setTimeout(function() {
    res.write('<script>BigPipe.view("#moduleB","moduleB");</script>');
    res.write('</body></html>');
  },300);
  
  res.end();
});

app.listen(3000);
  ```

## 3. BigPipe的应用场景
&nbsp;&nbsp;BigPipe并不是所有的场景都使用，如果你的首页不是特别复杂，大可不必使用。其次，你的页面必须分为不同的模块，并且每个模块之间都是独立了,比如现在的淘宝，就比较适用。
而且BigPipie需要服务端的支持，且由于所有的内容是动态插入的，所以可能对于SEO不太友好。如果服务端是由node.js来写，并且页面直接由Node.js来渲染的，用BigPipe比较的合适。
&nbsp;&nbsp;此文只是作为BigPipe的小小学习，至于更深入的学习，以后有用到再做进一步的研究
  

参考链接：
1. [新版卖家中心 Bigpipe 实践（一）](http://taobaofed.org/blog/2015/12/17/seller-bigpipe/)
2. [BigPipe 学习](https://segmentfault.com/a/1190000002940886)
3. [Facebook让网站速度提升一倍的BigPipe技术分析](http://limu.iteye.com/blog/765173)
4. [性能优化三部曲之三——Node直出让你的网页秒开](https://github.com/lcxfs1991/blog/issues/6)
