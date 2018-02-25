---
title: 如何写一个npm包
date: 2018-02-25 10:15
categories:
tags: [npm]
toc: true
---

> 一直想学习一下如何写一个npm包，奈何一直拖拖拖，今天开始行动，此文用于记录一下写npm包的过程。当然，网上也有很多类似的课程可找

<!-- more -->

我们今天要写的一个npm包的主要作用是用于判断用户是否登录了。暂且给它起名为：`is-login`.

下面开始我们的npm包的制作过程

## 1. 初始化一个项目

```bash
mkdir is-login
cd is-login
npm init
```
一路回车即可

## 2. 添加我们的代码

新建index.js文件。我们的这个npm包是判断用户是否登录了，所以我们的代码也比较简单

代码如下:
```javascript
module.exports = function(options) {
    const options = options || {};
    const sessionName = options["sessionName"] || "user";
    const redirect = options["redirect"] || "/login";
    return function(req,res,next) {
        if(!req.session[sessionName]) {
            return res.redirect(redirect);
        }
        next();
    }
}
```
代码比较简单，默认session的名字为user,如果没有登录，则默认转到`/login`这个路由,也可以自定义session的名字以及自定义未登录时跳转的路由

## 3. 添加LICENCE文件，说明对应的开源协议

可到[SPDX License List](https://spdx.org/licenses/) 或者 [Open Source Initiative](https://opensource.org/licenses/alphabetical) 下载相应协议的模板。这里我们选择的是MIT开源协议。

我们新建一个名为 `LICENSE` 的文件，并在里面粘贴进如下内容
```javascript
The MIT License (MIT)

Copyright (c) <year> <copyright holders>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
```
将`<year>` 和 `<copyright holders>`修改为 对应的年份 和 版权拥有者名字

## 4. 添加 README.md 文件 和 .gitignore 
README.md 文件主要用于 该项目的一些说明，使用方法等
在一些npm包中的README.md中，会看到类似于下图好看的版本信息
![icon](/img/npm-icon.png)
这些版本信息图标可以在 [https://shields.io](https://shields.io) 制作
登录该网站，拉到最下面
![icon](/img/p25.jpg)输入对应 文字，版本号即可 ![icon](/img/p26.jpg)


.gitignore主要用于忽略不需要提交的文件更改。可以去[gitignore](https://github.com/github/gitignore)下载对应的模板，然后自己修改修改

## 5. 发布npm包

 


参考文件： 
[如何自己写一个公用的NPM包](http://blog.csdn.net/A41202197514/article/details/76736776) 
[从零开始教你写一个NPM包](https://segmentfault.com/a/1190000011095467) 
[手把手教你用npm发布一个包](https://www.jianshu.com/p/36d3e0e00157) 

