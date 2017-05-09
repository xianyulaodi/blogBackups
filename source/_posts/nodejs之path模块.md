---
title: nodejs之path模块
date: 2017-05-07 18:50
categories:
tags:
     - path
     - node
---
&ensp;&ensp;&ensp;&ensp;path 模块是 node 用于整理、转换、合并路径的神器，只要是路径问题，都可以交给它处理。但它仅仅是处理路径字符串，而不会去坚持或处理文件。

<!-- more -->

### 格式化路径 path.normalize(p);
作用：将不符合规范的路径格式化，简化开发人员中处理各种复杂的路径判断
```javascript
var path = require('path');
path.normalize('/foo/bar//baz/asdf/quux/..');
 //==>  '/foo/bar/baz/asdf'
```

### 路径合并 path.join([path1], [path2], […]);
作用：将所有名称用path.seq串联起来，然后用normailze格式化，规范化的路径字符串。
```javascript
path.join('///foo', 'bar', '//baz/asdf', 'quux', '..');
//==>  '/foo/bar/baz/asdf'
```

### 绝对路径 path.resolve([from …], to);
作用：相当于不断的调用系统的cd命令
```javascript
path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subfile')
//相当于：
// cd foo/bar
// cd /tmp/file/
// cd ..
// cd a/../subfile
// pwd
```

### 相对路径 path.relative(from, to);
作用： 返回某个路径下相对于另一个路径的相对位置串.
```javascript
/**
 * from 当前路径，并且方法返回值是基于from指定到to的相对路径
 * to   到哪路径，
 */
var from = 'c:\\from\\a\\',
    to = 'c:/test/b';
var _path = path.relative(from, to);
console.log(_path); //..\..\test\b; 表示从from到to的相对路径
```

## 文件路径 path.dirname
作用： 根据一个文件或目录得到它所在的目录路径，这个很常用。
```javascript
var myPath = path.dirname(__dirname + '/test/util you.mp3');
console.log(myPath);   // E:/doc/test/util you.mp3
```

## 文件名称 path.basename(p, [ext]);
作用： 返回最后一个路径分割后面的文件名，不论是文件还是目录，第二个参数可以忽略文件后缀。
```javascript
var str = path.basename('path/upload/file/123.jpg');
console.log(str); // 123.jpg
var str = path.basename('path/upload/file/123.jpg', '.jpg');
console.log(str); // 123
```

## 文件扩展名 path.extname(path);
作用：返回最后一个 . 之后的字符串，没有则返回空。
```javascript
var str = path.extname('path/file/abc.txt');
console.log(str); // '.txt'

var str = path.extname('path/file/abc.');
console.log(str); // '.'

var str = path.extname('path/upload/file/');
console.log(str); // ''
```

## 解析路径 path.parse
作用：把一个路径解析为一个 `{root:'', dir:'', base:'', ext:'', name:''}` 这样的对象。
```javascript
path.parse('/home/user/dir/file.txt')
// returns
{
    root : "/",
    dir : "/home/user/dir",
    base : "file.txt",
    ext : ".txt",
    name : "file"
}
// windows
path.parse('C:\\path\\dir\\index.html')
// returns
{
    root : "C:\\",
    dir : "C:\\path\\dir",
    base : "index.html",
    ext : ".html",
    name : "index"
}
```

## 生成路径 path.format
作用：跟 `path.parse` 相反，这个则是根据 `{root:'', dir:'', base:'', ext:'', name:''}` 这样的对象来生成字符串
```javascript
path.format({
    root : "/",
    dir : "/home/user/dir",
    base : "file.txt",
    ext : ".txt",
    name : "file"
})
// returns
'/home/user/dir/file.txt'
```
