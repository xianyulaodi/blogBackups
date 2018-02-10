---
title: BigPipe设计原理与技术实现
date: 2018-02-10 12:18
categories:
tags: [BigPipe,node]
toc: true
---

## 背景： 
    BigPipe其实是很久之前就出现过的概念，只是最近在好一些文章中有看到，所以打算研究一波。
2010年初的时候，Facebook将他们的个人主页加载由原来的5秒减为了2.5秒，带来了很好的用户体验，并且在这次的优化项目中，提出了一种新的页面加载技术，称为BigPipe


## 1. 什么是BigPipe

   在介绍BigPipe之前，我们先来看看传统页面渲染的一个问题：

   传统页面交互模型是按照一定顺序的，即，当服务器端获取数据并生成页面的时候，客户端被闲置，等待服务器端生成数据；当客户端接收到服务前端返回的页面并开始下载资源，解析页面的时候，服务器又在等待来自客户端的下一次请求。空闲时间造成资源的浪费。

   也就是说，正常的一个网页，是按照下面的顺序进行执行的：
1. 浏览器发送HTTP请求给服务器
2. 服务器解析请求，从存储层（缓存、数据库等）获取到数据，生成HTML页面，给客户端发送HTTP响应。
3. 客户端解析响应，开始构建DOM tree，然后开始下载CSS和JavaScript。
4. CSS下载完毕，解析CSS并继续生成DOM tree。
5. JavaScript下载完毕，被浏览器解析并执行。


   在一个请求了分割页面为多个Pagelet小块，然后分段输出到浏览器，前后端并行处理

   让页面分块，逐步呈现

  BigPipe是一个重新设计的基础动态网页服务体系。大体思路是，分解网页成叫做Pagelets的小块，然后通过Web服务器和浏览器建立管道并管理他们在不同阶段的运行。这是类似于大多数现代微处理器的流水线执行过程：多重指令管线通过不同的处理器执行单元，以达到性能的最佳。虽然BigPipe是对现有的服务网络基础过程的重新设计，但它却不需要改变现有的网络浏览器或服务器，它完全使用PHP和JavaScript来实现。


## 2. BigPipe的原理

## 3. 用express实现BigPipe
  
   并考虑注意点： 1. SEO 2. 

参考： 

1. 高性能页面加载技术--BigPipe设计原理及Java简单实现
https://www.cnblogs.com/jaylon/p/4924703.html  

2.Bigpipe 减少首屏渲染时间的方案。产出一篇文章 
参考： https://github.com/lcxfs1991/blog/issues/6 、 
http://taobaofed.org/blog/2016/03/25/seller-bigpipe-coding/ 
  周六任务，写一篇关于bigpipe的文章

3. 用node.js实现BigPipe 
https://www.cnblogs.com/axes/p/4369510.html

后(懒)渲染机制
5. https://segmentfault.com/a/1190000002940886  BigPipe 学习

4. Facebook让网站速度提升一倍的BigPipe技术分析  http://limu.iteye.com/blog/765173
