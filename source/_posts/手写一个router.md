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
	this.routers[this.currentUrl]();
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
这样，我们点击不同路由，显示不同路由对应的文字 ,点击查看<a href="/demo/router1.html" target="_blank">demo</a>

## 再来个复杂版本
通过上面的代码，我们完成了一个简单的js Router,接下来我们再来一个稍微高级一点的版本，真正实现一个router库
我们封装一下我们的代码，

```javascript
;(function (global, factory) {

    if (typeof define === 'function' && (define.amd || define.cmd)) {
        //AMD/CMD
        define(function (global) {
            return factory(global);
        });
    } else if (typeof module === 'object' && typeof module.exports === 'object') {
        //CommonJS
        module.exports = factory(global);
    } else {
        //Browser
        global.Router = factory(global);
    }

}(typeof window !== 'undefined' ? window : this, function (window) {

var Router = {
	
    /**
     * 注册的所有路由对象
     */
    hashList: {},
	
	/**
	 * 当前路由
	 */
    index: null,
   

    /**
     * Add router
     * 注册路由对象
	**/
    add: function (path,callback) {
    	
        this.hashList[path] = callback;
    },
    
    /**
     * 跳转到指定路由
     */
    go: function(path) {
    	
    	window.location.hash = '#' + path;
    },
    
    /**
     * 删除路由
     */
    remove: function(path) {	 
    	
        delete this.hashList[path];
    },
    /**
     * 重新加载页面
     */
    reload:function() {
    	var self = this;
        var hash = window.location.hash.replace('#', '');
        var addr = hash.split('/')[0];
        var cb =   self.getCb(addr, self.hashList);
        if(cb != false) {
            var arr = hash.split('/');
            arr.shift();
            cb.apply(self, arr);
        } else {
            self.index && self.go(self.index);
        }
    },

 	/**
     * 设置主页地址
     * @param index: 主页地址
     */
    setIndex: function(index) {
        this.index = index;
    },
    
     /**
     * 获取callback
     * @return false or callback
     */
    getCb: function(addr, hashList) {
        for(var key in hashList) {
            if(key == addr) {
                return hashList[key]
            }
        }
        return false;
    },
    
    /**
     * 初始化路由
     */
    init: function (options) {
    	var self = this;
    	window.onhashchange = function() {
            self.reload();
        };
    },
    start: function() {
    	this.reload();
    }
};

return Router;
    
}));
```
使用方法：
```html
<a href="#index">首页</a>
<a href="#detail/1654499">详情页</a>
<script>
Router.init();
Router.add('index', function() {
    alert('这里是首页的内容');
    });
 
    Router.add('detail', function(id) {
    alert('这里是详情页，id为'+id);
});
Router.setIndex('index'); //设置首页
Router.start();
</script>
```
这里查看<a href="/demo/router2.html" target="_blank">demo</a>


&ensp;&ensp;&ensp;这样，我们的简单的路由器就完成了，后面我们将HTML5的history API搞进来，弄成一个即可用hash，也可以用history的版本

