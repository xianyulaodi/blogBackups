---
title: hexo+github搭建自己的博客
date: 2017-04-8 15:08:32
categories:
tags:
    - hexo
    - github
toc: true
---

之前很早就想用hexo弄一个自己独立的博客了，在博客园也写了很多的博客,不过不喜欢博客园的风格。不过今天，终于折腾成功了，用hexo搭建了一个在github写的博客，开心，后面会将自己以前的博客慢慢迁移过来。

<!--more-->
## 前期准备工作
1. 安装hexo `npm install -g hexo`
2. 创建一个文件夹，如：myBog，cd到myBog里执行`hexo init`命令
3. 执行`hexo generate` （hexo g  也可以）  
4. 执行`hexo server `   

## hexo写博客的步骤

### 新建一篇博客:

* 方法1：	
```
hexo new "文章标题"

```
* 方法2：在本地博客文件夹`source->_post`文件夹下看到我们新建的markdown文件

两者的效果是一样的


### 进行本地发布
1. 执行命令 `hexo server`
2. 浏览器打开：http://localhost:4000/

### 部署到线上，执行三个命令
1. `hexo clean`
2. `hexo generate` 也可以 `hexo g`
3. `hexo deploy`
或者直接执行 hexo c && hexo g && hexo d

## 一些常用命令
`hexo new "postName"` #新建文章
`hexo new page "pageName"` #新建页面
`hexo generate` #生成静态页面至public目录
`hexo server` #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
`hexo deploy` #将.deploy目录部署到GitHub
`hexo help`  #查看帮助
`hexo version`  #查看Hexo的版本

## 小tips
1. 问：如何让文章想只显示一部分和一个 阅读全文 的按钮？ 
答：在文章中加一个 `<!--more-->` ， `<!--more-->` 后面的内容就不会显示出来了。

2. 问：本地部署成功了，也能预览效果，但使用 username.github.io 访问，出现 404 . 
答：首先确认 hexo d 命令执行是否报错，如果没有报错，再查看一下你的 github 的 username.github.io 仓库，你的博客是否已经成功提交了，你的 github 邮箱也要通过验证才行。

## 如何更换主题
分为以下个步骤：
1. 选择主题:哪里选呢，可以在这里[官方主题](https://hexo.io/themes/)
2. 安装主题： 将主题下载或者clone到你的站点目录的 themes 目录中，比如我要安装yilia主题，那么将改文件夹复制到themes中，即为 `themes/yilia`
3. 打开 站点配置文件_config.yml，找到 theme 字段，并将其值更改为 `yilia`(你要安装的主题的文件夹名字) 。
4. 验证主题是否启用: 运行 `hexo s --debug` ，并访问 `http://localhost:4000` ，确保站点正确运行。
5. 部署和发布到文章的步骤一样

## 头像设置
在主题文件夹下的`_config.yml`中：`avatar: https://avatars1.githubusercontent.com/u/32269?v=3&s=460`.比如我的是`themes/yilia/_config.yml`
由于我用的是yilia主题，或者直接修改`layout/_partial/left-col.ejs`的第六行和第八行为：
```html
 <img src="<%=theme.avatar%>" class="js-avatar show">
 <img src="<%=theme.avatar%>" class="js-avatar show" style="width: 100%;height: 100%;opacity: 1;">
```
## 添加阅读量统计
这里使用的是不蒜子统计  [点击前往](http://busuanzi.ibruce.info/)

## 添加评论模块

评论模块使用的是 Valine -- 一款极简的评论系统
[点击前往](https://ioliu.cn/2017/add-valine-comments-to-your-blog/)
后台管理地址: https://leancloud.cn/dashboard/data.html?appid=lfWm5WyOhILUbB9yW5jfsSPM-gzGzoHsz#


## 文章目录导航
参考的是这边文章  [点击前往](http://www.54tianzhisheng.cn/2017/06/13/Hexo-yilia-toc/)
