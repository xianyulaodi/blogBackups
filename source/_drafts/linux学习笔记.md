---
title: linux学习笔记
date: 2018-01-16 20:05
categories:
tags: [linux]
toc: true
---

## 常用目录作用

Linux系统中常见目录的作用：
 
** / **  	
Linux系统的根目录。

** /bin/ **	
存放系统命令的目录，普通用户和超级用户都可以执行里面的命令。该目录中的命令在单用户模式下也可以执行。

** /sbin/ **	
存放与系统环境设置相关的命令，只有超级用户才可以执行里面的命令，但是有些命令允许普通用户查看。

** /usr/bin/ **	
存放系统命令的目录，普通用户和超级用户都可以执行里面的命令。这些命令和系统启动无关，在单用户模式下不能执行。

**	/usr/sbin/	**	
存放根文件系统不必要的系统管理命令，如多数服务程序。只有超级用户可以使用。

**	/boot/	**	
系统启动目录，存放与系统启动相关的文件，如内核文件、启动引导程序（grub）文件等。

**	/dev/	**	
存放硬件设备文件的目录。Linux中所有的内容都会以文件形式保存，包括硬件。这个目录就是用来保存所有的硬件设备文件的。

**	/etc/	**	
存放配置文件的目录。系统内所有采用默认安装方式（rpm包安装）的服务的配置文件都保存在这个目录中，如用户名和密码、服务的启动脚本、常用服务的配置文件等。

**	/root/ **		
超级用户root（也叫超级管理员）的家目录。家目录是用户的默认登录位置，当切换到其他目录后，想迅速返回家目录，可直接使用 cd 命令。

**	/home/ **		
普通用户的家目录。创建普通用户时，每个用户要有一个默认的登录位置，这个位置就是该用户的家目录。普通用户的家目录就是在 /home/ 下自动创建一个和用户名相同的子目录。如用户user01的家目录就是 /home/user01/ 。

**	/lib/	**	
存放系统函数库的目录。

**	/lost+found/ **	
当系统意外崩溃或机器意外关机时，产生的一些文件碎片会保存在这个目录。系统再次启动时，fsck工具会检测这里，并修复已经损坏的文件系统。这个目录只在每个分区中出现，如 /lost+found 就是根分区的备份恢复目录， /boot/lost+found 就是 /boot 分区的备份恢复目录。

**	/media/	**	
挂载目录。系统默认推荐的用于挂载媒体设备的，如光盘和软盘。

**	/mnt/	**	
挂载目录。早期的Linux中只有这一个挂载目录，现在这个目录一般用来挂载额外的设备，如U盘、移动硬盘等。

**	/misc/	**	
挂载目录。系统推荐用来挂载NFS服务的共享目录。系统给我们准备了三个默认的挂载目录 /media、/mnt、/misc，但具体在哪个目录中挂载什么设备，均可以由管理员自己决定。如：可以在 /mnt 目录中创建两个空目录 /mnt/cdrom 和 /mnt/usb，分别用来挂载光盘和U盘。当然，也可以自己创建一级空目录 /usb 来挂载U盘。

**	/opt/	**	
第三方软件的安装目录。如：手工安装的源码包软件就可以安装到这个目录当中。不过，现在更多的用户和厂家倾向于把软件安装到 /usr/local/ 目录当中。

**	/proc/	**	
虚拟文件系统。该目录中的数据并不是保存在硬盘中，而是保存在内存当中。主要用来保存进程、外部设备等信息。如 /proc/cpuinfo 保存的是CPU信,/proc/devices 保存的是设备驱动信息列表。

** /sys/	**
虚拟文件系统。和 /proc 目录相似，里面的数据也是保存在内存中的，它主要用来保存内核的相关信息。

** /srv/ **	
存放系统服务相关数据的目录。

** /tmp/	**
临时目录。系统存放临时文件的目录，所有用户对于该目录都有读和写的权限。不要在该目录保存重要数据，最好每次开机都把该目录清空。

