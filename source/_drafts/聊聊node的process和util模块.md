---
title: 聊聊node的process和util模块
date: 2017-09-25 22:38
categories:
tags:
     - node
     - process
     - util
---

&ensp;&ensp;util是一个Node.js核心模块，提供常用函数的集合，用于弥补JavaScript的功能的不足。util模块设计的主要目的是为了满足Node内部API的需求，也就是将一些方法封装了起来。其中包括：格式化字符串、对象的序列化、实现对象继承等常用方法。
使用也比较简单，只需要require('util')即可。

https://nodejs.org/api/util.html#util_util
https://nodejs.org/api/process.html