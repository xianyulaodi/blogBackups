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




Process模块提供了访问正在运行的进程。
child_process模块可以创建子进程，并与他们通信。
cluster模块提供了实现共享相同端口的集群服务能力，允许多个请求同时处理。
Process模块是一个无须使用require()就可以从node.js应用程序进行访问的全局对象。
//返回进程的当前工作目录
console.log('Current directory: ' + process.execPath);

//该进程的环境中指定的键/值对
console.log('Environment Settings: ' + JSON.stringify(process.env));
//用于启动Node.js应用程序的命令参数
console.log('Node Args: ' + process.argv);
//Node。js从中启动的绝对路径
console.log('Execution Path: ' + process.execPath);
//用于启动应用程序的特定节点的命令行选项
console.log('Execution Args: ' + JSON.stringify(process.execArgv));
//Node.js版本号
console.log('Node Version: ' + process.version);
//提供一个对象,包含Node.js应用程序所需的模块和版本
console.log('Module Versions: ' +  JSON.stringify(process.versions));
//用于编译当前节点可执行程序的配置选项
console.log('Node Config: ' +  JSON.stringify(process.config));
//当前进程ID
console.log('Process ID: ' + process.pid);
//当前进程标题
console.log('Process Title: ' + process.title);
//操作系统
console.log('Process Platform: ' + process.platform);
//进程正在运行的处理器体系结构
console.log('Process Architecture: ' + process.arch);
//Node.js进程的当前内存使用情况可使用util.inspect()读取
console.log('Memory Usage: ' + util.inspect(process.memoryUsage()));
//返回一个高精确的时间
var start = process.hrtime();
setTimeout(function() {
  var delta = process.hrtime(start);
  console.log('High-Res timer took %d seconds and %d nanoseconds', 
              delta[0], + delta[1]);
  console.log('Node has been running %d seconds', process.uptime());
}, 1000);