** /usr/ **
存放系统软件资源的目录。它是“Unix Software Resource”的缩写，而不是user的缩写。

** /var/	**
存放动态数据的目录。主要保存日志、邮件、缓存等。


** 注意: **  /usr下也有/bin和/sbin目录，同/下的两个目录一起保存系统命令，/sbin下的命令只有超级用户才能执行,可以在家目录'/home'或根目录/，以及/tmp目录下随便放内容，其他都别动，但也不推荐在根目录下操作，只放必要数据


## 常用快捷键

| 快捷键 | 含义 |  
| - | :-: | -: | 
| ctrl+c | 强制终止当前命令 | 
| ctrl+l | 清屏 | 
| ctrl+a | 光标移动到命令行首 | 
| ctrl+e | 光标移动到命令行尾 | 
| ctrl+z | 把命令放入后台 | 
| ctrl+r | 在历史命令中搜索 | 



<!--more-->

## 关机和重启命令

** 关机命令 ** 

shutdown 【选项】 时间
- c 取消前一个关机命令
- h 关机
- r 重启
  - 使用date命令看系统日期
  - shutdown -r 05:30表示在凌晨5点30分重启，此时进入倒计时状态，无法再操作，通过ctrl+c取消。
  - 在最后加上&，使命令在后台执行，不占用操作界面，两次回车后继续自己的操作。此时如果不想在后台执行这条命令，可以用shutdown -c来取消。
  - shutdown -r now表示现在就重启。
  - 在远程登录服务器时要避免使用这个命令！

** 其他关机命令 **
halt、poweroff、init 0 。但这三者都不太安全

** 其他重启命令 **
reboot 比较安全

** 退出登录命令：logout **


## 链接命令

命令： ln -s 【原文件】【目标目录】
作用：功能是生成链接文件，-s 创建软链接。可以理解为类似于windows中的快捷方式

1. 硬链接的特征
- 硬链接拥有相同的i节点和存储block块，可看做是同一个文件
- 可通过i节点识别
- 不能跨分区
- 不能针对目录使用
```bash
ln desktop/Learning_Python/hello_world.py documents/hello_world.hard

ls -l desktop/Learning_Python/hello_world.py
-rw-r--r--  2 Jeff  staff  3379  3 14 10:30 desktop/Learning_Python/hello_world.py

ls -l documents/hello_world.hard
-rw-r--r--  2 Jeff  staff  3379  3 14 10:30 documents/hello_world.hard

ls -i desktop/Learning_Python/hello_world.py documents/hello_world.hard
2231081 desktop/Learning_Python/hello_world.py
2231081 documents/hello_world.hard
```
2. 软链接的特征

- 类似于win下的快捷方式
- 软链接拥有自己的i节点和block块，但数据块中只保存原文件的文件名和i节点号，并没有实际的文件数据
- Irwxrwxrwx I软链接 软链接文件的权限都是rwxrwxrwx
- 修改任意文件，另一个都改变
- 删除原文件，软链接不能使用
- 做软链接，一定要写绝对路径

## 目录处理命令

* 创建目录命令
命令：mkdir -p 【目录名】
作用：-p用来递归创建，当上一级目录不存在时也会创建
例如： mkdir -p outerdir/innerdir

* 删除空目录
命令：rmdir【目录名】
作用：删除非空目录或文件
rm -rf 【目录/文件】
-r 删除目录
-f 强制
单纯rm或者rm -r会问你真的要删文件或者目录吗？
千万别打rm -rf /会删掉根目录下的所有文件！！！

* 复制命令
cp 【选项】【原文件/目录】【目标目录】
选项如下：
r 复制目录
p 连带文件属性复制
d 若原文件是链接文件，则复制链接属性
a 相当于-pdr，保证和原文件属性一模一样！
例子：
cp -r japan/ /Users/Jeff/Documents //复制japan目录下的目录到文稿目录下
cp -r japan /Users/Jeff/Documents //复制japan目录到文稿目录下
另外，ll就是ls -l命令！

* 剪切或改名命令，在同一目录下就是改名
mv 【原文件或目录】【目标目录】

