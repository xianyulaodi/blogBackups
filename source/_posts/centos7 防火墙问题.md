---
title: centos7防火墙问题
date: 2018-03-27 17:19
categories:
tags: [服务器]
toc: true
---

## 背景： 
 买了个阿里云服务器，操作系统为centos7的，发觉外网访问的时候，只能访问80端口，其他端口都不能访问。此时，你去google查找解决方法，查来查去，大家都会告诉你，是防火墙的问题，你只要在防火墙开放你需要访问的端口，问题就解决了，于是你试啊试，WTF,没有一个方法是可行的。 真正的解决办法如下：

<!-- more-->

## 1. 正确解决方法

其实是因为阿里云的宿主机环境中，你要访问的端口没有被添加到安全组中。

步骤如下： 

1. 登录云服务器管理控制台

2. 选择安全组配置
![](/img/aliyun1.png)

3. 点进去，【配置规则】=> 【添加安全组规则】，设置规则如下：

网络类型：选择内网
规则方向：入方向
授权策略：允许
协议类型：自定义tcp
端口范围：比如你想开放 3000端口，则输入 3000/3000
授权类型：地址段访问 
授权对象：0.0.0.0/0
优先级：1-100，数值越小，优先级越高。


## 2. centos7防火墙

centos7的防火墙是 firewall,而不是iptables，网上有很多乱七八糟的教程叫个禁掉 firewall,改用iptables,不要去改，否则会越改越烂。在centos7中，防火墙咋就用 firewall。

在第一步完成之后，在服务器那里配置一下你需要开放的端口，比如我要开放8080端口，执行如下命令

`firewall-cmd --zone=dmz --add-port=8080/tcp`



centos7防火墙的一些使用方法如下：

* 检查防火墙状态:
`firewall-cmd --state`
`firewall-cmd --list-all`

* 停用:
`systemctl stop firewalld`

* 开启:
`systemctl start firewalld`

* 更新防火墙:
`firewall-cmd --reload`

* 在开机时启用一个服务:
`systemctl enable firewalld.service`

* 在开机时禁用一个服务:
`systemctl disable firewalld.service`



不得不说，真的好讨厌那些抄袭党，同一篇文章，被抄来抄去，以至于搜索的时候，很多都是同一篇文章

