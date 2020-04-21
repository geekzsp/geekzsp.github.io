---
title: Git常用命令
date: 2019-10-25 16:57:25
tags:
---



记录一下 备忘
学习地址：http://www.liaoxuefeng.com/
http://stormzhang.com/github/2016/07/09/learn-from-github-from-zero6/
## git 安装 配置
1. 安装
2.  配置
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com" 

//非必要配置 (可查看上面的学习地址)
git config --global color.ui true//让Git显示颜色，会让命令输出看起来更醒目：

忽略特殊文件
配置别名 
.
.
.
```

## 基本命令
```
git init   //创建git版本库

git add <file> //添加到暂存区
git add * 添加所有（不包括.gitignore 配置的忽略内容）

git rm  <file> //删除文件

git commit -m "描述" //提交到版本库

git status //状态查看

git diff //查看修改内容

git log //提交历史 

git log --pretty=oneline 

git log --graph //查看分支合并图

git reflog //命令历史

git reset --hard HEAD^ //版本回退 上一版本 前多少版本  head~数字

git reset --hard 3628164 //根据commitid进行版本回退

git checkout -- <file> //丢弃工作区的修改（实是用版本库里的版本替换工作区的版本）

git reset HEAD <file> //丢弃暂存区的修改

git tag <name>//用于新建一个标签，默认为HEAD，也可以指定一个commit id；

git tag -a <tagname> -m "blablabla..."//可以指定标签信息；

git tag -s <tagname> -m "blablabla..."//可以用PGP签名标签；

git tag//可以查看所有标签。

git push origin <tagname>//可以推送一个本地标签；

git push origin --tags//可以推送全部未推送过的本地标签；

git tag -d <tagname>//可以删除一个本地标签；

git push origin :refs/tags/<tagname>//可以删除一个远程标签。
git show <tagname>//查看标签信息
git checkout tag/commitid //版本回退
```
## git 结合远程库 的一些命令
```
ssh-keygen -t rsa -C "youremail@example.com" //生成ssh key

ssh -T git@github.com //测试是否能够连接

git remote add origin git@github.com:michaelliao/learngit.git //建立本地和远程库关联 远程库的名字就是origin，这是Git默认的叫法

git push  origin <分支名称> //推送到远程 第一次要 加 -u 建立分支关联

git clone git@github.com:michaelliao/gitskills.git //克隆 默认是master

git clone -b <指定分支名称> git@github.com:michaelliao/gitskills.git //克隆指定分支

git pull //从远程抓取最新的提交

git remote -v //查看远程库信息
git remote remove origin 解除和远程库关联

```
## .gitignore配置
.gitginore 也要同步上传的远程库
```
/.idea/
*.iml
/target/
```
## 分支管理
>如果你是一个人开发，可能只需要 master、develop 两个分支就 ok 了，平时开发在 develop 分支进行，开发完成之后，发布之前合并到 master 分支

```
git branch //查看分支

git branch <name> //创建分支

git checkout <name> //切换分支
git checkout -b <name> //创建+切换分支

git merge <name> //合并某分支到当前分支

//加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并
git branch -d <name> //删除分支

git branch -D <name> //丢弃一个没有被合并过的分支

git stash

git stash pop //回到工作现场

git stash list

git checkout -b branch-name origin/branch-name//在本地创建和远程分支对应的分支

git branch --set-upstream branch-name origin/branch-name //建立本地分支和远程分支的关联

```

## checkout 

### 切换分支
```
git checkout <name> //切换分支
git checkout -b <name> //创建+切换分支
git checkout -b branch-name origin/branch-name //在本地创建和远程分支对应的分支
```
切换tag，切换到某次commit
```
git checkout tag/commitid //版本回退
```
### 撤销

```git checkout -- <file> //丢弃工作区的修改（实是用版本库里的版本替换工作区的版本）
 ```
## 补充

一. reset的hard，soft，mixed参数区别

git reset :和git add命令作用相反，撤销add操作，把index区内容撤销到working directory

git reset的--hard  --mixed  --soft参数区别：
hard：把working directory和index区的内容都重置为指定的commit 版本
soft：保留现在working directory和index区的内容，HEAD指向制定的commit 版本
mixed：保留working区的内容，将index区和HEAD内容都修恢复到指定的commit版本

二. reset和revert区别
      reset是回朔到指定的commit版本（指定commit版本之后的操作都消失了）。revert是删除指定的commit操作的内容（指定的版本内容消失，之前和之后commit版本内的操作都保留），但是这个操作也会做了一个commit提交版本。
      
三： 如果本地的版本回退使用reset的话,  push 到 远程库 需要加 -f 强制操作  （因为 远程的commit 高于 本地)
四： pull 时候和本地冲突
error: Your local changes to 'c/environ.c' would be overwritten by merge.  Aborting.
 Please, commit your changes or stash them before you can merge.
这个意思是说更新下来的内容和本地修改的内容有冲突，先提交你的改变或者先将本地修改暂时存储起来。
处理的方式非常简单，主要是使用git stash命令进行处理，分成以下几个步骤进行处理。
1、先将本地修改存储起来
$ git stash
这样本地的所有修改就都被暂时存储起来 。是用git stash list可以看到保存的信息：
git stash暂存修改
其中stash@{0}就是刚才保存的标记。
2、pull内容
暂存了本地修改之后，就可以pull了。
3、还原暂存的内容
$git stash pop stash@{0}
系统提示如下类似的信息：
Auto-merging c/environ.c
CONFLICT (content): Merge conflict in c/environ.c
意思就是系统自动合并修改的内容，但是其中有冲突，需要解决其中的冲突。
4、解决文件中冲突的的部分
就可以正常的提交了。
五：强制pull
>git fetch --all  
git reset --hard origin/master 
git pull

git fetch 只是下载远程的库的内容，不做任何的合并 git reset 把HEAD指向刚刚下载的最新的版本