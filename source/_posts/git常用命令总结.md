---
title: git常用命令总结
date: 2017-03-31 15:50
categories:
tags:
     - git
---

平时要用一些命令老是去网上找还挺麻烦的，所以总结起来，方便以后自己的查阅

<!--more-->

## 正常的开发流程命令

1. 首先进入项目的主分支

2. Fork一份工程，当做自己的项目管理分支
&ensp;&ensp;&ensp;Fork的作用:
&ensp;&ensp;&ensp;相当于你在原项目的主分支上又建立了一个分支，你可以在该分支上任意修改，如果想将你的修改合并到原项目中时，可以pull request，这样原项目的作者就可以将你修改的东西合并到原项目的主分支上去，这样你就为开源项目贡献了代码，开源项目就会在大家共同的努力下不断壮大和完善。

3. 在电脑上创建一个文件夹，先Clone一份自己工程的项目分支(xxx屏蔽公司信息)
```
Git clone git@xxxx.gitlab.com:xxxxxx/SELand_Vertu
```

4. 进入项目的二级目录进入git客户端，确认要pull分支
      `git branch `                看看当前的分支
      `git checkout -b develop`    切换到develop分支，因为我要pull拉去develop分支上的项目


5. 然后在将自己的项目分支同步项目主分支（我们项目分支为develop分支） 
```
git pull git@xxx.gitlab.com:xxx/SELand_Vertu develop
```

6. 每次提交代码时候，需要先同步项目主分支代码
* `git status`               是哪些文件有所修改
* `git diff`                 可以查询所修改的代码
* `git add -A `              增加自己所做的修改
* `git commit -a`            提交所有修改的代码 ，加注释是这样  `git commit -a -m "这里是注释的内容"`
* `git push origin develop`  提交代码,这里的提交只是提交到了项目的develop分支上面，还没提交到master上面

## 若代码有冲突，可以这样解决

1. `git pull git@xxx.gitlab.com:xxx/SELand_Vertu develop`     先同步一下会出现以上的错误
2. pull会使用git merge导致冲突，需要将冲突的文件resolve掉   `git add -u`,
3. 在项目中看看哪些代码是对方改的，哪些代码是自己修改的，在合并成一份最新的代码
4. `git commit` 之后才能成功

## 添加修改
1. 添加文件到暂缓区：
* `git add -A .`一次添加所有改变的文件
* `git add xx`将xx文件添加到暂存区
* `git add .` 添加新文件和编辑过的文件不包括删除的文件
* `git add -u` 添加编辑或者删除的文件，不包括新添加的文件。

2. commit文件
* `git commit -m "这里是注释"` 提交的是暂存区里面的内容，也就是 Changes to be committed 中的文件。
* `git commit -a -m "这里是注释"` 除了将暂存区里的文件提交外，还提交 Changes bu not updated 中的文件。
* `git commit --amend`有时候我们会发现有几个文件漏了提交或者想修改一下提交信息，又或者忘记使用 -a 选项导致一些文件没有被提交，我们希望对上一次提交进行修改，或者说取消上一次提交，这时候我们需要使用 --amend 选项。
* `git commit --amend -a`用来当我们发现在提交时忘记使用 -a 选项，导致 Changes bu not updated 中的内容没有被提交

## 撤销修改

### 1.撤销commit
方法1:
&ensp;&ensp;执行`git log`查看 commit日志，然后`git reset --hard commit_id `commit_id是控制台上的hash值
方法2: 
&ensp;&ensp;`git reset  –hard HEAD^`,如果是上上一个版本`git reset  –hard HEAD^^`,如果是上一百个版本`git reset  –hard HEAD~100 `;
方法3: 
&ensp;&ensp;`git checkout  —-文件名` 撤销对某个文件的修改,例：`git checkout  —-readme.txt`；
&ensp;&ensp;`git checkout -- .`撤销对所有文件的修改

！！注意： 撤销之后，由于本地版本低于线上版本，想要提交代码，只能强行提交，覆盖线上，可以使用下面的命令：`git push -f origin 分支名`
### 2.恢复到某一版本
现在我又发觉我最新的版本是没错的，我不想撤销了，我要回到最新版本，两步:
`git reflog`查看历史版本；`git reset  –hard 版本号 `
### 3.撤销add
&ensp;&ensp;`git reset head <文件名>`   撤销对某个文件的add命令
&ensp;&ensp;`git reset head .`  撤销所有文件的add命令

## 创建与合并分支命令如下：
* 查看分支：`git branch`
* 创建分支：`git branch name`
* 切换分支：`git checkout name`
* 创建+切换分支：`git checkout –b name`
* 合并某分支到当前分支：`git merge name`
* 删除分支：`git branch –d name`

