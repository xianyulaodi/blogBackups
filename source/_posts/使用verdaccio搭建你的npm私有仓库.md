---
title: 使用verdaccio搭建你的npm私有仓库
date: 2018-03-29 15:41
categories:
tags: [npm]
toc: true
---

## 背景： 
 
 有些时候，我们的一些npm包不想被外部的人访问，所以此时就需要一个npm私有仓库。本文主要记录一下使用verdaccio搭建npm私有仓库的一个过程

[verdaccio的官网](http://www.verdaccio.org)

<!-- more-->


## 1. 搭建的步骤如下

* 1.安装verdaccio： `npm install -g verdaccio`

* 2.启动verdaccio: `verdaccio`

    显示界面如下：
    ```javascript
    warn --- config file  - /home/.config/verdaccio/config.yaml 
    warn --- http address - http://localhost:4873/ - verdaccio/3.0.0
    ```
    配置文件位置:`warn --- config file  - /home/.config/verdaccio/config.yaml` 

    访问地址:`warn --- http address - http://localhost:4873/ - verdaccio/3.0.0` 

* 3.注册一个用户
`npm adduser --registry http://localhost:4873`
输入用户名，密码，邮箱完成注册。注册完，输入`npm whoam i`,如果显示的是你的用户名，则说明登录成功了


* 4.发布一个npm包
执行`npm publish --registry http://localhost:4873`,即可发布成功。


## 2. 私有npm包的使用

1. 设置镜像为你的npm包地址，如：`npm set registry http://localhost:4873`

2. 正常安全其他npm包一样安装，如果在你私有npm仓库有你需要的包，则会去你仓库那里下载，否则到npm公共镜像查找

  注意点： 设置镜像为你的npm私有包后，每次安装npm包，都会在你服务器上备份一份，下次你再次安装的时候，会读取服务器缓存的部分，安装速度会快很多。但也带来了缺点，就是这些包都在你服务器上存储了一份，占内存。如果其他公共的npm包不想存储在服务器，可以将镜像切换回默认的 `npm config set registry http://registry.npmjs.org`
  

## 3. 一些配置

配置文件为 config.yaml，也就是第一个warn输出的那个路径


设置首页logo，title
```javascript
web:
  enable: true
  title: Verdaccio
  logo: logo.png
```

设置端口：
`listen: xxx`

部署到服务器上，listen要这样写：
`listen: 0.0.0.0:4873`

...

如果需要使用pm2来守护进程，执行 
```javascript
pm2 start `which verdaccio`
```

更多配置可以看  [这里](http://www.verdaccio.org/docs/en/configuration.html)