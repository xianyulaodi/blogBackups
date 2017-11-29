---
title: javascript高阶函数
date: 2017-07-03 10:38
categories:
tags:
     - js
     - 高阶函数
toc: true
---

&ensp;&ensp;&ensp;&ensp;最近在看js设计模式相关的书籍，发觉很多模式都有用到高阶函数这个东东，发觉自己之前没有总结过这类文章，所以来个小小的总结。总结的篇幅有点少，纯属做做笔记

<!--more-->

## 1. 高阶函数的定义
高阶函数（Higher Order Function）,按照维基百科上面的定义，至少满足下列一个条件的函数：
* 1 函数作为参数传入
* 2 返回值为一个函数

只要满足上面两个条件中的其中一个，就属于高阶函数，我们下面用代码来更好的理解一下

** 函数作为参数传入 **
```javascript
function add(a,b,fn){
    return fn(a)+fn(b);
}
var fn=function (a){
  return a*a;
}
add(2,3,fn); //13
```
** 函数作为返回值 **
```javascript
var getSingle = function(fn) {
    var ret;
    return function() {
        return ret || (ret = fn.apply(this, arguments));
    };
};
```
## 2. 函数柯里化
为啥会在这里穿插函数柯里化呢，因为函数柯里化里面包含了很多的高阶函数，所以这里也一并总结总结

** 定义 **

&ensp;&ensp;柯里化（Currying），又称部分求值，一个currying的函数首先会接受一些参数，接受这些参数之后，函数**并不会立即求值**，而是继续返回另一个函数，刚才传入的参数在函数形成的闭包中被保存起来，待到函数被真正需要求值的时候，之前传入的所有参数都会被一次性用于求值。
&ensp;&ensp;比如有下面的场景：有个销售需要统计每天的销售额，待到月底的时候再统一计算该月的销售总额。
用代码的形式如下：
```javascript
var currying = function(fn) {
    var args = [];
    return function() {
      if(arguments.length === 0) {
        return fn.apply(this,args);
      } else {
          Array.prototype.push.apply(args,arguments);
          return arguments.callee;
      }
    }
}
var sell = function() {
  var sum = 0;
  for(var i = 0,len = arguments.length; i < len; i++) {
    sum += arguments[i];
  }
  return sum;
}
var sellAmount = currying(sell);
sellAmount(100);
sellAmount(200);
sellAmount(300);
console.log(sellAmount()); //600
```
上面的代码就是函数柯里化的一种形式，我传入了每天的销售额，并不会立即求值，而是等到月底，我需要统计总额的时候才一次性求值

## 3. 高阶函数实战
* 1.比较常用的一个地方,为一个元素添加事件;
```javascript
var addEvent = function(elem, type, handler) {
   if (window.addEventListener) {
      addEvent = function(elem, type, handler) {
        elem.addEventListener(type, handler, false);
      }
   } else if (window.attachEvent) {
      addEvent = function(elem, type, handler) {
        elem.attachEvent('on' + type, handler);
      }
  }
  addEvent(elem, type, handler);
};
```
* 2.函数节流
比如在window.resize中，不想被频繁的触发事件，想每隔一定的间隔事件来触发
```javascript
function throttle(fn, interval) {
  var doing = false;

  return function() {
    if (doing) {
      return;
    }
    doing = true;
    fn.apply(this, arguments);
    setTimeout(function() {
      doing = false;
    }, interval);
  }
}

window.onresize = throttle(function(){
    console.log('execute');
}, 500);
```
<br >
&ensp;&ensp;&ensp;&ensp;高阶函数在设计模式中用的还是比较多的，也是比较有用的一个东西，所以掌握它是比较有必要性的。
最近在看曾探写的《Javascript设计模式与开发实践》，非常不错，值得推荐



参考资料:《Javascript设计模式与开发实践》