## 文件搜索命令

1. 文件搜索命令
locate 【文件名】 
它的更新速率默认是一天一次，可以使用updatedb命令强制更新数据库

2. 搜索命令的命令
whereis【选项】命令名
b 只查找可执行文件
m 只查找帮助文件

3. which 命令名 可以查看别名，echo $PATH查看路径

4. 文件搜索命令
find【搜索范围】【搜索条件】
-name 按照文件名搜索
-iname 文件名不区分大小写

* Linux中的通配符
\* 匹配任意内容（任意多个字符）
? 匹配任意一个字符
[] 匹配任意一个中括号内的字符
如find desktop/ -name "ab[cd]" 匹配abc或abd

* 按照所有者来搜索
-user 按照所有者搜索,如find /root -user root    是找root目录下所有者为root的文件
-nouser 是找没有所有者的文件，如find /root -nonuser  是找root目录下没有所有者的文件

* 按照时间来搜索
find 【搜索范围】 -(X)time (+/-)时间
如find /root -mtime +10是查找十天前修改的文件
+10 10天前修改的文件
10 10天当天修改的文件
-10 10天内修改的文件
atime 访问文件的时间
ctime 改变文件属性的时间
mtime 修改文件内容的时间

* 按照大小来搜索
find 【搜索范围】-size (+/-)X(k/M)
小写k是kB，大写M是MB
如find /root -size +10k是查找root目录下大小大于10kB的文件

* 按照i节点来搜索
find 【搜索范围】-inum 【i节点号】
和ls -i 【文件名】正好相反

* 多条件查询
find /root -size +20k -a -size -50k查找20~50kB大小的文件
find /root -size -20k -o -size +50k查找小于20或者大于50kB的文件
find /root -size +20k -a -size -50k -exec ls -lh {}\;查找20~50kB的文件，并列出详细信息
-exec/-ok 命令 {} \;用来继续处理搜索到的文件
find /root -inum 606838 -exec rm -rf {} \;找到i节点号为606838的文件然后删除


## 压缩命令

Linux中最常见的压缩格式有：.zip .gz .bz2 .tar.gz .tar.bz2

1. .zip格式压缩
zip 压缩文件名 原文件 #压缩文件
zip -r 压缩文件名 源目录 #压缩目录

2. .zip格式解压缩
unzip 压缩文件 #在哪个目录下操作就解压到那个目录下

3. .gz格式压缩
gzip 源文件 #压缩后源文件会被删除
gzip -c 源文件 > 压缩文件 #源文件被保留
如gzip -c cangls > cangls.gz
gzip -r 目录 #只能压缩目录下所有的子文件，但不能压缩目录
可以使用通配符统一处理多个压缩文件，如rm -rf *.zip

4. .gz格式解压缩
gzip -d 压缩文件
等价于gunzip 压缩文件，会把源压缩文件删除

5. .bz2格式压缩
bzip2 源文件 #压缩后源文件会被删除
bzip2 -k 源文件 #源文件被保留
此命令不能压缩目录！

6. .bz2格式解压缩
bzip2 -d 压缩文件可以用-k保留源文件
等价于bunzip2 压缩文件同样可用-k

** 下面两个打包用的较多 **

7. 通过打包.tar解决.gz和.bz2压缩目录的不便之处
* 常用压缩格式：.tar.gz和.tar.bz2

* 打包命令
tar -cvf 打包文件名 源文件
-c 打包
-v 显示过程
-f 指定打包后的文件名
如tar -cvf longzls.tar longzls
然后对打包文件进行压缩

* 解打包命令
tar -xvf 打包文件
-x 解打包
如tar -xvf longzls.tar

* .tar.gz格式
tar -zcvf 压缩包名.tar.gz 源文件 压缩
tar -zxvf 压缩包名.tar.gz 解压缩
tar -ztvf 压缩包名.tar.gz 查看压缩包内容

* .tar.bz2格式
tar -jcvf 压缩包名.tar.bz2 源文件 压缩
tar -jxvf 压缩包名.tar.bz2 解压缩
tar -jtvf 压缩包名.tar.bz2 查看压缩包内容

