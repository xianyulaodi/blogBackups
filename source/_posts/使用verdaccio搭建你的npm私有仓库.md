---
title: 使用verdaccio搭建你的npm私有仓库
date: 2018-03-29 15:41
categories:
tags: [npm]
toc: true
---

## 背景： 
 有时候dxx

<!-- more-->

[verdaccio的官网](http://www.verdaccio.org)

1. 搭建的步骤如下

全局安装： `npm install --g verdaccio`

安装完运行命令： `verdaccio`

会看到显示
```javascript
warn --- config file  - /home/.config/verdaccio/config.yaml 
warn --- http address - http://localhost:4873/ - verdaccio/3.0.0
```
说明运行成功了，其中
第一条warn是 verdaccio的配置文件存在的位置，在你本机上，则显示存在你电脑上的路径，里面可以设置很多东西，如：包存储的位置，最大用户数，是否允许所有用户使用等等

第二条warn是 访问的地址，在浏览器打开`http://localhost:4873/`,会看到一个类似于http://wwww.npmjs.com 的界面

2. 发布包
新开一个命令窗口，输入`npm adduser --registry http://localhost:4873`,新增用户名，输入用户名，密码，邮箱
输入完，输入`npm whoam i`,如果显示的是你的用户名，则说明登录成功了


在你需要发布的包文件夹下，执行`npm publish --registry http://localhost:4873`,即可发布成功。

这时刷新一下 `http://localhost:4873/` 这个页面，即可看到你发布的包



3. 一些设置

配置文件都是在 config.yaml 上修改，也就是第一个warn输出的那个路径

设置首页logo，title
```javascript
web:
  enable: true
  title: Verdaccio
  logo: logo.png
```

设置端口：
`listen: xxx`

如果要部署到服务器上，listen需要这样写
`listen: 0.0.0.0:4873`,端口可以改为你开放的端口

...

具体的配置情况可以看这里 [http://www.verdaccio.org/docs/en/configuration.html](http://www.verdaccio.org/docs/en/configuration.html)

4. 如何使用你的私有npm包

在你的电脑上配置： `npm set registry http://localhost:4873` 设置镜像为本地镜像,npm 进行安装操作时会先对本地进行查找，没有找到的会去npm公共镜像查找。


如果需要修改为原来的镜像： npm config set registry http://registry.npmjs.org 

然后跟正常安装其他安装包一样使用即可，它会先查找你电脑上有没有这个包，如果有，则用你电脑上的，否则，会去 npmjs.com上去查找


ngrok 学习

先找到verdaccio在哪里，执行`which verdaccio`

pm2 start /root/.nvm/versions/node/v8.9.4/bin/verdaccio

https://medium.com/strapi/testing-your-npm-package-before-releasing-it-using-verdaccio-ngrok-28e2832c850a

http://www.verdaccio.org/docs/en/iss-server.html

http://music.163.com/#/artist?id=13193


npm adduser --registry http://localhost:4873