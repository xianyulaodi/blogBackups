---
title: 手写一个router
date: 2017-06-18 23:05
categories:
tags: [router,js]
---

>	最近在玩vue,之前玩的react等，都有涉及到router的概念，router的入门不难，本着好奇的心态，想自己用js写一个router，纯属娱乐，应该不可用于实际项目

<!--more-->

## 前期知识准备
1. HTML5提供了一个新的API `History `
History: 接口允许操作浏览器的曾经在标签页或者框架里访问的历史记录。

2. window中有个`hashchange`事件，当 url 的 hash 发生变化时，会触发该事件

## 先来个简单版本
我们的ruoter先来个简单点的，暂时不用H5的新api，而是通过url中的hashg改变，触发hashchange事件来执行。

思路：
1. 首先我们需要一个容器来存储我们更新url时的回调函数。
2. 当执行当前url对应的回调函数时，我们需要更新我们的页面
3. 我们需要监听url中hash的改变

根据我们上面的思路，我们可以构思出下面的代码
```javascript
function Router() {
	this.routers = {};    //存储路由回调函数,以传入的路径为key,callback为value
	this.currentUrl = ''; //当前路由
}
// 注册路由路径和存储回调函数
Router.prototype.route = function(path,callback) {
	this.routers[path] = callback || function() {}
}
// 更新页面，其实就是执行注册的回调函数
Router.prototype.refresh = function() {
	this.currentUrl = location.hash.slice(1) || '/';
	this.router[this.currentUrl]();
}
Router.prototype.init = function() {
	window.addEventListener('load',this.refresh.bind(this),false);
	window.addEventListener('hashchange',this.refresh.bind(this),false);
}

window.Router = new Router();
window.Router.init();
```
经过上面的代买，我们搭建起了一个简单的路由，接下来我们来实战一下
```html
<ul> 
    <li><a href="#/">index</a></li> 
    <li><a href="#/page1">page1</a></li> 
    <li><a href="#/page2">page2</a></li> 
</ul> 
<p class="text"></p>

<script>
var text = document.querySelector('.text');
Router.route('/', function() {
	text.innerHTML = '这里是首页';
});
Router.route('/page1', function() {
    text.innerHTML = '这里是page1';
});
Router.route('/page2', function() {
    text.innerHTML = '这里是page2';
});
</script>
```
这样，我们点击不同路由，显示不同路由对应的文字

## 再来个复杂版本
通过上面的代码，我们完成了一个简单的js Router,接下来我们再来一个高级一点的版本，真正实现一个router库








































参考：
1. [一个轻量级的JS路由库——fd-router](http://www.feeldesignstudio.com/2016/01/fd-router-a-simple-javascript-router-for-spa/?utm_source=tuicool&utm_medium=referral)
2. [原生JS实现一个简单的前端路由（路由实现的原理）](http://blog.csdn.net/sunxinty/article/details/52586556)
3. [自己动手写一个前端路由插件](http://www.cnblogs.com/libin-1/p/5906526.html)
