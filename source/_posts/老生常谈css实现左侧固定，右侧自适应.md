---
title: 老生常谈，css实现左侧固定右侧自适应
date: 2017-05-25 10:20
categories:
tags:
     - css
---

&ensp;&ensp;这是一个在项目中经常使用的一个功能，而且实现方式也比较多，所以我们今天来总结以下几种实现方法

<!--more-->
我们的所有html布局都一样，都为下面的布局

```html
<div class="parent">
  <div class="left">左边内容</div>
  <div class="right">右边内容</div>
</div>
```
<br>
## 方法1：position + margin

方法：父集相对定位，左侧绝对定位，右侧margin-left
```css
.parent {
  width: 300px;
  height: 100px;
  margin: 100px auto;
  position: relative;
}
.left {
   width: 100px;
   height: 100px;
   position: absolute;
   left: 0;
   background: red;
}
.right {
  height: 100px;
  margin-left: 100px;
  background: blue;
}
```

<br>
## 方法2 ： 左测浮动，右侧overflow

```css
.parent {
  width: 300px;
  height: 100px;
  margin: 100px auto;
}
.left {
   width: 100px;
   height: 100px;
   float: left;
   background: red;
}
.right {
  height: 100px;
  overflow: hidden;
  background: blue;
}
```
<br>
## 方法3：左侧浮动，右侧margin-left
跟方法2类似，只是将右侧overflow改为margin-left
```css
.parent {
  width: 300px;
  height: 100px;
  margin: 100px auto;
  position: relative;
}
.left {
   width: 100px;
   height: 100px;
   float: left;
   background: red;
}
.right {
  height: 100px;
  margin-left: 100px;
  background: blue;
}
```
<br>
## 方法4; css3 flex
方法： 利用css3的flex属性，右边设置为占比1，填充满剩余空间

```css
.parent {
  width: 300px;
  height: 100px;
  margin: 100px auto;
  display: flex;
}
.left {
   width: 100px;
   height: 100px;
   background: red;
}
.right {
  height: 100px;
  flex: 1;
  background: blue;
}
```
<br>
## 方法五：使用CSS3属性calc()进行计算。注意：calc()里的运算符两边必须有空格
```css
.parent {
  width: 300px;
  height: 100px;
  margin: 100px auto;
  position:relative;
}
.left {
   width: 100px;
   height: 100px;
   float:left;
   background: red;
}
.right {
  height: 100px;
  float:left;
  width:calc(100% - 100px);
  background: blue;
}
```
<br>
## 方法六  左右两边 absolute
```css
.parent {
  width: 300px;
  height: 100px;
  margin: 100px auto;
  position:relative;
}
.left {
   width: 100px;
   height: 100px;
   position: absolute;
   left: 0;
   background: red;
}
.right {
  height: 100px;
  position: absolute;
  left: 100px;
  right: 0;
  background: blue;
}
```
