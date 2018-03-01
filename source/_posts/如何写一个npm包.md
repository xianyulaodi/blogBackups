---
title: 如何写一个npm包
date: 2018-02-25 10:15
categories:
tags: [npm]
toc: true
---

> 一直想学习一下如何写一个npm包，奈何一直拖拖拖，今天开始行动，此文用于记录一下写npm包的过程。当然，网上也有很多类似的课程可找

<!-- more -->

我们今天要写的一个npm包的主要作用是用于判断用户是否登录了,由于我们是写来玩的，所以暂且给它起名为：`is-login-test`

下面开始我们的npm包的制作过程

## 1. 初始化一个项目

```bash
mkdir is-login-test
cd is-login-test
npm init
```
一路回车即可

## 2. 添加我们的代码

新建`index.js`文件。我们的这个npm包是判断用户是否登录了，所以我们的代码也比较简单

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

## 3. 完善你的package.json文件

这一步也可以在初始化项目的时候完成，看个人喜欢

`name`：包的名字，确保该名字是独一无二的
`version`：包的版本，默认是1.0.0
`description`：包的描述
`main`：入口文件，默认是index.js
`test command`：测试命令
`repository`：git仓库地址,一般为"type":"git","url":"git的url"
`keyword`：这个挺重要，尽量用比较贴切的关键字作为这个包的索引，这样才能有更多的人搜索到你的包
`author`：写你的账号或者你的github账号吧
`license`：开源协议用了哪个

## 4. 添加LICENCE文件，说明对应的开源协议

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

## 5. 添加 README.md 文件 和 .gitignore 
README.md 文件主要用于 该项目的一些说明，使用方法等
在一些npm包中的README.md中，会看到类似于下图好看的版本信息
![icon](/img/npm-icon.png)
这些版本信息图标可以在 [https://shields.io](https://shields.io) 制作
登录该网站，拉到最下面
![icon](/img/p25.jpg)输入对应 文字，版本号即可 ![icon](/img/p26.jpg)


.gitignore主要用于忽略不需要提交的文件更改。可以去[gitignore](https://github.com/github/gitignore)下载对应的模板，然后自己修改修改

## 6. 发布npm包

1. 首先到 [https://www.npmjs.com](https://www.npmjs.com) 上注册一个账号

2. 在你需要发布的项目文件中，终端输入 `npm adduser`,然后按照提示，输入对应的用户名，密码，邮箱即可。
这里需要注意，你的镜像必须是npm，而不是cnpm，可以使用nrm来管理你的镜像。登录之后，可以终端输入`npm whoami`,如果出现你的名字，那就说明登录成功了。

3. 最后输入`npm publish --access=public`,即可发布成功。如果有报错，那可能你的包名被人占用了，换个名字吧

4. 在你的电脑上，直接`npm install 你的包名`，来看看你的包是否发布成功了。当然，也可以在[https://www.npmjs.com](https://www.npmjs.com)这里查找你发布的包

5. 如果你要更新你的npm包，记得修改package.json的版本号，而且一般比上一个版本要高一点，否则会报错


