---
title: 浅析css中的cursor
date: 2017-07-19 23:00
categories:
tags:
     - css
toc: true
---

&ensp;&ensp;&ensp;&ensp;平时我们在css中cursor最多的就是`cursor:pointor`,今天在掘金点击一张图片放大缩小的时候，看了一下源码，发觉cursor中有一个zoom-in和zoom-out的值，可以实现一个放大缩小的鼠标手，觉得挺好玩，所以顺便也总结一下cursor中的一些属性。

<!--more-->

## cursor: zoom-in 和 cursor: zoom-out

正如我是在点击图片的时候看到这个属性的，zoom-in/zoom-out是实现放大缩小的效果

效果图如下：
![](http://image.zhangxinxu.com/image/blog/201412/2014-12-22_162640.png)![](http://image.zhangxinxu.com/image/blog/201412/2014-12-22_162706.png)

代码如下：
```css
.zoom-in {
    cursor: zoom-in;    /* 放大，也就是 + 号 */
}
.zoom-out {
   cursor: zoom-out; /* 缩小，也就是 -  号 */
}
```
可以点击查看 <a href="/demo/cursor-zoom.html" target="_blank">demo</a>

兼容性方面：
![](/img/zoom-in.png)
&ensp;&ensp;从图中我们可以看到，除了IE，其他浏览器的兼容性都特别好，也就是说，除了IE11及以下，其他浏览器都支持 zoom-in/zoom-out
(唉，IE何时灭亡)

## cursor的其他属性
&ensp;&ensp;正如标题所说，本文是对cursor的浅析，其实我只是想总结一些zoom-in/zoom-out而已，关于cursor的其他属性，可以看下面的表格

| 值        | 描述    |  
| --------   | -----  |
| url       | 需使用的自定义光标的 URL。<br />（注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标。)      |   
| default        | 默认光标（通常是一个箭头）      |   
| auto       | 默认。浏览器设置的光标。     |   
| crosshair         | 光标呈现为十字线。      |   
| pointer       | 光标呈现为指示链接的指针（一只手）      |   
| move      | 此光标指示某对象可被移动。     |   
| e-resize       | 此光标指示矩形框的边缘可被向右（东）移动。      |   
| ne-resize       | 此光标指示矩形框的边缘可被向上及向右移动（北/东）。 |   
| nw-resize       | 此光标指示矩形框的边缘可被向上及向左移动（北/西）。    |   
| n-resize        | 此光标指示矩形框的边缘可被向上（北）移动。    |   
| se-resize        | 此光标指示矩形框的边缘可被向下及向右移动（南/东）。   |   
| sw-resize       | 此光标指示矩形框的边缘可被向下及向左移动（南/西）。   |   
| s-resize      | 此光标指示矩形框的边缘可被向下移动（南）。  |   
| w-resize       | 此光标指示矩形框的边缘可被向左移动（西）。   |   
| text       | 此光标指示文本。   |   
| wait        | 此光标指示程序正忙（通常是一只表或沙漏）。   |   
| help	        | 此光标指示可用的帮助（通常是一个问号或一个气球）。  |   
| grab        | 抓手，抓，除了IE11及以，都支持，要加浏览器前缀   |   
| grabbing	        | 抓手，抓住 ，除了IE11及以，都支持，要加浏览器前缀  |

具体的效果可以点击查看 <a href="/demo/cursor-other.html" target="_blank">demo</a>
