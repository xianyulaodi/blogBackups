---
title: 以setTimeout来聊聊Event Loop
date: 2017-02-26 13:35
categories:
tags:
     - 博客园迁移
     - setTimeout
     - Eventloop
toc: true
---

> 平时的工作中，也许你会经常用到setTimeout这个方法，可是你真的了解setTimeout吗？本文想通过总结setTimeout的用法，顺便来探索javascript里面的事件执行机制。


<!--more-->

## 1、setTimeout基本用法
---
1、`setTimeout(code,millisec)`
　　setTimeout函数接受两个参数，第一个参数code是将要推迟执行的函数名或者一段代码，第二个参数millisec是推迟执行的毫秒数。
例如：
```javascript
setTimeout('console.log(2)',100);
setTimeout(function(){console.log(2)},100);
```
　　如果直接在setTimeout中直接执行代码， 需要以字符串的形式去写，引擎内部会将字符串转为可执行的代码

2、再来一些简单些的代码
```javascript
console.log(1);
setTimeout('console.log(2)',1000);
console.log(3);
```
是的，如你所愿，依次输出的是  `// 1 3 2`

3、代码升级版
```javascript
console.log(1);

setTimeout(function(){
    console.log(2);
},300);

setTimeout(function(){
    console.log(3)
},400);

for (var i = 0;i<10000;i++) {
    console.log(4);
}
setTimeout(function(){
    console.log(5);
},100);
```
这个时候的输入顺序是怎样的呢？这里先埋个伏笔，因为我们是以setTimeout来聊Event Loop