* 可以在解压缩命令后加上-C 指定目录将解压出来的文件放到其他目录

* 可以在压缩命令的源文件处空格分开多个文件一起压缩，如tar -zcvf jp.tar.gz japan install.log


## 用户登录查看命令

1. 查看用户登录信息

* w 用户名 （不加用户名也可以）
命令输出：
USER：登录的用户名
TTY： 登录终端
FROM：从哪个IP地址登录
LOGIN@：登录时间
IDLE：用户闲置时间
JCPU：指的是该终端连接的所有进程占用的时间。这个时间里并不包括过去的后台作业时间，但却包括当前正在运行的后台作业所占用的时间
PCPU：是指当前进程所占用的时间
WHAT：当前正在运行的命令

* who 用户名 和w一样，只不过更简单（不加用户名也可以）
命令输出：
-用户名
-登录终端
-登录时间（登录来源IP地址）
查看过去所有用户的开关机重启信息

* last 默认是读取/var/log/wtmp文件数据，这是一个二进制文件，防止人为修改
命令输出：
-用户名
-登录终端
-登录IP
-登录时间
-退出时间（在线时间）
查看所有用户的最后一次登录信息

* lastlog 默认读取/var/log/lastlog文件数据


## linux命令

[root@localhost ~]#

root 用户名
~ 当前所在位置
# 超级管理员  /root
$ 为普通用户 /home/name/

  pwd 显示当前所在位置


linux命令格式
【命令】 【选项】 【参数】

  -a 显示当前目录所有选项

  ls 后面可加命令

  ls -l 显示详细信息
  ls -lh 显示更人性化
  ls -a 显示所有文件，包括隐藏文件
  ls -d 查看目录属性
  ls -i 显示 inode

  文件类型 - 普通文件  d 目录文件  l 软链接文件

  目录文件处理命令：

  mkdir -p [目录名]  -p为递归创建，可选  比如 mkdir -p japan/canglaoshi  这样可以创建不存在的上一级目录

  cd ~ 回到当前用户家目录
  cd - 进入上次所在目录


删除空目录

rmdir [目录名]  删除空目录，少用

rm -rf [目录名] 删除非空目录

-r 删除目录
-f 强制删除

不要轻易用 -rf ，删了就没了

复制目录：
cp [选项] [源文件] [目标目录]

选项 

-r 复制目录
-p 连带文件属性复制  比如时间
-d 若源文件是链接文件，则复制链接属性
-a 相当于  -pdr

ll命令 ==  ls -l


#### 链接命令  ln

ln -s 源文件 目标文件

硬链接，软链接

硬链接： 一个文件的两个不同接入点。可以理解为，一个教室分为前门和后门，不能跨分区，不能针对目录，只能针对文件  


软链接：软链接特征 ** 软链接用的比较多
1. 类似于windows快捷键
2. 修改任意文件，另一个都改变
3. 删除原文件，软链接不能使用
4. 软链接文件权限都为rwxrwxrwx

做软链接，一定要写绝对路径

## 文件搜索命令

文件搜索命令 locate 
命令搜索命令 whereis 与 which
文件搜索命令 find
字符串搜索命令grep

1. 
locate 文件名

作用：在后台数据库中按文件名搜索，locate文件不是实时更新，所以新建的文件可能找不到。

可以更新一下文件名 updatedb
记不得文件名，可以  locate locate

缺点： 只可以按照文件名来搜索，有些文件搜索不到，可能 /etc/updatedb.conf 配置文件里没设置。可进行设置

2. whereis 与 which 命令搜索命令
whereis 只能搜索系统命令 
which，跟whereis类似。 可以看到命令别名

eg: whereis ls 
    whoami 
    whatis ls

3. find命令
避免大范围搜索，会非常耗费系统资源

find 目录 -name "*文件名"  
* 通配符
? 一个名


find /root -iname install.log  不区分大小写

find /root  -user root  当前目录下，所有者名字

