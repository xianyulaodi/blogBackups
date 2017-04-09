---
title: cookie小结
date: 2017-03-12 12:18
categories:
tags: [博客园迁移,cookie]
---

> 　　前记：前段时间搞一个活动，开发的时间被严重压缩，忙到飞起，以致于都没怎么写文章了，内疚.
　　2月份参加了一场面试，有一些关于cookie的问题回答的不是很好，所以这篇文章我们来对cooKie做一个探讨和总结，查漏补缺。其实本文很早之前都写的差不多了，不过关于cookie跨域方面，查了比较多的资料，始终没有一个太好的结果，所以本文一直没有发布。
　　本文的很多内容都是参考网上的资料，可以说是好几篇资料的集合，毕竟是总结嘛，就是将自己觉得有用的东西集合在一起。

<!--more-->

## 什么是cookie　     
　　官方定义：Netscape官方文档中的定义为，Cookie是指在HTTP协议下，服务器或脚本可以维护客户端计算机上信息的一种方式 。通俗地说，Cookie是一种能够让网站Web服务器把少量数据储存到客户端的硬盘或内存里，或是从客户端的硬盘里读取数据的一种技术。 Cookie文件则是指在浏览某个网站时，由Web服务器的CGI脚本创建的存储在浏览器客户端计算机上的一个小文本文件，其格式为：用户名@网站地址 ［数字］.txt。
　　再通俗一点的讲，由于HTTP是一种无状态的协议，服务器单从网络连接上无从知道客户身份。怎么办呢？就给客户端们颁发一个通行证，每人一个，无论谁访问都必须携带自己通行证。这样服务器就能从通行证上确认客户身份了。
![](http://images2015.cnblogs.com/blog/776370/201702/776370-20170227220454188-452465694.jpg)

## cookie的作用　
　　HTTP协议是一种无状态、无连接的协议，不能在服务器上保持一次会话的连续状态信息。Cookie的作用是记录用户的有关信息，它最根本的用途是帮助Web站点保存有关访问者的信息。如身份识别号码ID、密码、浏览过的网页、停留的时间、用户在Web站点购物的方式或用户访问该站点的次数等，当用户再次链接Web服务器时，浏览器读取Cookie信息并传递给Web站点。　　 
　　　　
## cookie的属性 
我们先来看一张图：
![](http://images2015.cnblogs.com/blog/776370/201702/776370-20170227233400204-472517564.png)
　　在谷歌浏览器开发者模式中，我们可以看到网站的cookie，所以，相应的，我们就可以知道cookie的一些属性了，接下来介绍Cookie中的一些属性
　　如图所示，cookie具有的属性有 Name、value、Domain、path、Expires/Max-Age、Size、HTTP、Secure等等，我们接下来详细了解了解
`Name：`
该Cookie的名称，一旦创建，名称便不可更改
`value: `
该Cookie的值，如果值为Unicode字符，需要为字符编码,如果值为二进制数据，则需要使用BASE64编码
`domain：`
可以访问该Cookie的域名。如果设置为”.google.com”,则所有以”google.com”结尾的域名都可以访问该Cookie。注意第一个字符必须为“.”
**这个domain稍作解释：**
　　非顶级域名，如二级域名或者三级域名，设置的cookie的domain只能为顶级域名或者二级域名或者三级域名本身，不能设置其他二级域名的cookie，否则cookie无法生成。
　　顶级域名只能设置domain为顶级域名，不能设置为二级域名或者三级域名，否则cookie无法生成。
　　二级域名能读取设置了domain为顶级域名或者自身的cookie，不能读取其他二级域名domain的cookie。所以要想cookie在多个二级域名中共享，需要设置domain为顶级域名，这样就可以在所有二级域名里面或者到这个cookie的值了。
顶级域名只能获取到domain设置为顶级域名的cookie，其他domain设置为二级域名的无法获取。
`Path:`
　　path字段为可以访问此cookie的页面路径。 比如domain是abc.com,  path是/detail，那么只有/detail 路径下的页面可以读取此cookie。 
`Expires/Max-Age: `
　　该Cookie失效时间，单位秒。如果为正数，则Cookie在maxAge秒之后失效。
　　 如果为负数，该Cookie为临时Cookie，关闭浏览器即失效，浏览器也不会以任何形式保存Cookie.
　　 如果为0，表示删除Cookie。默认是-1
`Size:`
cookie的大小
`http： `
　　 cookie的httponly属性。若此属性为true，则只有在http请求头中会带有此cookie的信息，而不能通过document.cookie来访问此cookie。
比如截图中的__jsluid
`secure：`
  设置是否只能通过https来传递此条cookie

## cookie的特性
1、一个浏览器针对一个网站最多存20个Cookie，浏览器一般只允许存放300个Cookie
2、每个Cookie的长度不能超过4KB（稀缺）。但不同的浏览器实现的不同
3、Cookie的不可跨域名性。
　　例如：Cookie在客户端是由浏览器来管理的，浏览器能够保证Google只会操作Google的Cookie而不会操作Baidu的Cookie，从而保证用户的隐私安全。

## cookie的分类 
cookie有两种类型：
* 临时Cookie（会话Cookie）
* 永久Cookie
　　不设置过期时间，则表示这个cookie生命周期为浏览器会话期间，只要关闭浏览器窗口，cookie就消失了。这种生命期为浏览会话期的cookie被称为会话cookie。会话cookie一般不保存在硬盘上而是保存在内存里。可以类比于本地存储的sessionstore
　　设置了过期时间，浏览器就会把cookie保存到硬盘上，关闭后再次打开浏览器，这些cookie依然有效直到超过设定的过期时间。
　　存储在硬盘上的cookie可以在不同的浏览器进程间共享，比如两个IE窗口。而对于保存在内存的cookie，不同的浏览器有不同的处理方式。可以类比于本地存储的localstore

## cookie的操作
**1、 cookie的发送：**
　　服务器端像客户端发送Cookie是通过HTTP响应报文实现的，在Set-Cookie中设置需要像客户端发送的cookie，cookie格式如下：
```
·Set-Cookie: "name=value;domain=.domain.com;path=/;expires=Sat, 11 Jun 2016 11:29:42 GMT;HttpOnly;secure"
```
其中`name=value`是必选项，其它都是可选项。
![](https://segmentfault.com/img/bVthn4?_=6476991)
　　客户端的话用js即可操作，由于现在客户端设置大部分用H5的本地存储localstore和sessionstore多一点，所以客户端的这里不做介绍

**2、cookie的读取** 
　这里介绍的js来读取cookie，可以直接使用下面的方法，其实就是用document.cookie：
```javascript
function getCookie(name){
    var cookieName=encodeURIComponent(name)+"=",
    cookieStart=document.cookie.indexOf(cookieName),
    cookieValue=null;
    if(cookieStart>-1){
        var cookieEnd=document.cookie.indexOf(";",cookieStart);
        if(cookieEnd==-1){
            cookieEnd=document.cookie.Length;
        }
        cookieValue=decodeURIComponent(document.cookie.substring(cookieStart+document.cookie.length,cookieEnd));
    }
    return cookieValue;
}
```
** 3、cookie的修改与删除 **
　　Cookie并不提供修改、删除操作。如果要修改某个Cookie，只需要新建一个同名的Cookie，添加到response中覆盖原来的Cookie。
　　如果要删除某个Cookie，只需要新建一个同名的Cookie，并将maxAge设置为0，并添加到response中覆盖原来的Cookie。注意是0而不是负数。

## Cookie的实现原理
 　　Cookie实际上是一小段的文本信息。客户端请求服务器，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。客户端浏览器会把Cookie保存起来。当浏览器再请求该网站时，浏览器把请求的网址连同该Cookie一同提交给服务器。服务器检查该Cookie，以此来辨认用户状态。服务器还可以根据需要修改Cookie的内容。如下图所示：
![](http://hi.csdn.net/attachment/201111/9/0_13208318702UfF.gif)

这个跟其实跟浏览器你器缓存有点类似，具体的过程我们可以分解分解：
（1）客户端在浏览器的地址栏中键入Web服务器的URL，浏览器发送读取网页的请求。 
（2）服务器接收到请求后，产生一个Set-Cookie报头，放在HTTP报文中一起回传客户端，发起一次会话。 
（3）客户端收到应答后，若要继续该次会话，则将Set-Cook-ie中的内容取出，形成一个Cookie.txt文件储存在客户端计算机里。
（4）当客户端再次向服务器发出请求时，浏览器先在电脑里寻找对应该网站的Cookie.txt文件。如果找到，则根据此Cookie.txt产生Cookie报头，放在HTTP请求报文中发给服务器。
（5）服务器接收到包含Cookie报头的请求，检索其Cookie中与用户有关的信息，生成一个客户端所请示的页面应答传递给客户端。 浏览器的每一次网页请求，都可以传递已存在的Cookie文件，例如，浏览器的打开或刷新网页操作。

## Cookie的安全问题 
　　通常cookie信息都是使用http连接传递数据，这种传递方式很容易被查看，而且js里面直接有一个document.cookie方法，可以直接获取到用户的cooie,所以cookie存储的信息容易被窃取。假如cookie中所传递的内容比较重要，那么就要求使用加密的数据传输。

**如何来防范cookie的安全呢？有以下几种方法：**
* HttpOnly属性
　　 如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息，这样能有效的防止XSS攻击。
* secure属性
　　当设置为true时，表示创建的 Cookie 会被以安全的形式向服务器传输，也就是只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证，如果是 HTTP 连接则不会传递该信息，所以不会被盗取到Cookie 的具体内容。
　　我们再来看看一道经典的面试题：
`登录时候用cookie的话，安全性问题怎么解决？`
这个问题，网上找了比较久的答案，比较满意的有两种答案（答案是网上找的）
> 
第1种是：
把用户对象（包含了用户ID、用户名、是否登录..）序列化成字符串再加密存入Cookie。
密钥是：客户端IP+浏览器Agent+用户标识+固定的私有密钥
当cookie被窃取后，只要任一信息不匹配，就无法解密cookie，进而也就不能登录了。
这样做的缺点是IP不能变动、频繁加密解密会加重CPU负担

> 第2种是：
将用户的认证信息保存在一个cookie中，具体如下： 
1. cookie名：uid。推荐进行加密，比如MD5(‘站点名称’)等。 
2. cookie值：登录名|有效时间Expires|hash值。hash值可以由”登录名+有效时间Expires+用户密码（加密后的）的前几位 +salt” (salt是保证在服务器端站点配置文件中的随机数)
这样子设计有以下几个优点： 
1.即使数据库被盗了，盗用者还是无法登录到系统，因为组成cookie值的salt是保证在服务器站点配置文件中而非数据 库。 
2.如果账户被盗了，用户修改密码，可以使盗用者的cookie值无效。 
3.如果服务器端的数据库被盗了，通过修改salt值可以使所有用户的cookie值无效，迫使用户重新登录系统。 
4.有效时间Expires可以设置为当前时间+过去时间（比如2天），这样可以保证每次登录的cookie值都不一样，防止盗用者 窥探到自己的cookie值后作为后门，长期登录。

## cookie跨地址，跨域问题以及解决方案
　　cookie是不能跨域访问的，那么，假如需要跨域来进行cookie的访问和传递，该怎么办呢？查找了比较多的资料，比较少这方面的资料，
　　在cookie跨域这个问题上，前端能做的不多，很多都是需要和后端一起配合来完成。
　　总结了下面的几种方法，具体的实现过程这里没有写，可以点击我提供的链接自己看看。
　　前2种具体的实现方法可以点击看这里:[点我](http://www.cnblogs.com/hujunzheng/p/5744755.html)
**1、nginx方向代理：**
　　反向代理（Reverse Proxy）方式是指以代理服务器来接受Internet上的连接请求，然后将请求转发给内部网络上的服务器；并将从服务器上得到的结果返回给Internet上请求连接的客户端，此时代理服务器对外就表现为一个服务器。
　　反向代理服务器对于客户端而言它就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理 的命名空间(name-space)中的内容发送普通请求，接着反向代理将判断向何处(原始服务器)转交请求，并将获得的内容返回给客户端，就像这些内容 原本就是它自己的一样。
**2、jsonp方法：**
　　这个方法和我们平时处理js跨域的jsonp方法一样。具体实现方法可以看看淘宝的解决方法，[点我](http://developer.51cto.com/art/201104/255729.htm)　
**3、nodejs的superagent**
**4、iframe方法：**
比如有个`www.a.com/index.html`的页面，往`www.b.com/index.html`的页面传递cookie
`www.a.com/index.html`这样写：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>我是a页面</title>
</head>
<body>
    
</body>
<script type="text/javascript">
document.cookie = "name=" + "value;" + "expires=" + "datatime;" + "domain=" + "" + "path=" + "/path" + "; secure";
//name Cookie名字
//value Cookie值
//expires 有效期截至(单位毫秒)
//path 子目录
//domain 有效域
//secure 是否安全
window.location = "http://www.b.com/index.html?" + document.cookie;  //跳转到b页面
</script>
</html>
```
www.b.com/index.html 这样写：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>我是b页面</title>
</head>
<body>
   <iframe src='http://www.a.com/index.html' width='100' height='100' style="display:none"></iframe>
</body>
<script type="text/javascript">
var url = window.location.toString();//获取地址
var get = url.substring(url.indexOf("abc"));//获取变量和变量值
var idx = get.indexOf("=");//获取变量名长度
if (idx != -1) {
   var name = get.substring(0, idx);//获取变量名
   var val = get.substring(idx + 1);//获取变量值
   setCookie(name, val, 1);//创建Cookie
}
</script>
</html>
``` 



备注：本文主要是查找了网上比较多的资料来总结cookie的一些知识，文笔有限，有误之处，欢迎指出

 