## 2、Event Loop简介
---
#### 什么是Event Loop呢？
&ensp;&ensp;&ensp;&ensp;因为javascript是单线程的，所谓的单线程是指在JS引擎中负责解释和执行JavaScript代码的线程只有一个，可以叫它为主线程。
&ensp;&ensp;&ensp;&ensp;除了主线程，还存在其他的线程。例如：处理AJAX请求的线程、处理DOM事件的线程、定时器线程、读写文件的线程(例如在Node.js中)等等。我们以setTimeout为例，当在代码中调用setTimeout()方法时，注册的延时方法会交由浏览器内核其他模块（以webkit为例，是webcore模块）处理，当延时方法到达触发条件，即到达设置的延时时间时，这一延时方法被添加至任务队列里。这一过程由浏览器内核其他模块处理，与执行引擎主线程独立，执行引擎在主线程方法执行完毕，到达空闲状态时，会从任务队列中顺序获取任务来执行，这一过程是一个不断循环的过程，称为事件循环模型。
![](//images2015.cnblogs.com/blog/776370/201702/776370-20170226101214523-381417596.png)
　　Javascript执行引擎的主线程运行的时候，产生堆（heap）和栈（stack）。程序中代码依次进入栈中等待执行，当调用setTimeout()方法时，即图中右侧WebAPIs方法时，浏览器内核相应模块开始延时方法的处理，当延时方法到达触发条件时，方法被添加到用于回调的任务队列，只有执行引擎栈中的代码执行完毕，主线程才会去读取任务队列，依次执行那些满足触发条件的回调函数。
　　在上图中的callback queue中指的是 "任务队列"，也可以理解为消息的队列，“消息“我们可以简单理解为是：注册异步任务时添加的回调函数。

例如：
```javascript
setTimeout(function(){
    console.log(‘hello’);
},100);
```
&ensp;&ensp;&ensp;&ensp;其中里面的function(){console.log('hello')}就是一个消息，任务队列里面保存的就是这些回调函数

## 3、理解js代码的执行
---
我们以一段代码的运行来进行理解，代码如下：
```javascript
console.log('start');

//Timer1
setTimeout(function(){
    console.log('hello');
},200);

//Timer2
setTimeout(function(){
    console.log('world');
},100);

console.log('end');
```
代码运行的gif图如下：
![](//images2015.cnblogs.com/blog/776370/201702/776370-20170226112523241-1162148115.gif)

我们分步骤来进行这个过程解答
　　 1、js执行引擎开始执行上述代码时，会先讲一个main()方法加入执行栈。首先第一个console.log(‘start’)入栈，console.log方法是一个webkit内核支持的普通方法，而不是前面图中WebAPIs涉及的方法，所以这里log('start')方法立即出栈被引擎执行。
　　2、引擎继续往下，将setTimeout(callback,200)添加到执行栈。setTimeout()方法属于事件循环模型中WebAPIs中的方法，引擎在将setTimeout()方法出栈执行时，将延时执行的函数交给了相应模块，即图右方的timer模块来处理。
　　3、然后主线程继续向下执行，紧接着将第二个定时器也交给Timer模块，然后执行到第二个console.log()，控制台打印'end'，
　　4、执行完毕后清空执行栈。但是并没有结束，在主线程执行的同时，Timer模块会检查其中的异步代码，一旦满足触发条件，就会将它添加到任务队列中。Timer2延迟100ms，所以会早于Timer1被添加到队列排头。而主线程此时处于空闲状态，所以会检查任务队列是否有待执行的任务。此时会将Timer2回调中的console.log()执行，控制台打印'world'，然后执行栈空闲后继续检查任务队列，将Timer1的代码压入执行栈中执行，控制台打印'hello'，清空执行栈，此时任务队列为空，执行结束,程序处理完毕，main()方法也出栈。
　　5、在这里再次强调一下，不是setTimeout加入了事件队列，而是setTimeout里面的回调函数加入了事件队列

## 4、setTimeout问题的解答
---
回到我们文章之初的那倒题：
```javascript
console.log(1);
//Time1
setTimeout(function(){
    console.log(2);
},300);
//Time2
setTimeout(function(){
    console.log(3)
},400);

for (var i = 0;i<10000;i++) {
    console.log(4);
}
//Time3
setTimeout(function(){
    console.log(5);
},100);
```
如果理解了上面的内容，那么这道题理解起来就比较容易了。
　　首先是打印出 1，然后是10000个4，那么Time1、Time2、Time3是顺序是如何的呢？
　　在这个代码中，for循环比较耗时，在Time1和Timer加入到执行队列中后，主线程依然还在执行for循环中的代码，处于阻塞状态。队列中的Time1和Time2并不会得以执行。当for循环结束，这时才将Time3交由Timer模块去管理，清空执行栈。虽然在这里Time3的延迟时间最短，但是加入任务队列后还是会排在Time1和Time2的后面，所以此时按顺序执行任务队列中的代码，依次打印2、3、5。
所以执行结果为：
![](//images2015.cnblogs.com/blog/776370/201702/776370-20170226131730320-9660993.jpg)

## 5、关于setTimeout新的问题
```javascript
console.log(1);
//Time1
setTimeout(function(){
    console.log(2);
},300);
//Time2
setTimeout(function(){
    console.log(3)
},400);

for (var i = 0;i<10000;i++) {
    console.log(4);
}
//Time3
setTimeout(function(){
    console.log(5);
},100);
```
　　上面这个问题中，Time3加入任务队列的时间比Time2,Time1晚，所以它是最后才执行的。那么问题来了，请看下面代码：
```javascript
console.log(1);

//Time2
setTimeout(function(){
    console.log(3)
},400);

//Time1
setTimeout(function(){
    console.log(2);
},300);

for (var i = 0;i<10000;i++) {
    console.log(4);
}
//Time3
setTimeout(function(){
    console.log(5);
},100);
```
&ensp;&ensp;&ensp;&ensp;我们将Time1和Time2的顺序对换一下，按照前面的说法，Time2先加入任务队列，然后是Time1，再然后是Time3。可是执行的结果还是1、4、2、3、5，这是为什么呢？虽然Time1的执行时间短，可是它比Time2晚加入任务队列啊。
&ensp;&ensp;&ensp;&ensp;为了验证这个问题，我们可以提出这样的一个假设：
`如果setTimeout加入队列的阻塞时间大于两个setTimeout执行的间隔时间，那么先加入任务队列的先执行，尽管它里面设置的时间比另一个setTimeout的要大`

可能假设听起来比较拗口，我们可以用代码来理解一下：

**代码1：**

```javascript
//Time2
setTimeout(function(){
    console.log(2);
},400);

var start=new Date();
for (var i = 0;i<5000;i++) {
    console.log('这里只是模拟一个耗时操作');
};
var end=new Date();
console.log('阻塞耗时：'+Number(end-start)+'毫秒');

//Time1
setTimeout(function(){
    console.log(3)
},300);

```
Time1比Time2设定的执行时间早100ms，但是Time2先加入任务队列，在Time2和Time1时间有一个阻塞的for循环，执行结果如下：

![](//images2015.cnblogs.com/blog/776370/201702/776370-20170227132406766-1770219106.png)

Time2先执行；

 
**代码2：**
我们把for循环里面的时间设置短一点：

```javascript
setTimeout(function(){
    console.log(2);
},400);

var start=new Date();
for (var i = 0;i<500;i++) {
    console.log('这里只是模拟一个耗时操作');
};
var end=new Date();
console.log('阻塞耗时：'+Number(end-start)+'毫秒');

//Time1
setTimeout(function(){
    console.log(3)
},300);

```
![](//images2015.cnblogs.com/blog/776370/201702/776370-20170227132754095-1066713665.png)

此时，Time1先执行，因为阻塞的耗时小于Time1和Time2的执行间隔时间100毫秒；<br />

**代码3：**
我们再来验证一下，把Time2的执行时间设为350毫秒；
```javascript
//Time2
setTimeout(function(){
    console.log(2);
},350);

var start=new Date();
for (var i = 0;i<500;i++) {
    console.log('这里只是模拟一个耗时操作');
};
var end=new Date();
console.log('阻塞耗时：'+Number(end-start)+'毫秒');

//Time1
setTimeout(function(){
    console.log(3)
},300);
```
直接结果为：
![](//images2015.cnblogs.com/blog/776370/201702/776370-20170227133015141-186210832.png)
Time2先执行，因为阻塞的时间大于两个setTimeout之间的间隔时间。

　　通过上面的假设，我们可以得出这样一个结论：**如果setTimeout加入队列的阻塞时间大于两个setTimeout执行的间隔时间，那么先加入任务队列的先执行，尽管它里面设置的时间可能比另一个setTimeout的要大**

## 总结
 　　理解js的事件循环在平时的工作中还是挺有用的，它可以让我们清楚的知道事件的执行顺序，知道事件的走向，才能更好的驾驭Javascript。本文是对事件循环的一个小小总结，更多的干货，可以看看下面的参考文档。本文有误之处，欢迎指出<br /><br />

 
参考文档：

[【转向Javascript系列】从setTimeout说事件循环模型](//www.alloyteam.com/2015/10/turning-to-javascript-series-from-settimeout-said-the-event-loop-model/)
[JavaScript 运行机制详解：再谈Event Loop](//www.ruanyifeng.com/blog/2014/10/event-loop.html)
[JavaScript：彻底理解同步、异步和事件循环(Event Loop)](https://segmentfault.com/a/1190000004322358)