## github提交时想忽略某些文件
比如我提交的时候，不想提交`node_modules`这个文件夹或者想忽略掉更多的文件夹，可以在github上或者在你的文件中添加`.gitignore`这个文件
`.gitignore`里面的内容参考如下：
```bash
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

## 本地文件想提交到远程
1. 如果本地没有初始化git,在本地执行 `git init`
2. `git add -A .`添加所有文件到暂缓区
3. `git commit -a -m "添加所有文件"`
4. `git remote add origin https://github.com/xianyulaodi/blogBackups.git`。注意需要将origin后面换成自己的Git地址。
5. 将本地仓库推送到远程仓库`git push -u origin master`第一次需要这样，以后只要执行`git push origin master`
6. **关键！！**在执行该命令时有时候会出错，原因是远程的文件未同步下来。此时可以先执行：`git pull --rebase origin master`将远程文件同步下来。然后在执行推送即可。
  完成后在后续的推送文件到远程仓库中可直接执行`git push origin master`
可以看看[这里](http://blog.sina.com.cn/s/blog_6cf7405b0102w5f9.html)

## git常用命令
* 创建一个空目录 XX指目录名` mkdir XX `
* 显示当前目录的路径`pwd`
* 把当前的目录变成可以管理的git仓库，生成隐藏.git文件`git init`
* 把xx文件添加到暂存区去`git add XX`
* 提交文件 –m 后面的是注释`git commit –m "XX"`
* 查看仓库状态 `git status`
* 查看XX文件修改了那些内容 `git diff XX`
* 查看历史记录 ` git log`
* 回退到上一个版本 `git reset  –hard HEAD^` 或者 `git reset  –hard HEAD~ `
如果想回退到100个版本，使用`git reset –hard HEAD~100`
* 查看XX文件内容 `cat XX`
* 查看历史记录的版本号id `git reflog`
* 把XX文件在工作区的修改全部撤销 `git checkout --XX`
* 删除XX文件 `git rm XX`
* 关联一个远程库 `git remote add origin https://github.com/xx`
* 把当前master分支推送到远程库 `git push –u(第一次要用-u 以后不需要) origin master`
* 从远程库中克隆 `git clone https://github.com/xx`
* 创建dev分支 并切换到dev分支上 `git checkout –b dev`
* 查看当前所有的分支 `git branch`
* 切换回master分支 `git checkout master`
* 在当前的分支上合并dev分支 ` git merge dev`
* 删除dev分支 `git branch –d dev`
* 创建分支 `git branch name`
* 把当前的工作隐藏起来 等以后恢复现场后继续工作 `git stash`
* 查看所有被隐藏的文件列表`git stash list`
* 恢复被隐藏的文件，但是内容不删除`git stash apply `
* 删除被隐藏文件 `git stash drop`
* 恢复文件的同时 也删除文件 `git stash pop `
* 查看远程库的信息`git remote`
* 查看远程库的详细信息 `git remote –v`
* 把master分支推送到远程库对应的远程分支上 `git push origin master` 
* 把分支推送到远程的分支`git push origin develop`

## 常见问题

1、![](http://tutorialspots.com/wp-content/uploads/2016/10/Another-git-process-seems-to-be-running-in-this-repository.jpg)   
解决方法：执行`rm .git/index.lock`

2、在git pull代码的时候，可能会遇到这个问题
```bash
error: Your local changes to the following files would be overwritten by merge:
    xxx/xxx/xxx.php
Please, commit your changes or stash them before you can merge.
Aborting
```
出现这个问题的原因是其他人修改了xxx.php并提交到版本库中去了，而你本地也修改了xxx.php，这时候你进行git pull操作就好出现冲突了，解决方法，在上面的提示中也说的很明确了。

* 保留本地的修改 的改法

1）直接commit本地的修改

2）通过`git stash`
```bash
git stash
git pull
git stash pop
```
通过`git stash`将工作区恢复到上次提交的内容，同时备份本地所做的修改，之后就可以正常git pull了，git pull完成后，执行git stash pop将之前本地做的修改应用到当前工作区。

git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。

git stash pop: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复。

git stash list: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。

git stash clear: 清空Git栈。此时使用gitg等图形化工具会发现，原来stash的哪些节点都消失了。

* 放弃本地修改 的改法
`git reset --hard`
`git pull`

3、 ![](http://images2015.cnblogs.com/blog/630011/201603/630011-20160315120522896-1718649799.jpg)
git 在pull或者合并分支的时候有时会遇到这个界面。可以不管(直接下面3,4步)，如果要输入解释的话就需要:

1.按键盘字母 i 进入insert模式

2.修改最上面那行黄色合并信息,可以不修改

3.按键盘左上角"Esc"

4.输入":wq",注意是冒号+wq,按回车键即可
     