find /root -nouser  没有所有者的名字

find /var/log/ -mtime +10
+10天前修改的文件
10  10天当天修改的文件
-10 10天内修改的文件

atime  文件访问时间
ctime 改变文件属性
mtime 修改文件的内容


find . -size 25k  查找文件大于25k的文件
find /etc -size 25k

+25k 大于25k
25k 刚好25k 
-25k 小于25k

find /etc -size +20k -a -size -50k

-a  and与
-o or或

find /etc -size +20k -exec ls -lh {} \;

-exec/-ok 命令 {} \; 对搜索结果执行操作


grep 字符串搜索目录

grep [选项] 字符串 文件名
在文件当中匹配符合条件的字符串  可用正则表达式

-i 忽略大小写
-v 排除指定字符串，取反


find和grep区别： find主要在系统中搜索符合条件的文件名
grep在文件中搜索符合条件的字符串



## linux帮助命令
帮助命令 man
man -f 命令  查看某命令的等级

whereis ifconfg

## linux压缩命令

常用压缩格式 .zip .rar 


zip 压缩文件名  源文件
eg： zip cangls.zip cangls

压缩目录：
zip -r 压缩文件名 源目录
ziP -r jp.zip jp

解压文件：
unzip 压缩文件

命令： gzip 源文件
压缩为.gz 格式的压缩文件，源文件会消失

命令： gzip -c 源文件 > 压缩文件
压缩为.gz格式，源文件保留
eg: gzip -c cangls > cangls.gz

命令：gzip -r 目录
压缩目录下所有的子文件，但是不能压缩目录

gzip -d 压缩文件
解压.gz的文件


** .tar.gz 与 .tar.bz2 ** 这两种用的较多

tar -cvf 打包文件名 源文件
-c 打包
-v 显示过程
-f 指定打包后的文件名
-z 压缩为 .tar.gz格式
eg： tar -cvf longzls.tar longzls

tar -zxvf 打包文件名
解压打包 
  -x 解打包


tar -ztvf 打包文件名
查看压缩文件名


## linux关机和重启命令： 谨慎操作

shutdown [选项] 时间 
选项：
-c 取消前一个关机命令
-h 关机
-r 重启

date查看时间

eg： shutdown -r 05:30 &  
eg： shutdown -h now

runlevel  系统运行级别

logout退出登录命令

## linux其他常用命令

挂载命令： 分配盘服，类似于U盘

mount 查询系统中已经挂载好的命令


mount -a 依据配置文件/etc/fstab的内容，自动挂载


## vim编辑器

https://github.com/jeffacode/Learning-Linux.git  笔记

vi类似于 windows中的记事本

vi有三种操作模式：

命令模式、输入模式、底行模式

vim abc  如果abc存在，直接打开，否则是创建一个文件

刚进入的时候，是命令模式，输入  i，切换到编辑模式

保存退出： esc :wq 回车

cat abc  查看文件中的内容


dd 删除


## vim 命令模式

：q!  强制退出

vim + abc abc为文件名，打开文件，将光标定位在最后一行

vim +3 abc 跑到光标第三行

vim +/imooc abc 打开abc文件，第一次出现imooc的那一行

按字母n，可以在imooc出现的地方多次切换

vim aa bb cc  一次性打开或者创建多个文件，aa,bb,cc为文件名

：n  用来切换这三者文件 
：N  回来往后切换文件


## 底行模式和命令模式常用指令

：w 保存
: q 退出
:! 
：ls 列出刚才打开的所有文件
：15 光标切换到15行
/xxx  从光标位置开始，搜搜xxx出现第一行位置

-h 光标左移
-j 光标下移
-k 光标上移
-l 光标右移

ctrl + f 向下翻页
ctrl + b 向上翻页
ctrl + d 向下翻半页
ctrl + u 向上翻半页

dd 删除光标所在行
o 在光标所在行的下方插入一行并切换到输入模式
yy 复制光标所在的行
p 在光标所在行的下方粘贴
P大写  在光标所在行的上方粘贴
