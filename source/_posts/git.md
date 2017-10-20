---
title: git
date: 2017-10-10 12:55:35
tags: [git]
---
## git工作原理

先用一张图解释一下git的工作原理：

![git](https://dn-Myg6wSTV.qbox.me/fea7461e85aee80bbe96.png?imageView2/0/w/1280/h/960)

1. workspace:工作区，执行git add *命令就把改动提交到了暂存区，执行git pull命令将远程仓库的数据拉到当前分支并合并，执行git checkout branch-name切换分支
2. index：暂存区，执行git commit -m '说明'命令就把改动提交到了仓库区（当前分支）
3. repository：仓库区（本地仓库），执行git push origin master 提交到远程仓库，执行git clone 地址将克隆远程仓库到本地
4. remote: 远程仓库，就是类似github，coding等网站所提供的仓库

<!--more-->

## 忽略特定的文件

可以配置Git忽略特定的文件或者文件夹，这些配置都写在.gitignore文件中

## 追踪空的文件夹

git会忽略空的文件夹。但是如果想让版本控制包括空的文件夹，习惯上会在空的文件夹下创建一个.gitkeep文件；实际上只要文件夹下有文件，这个文件夹就会在版本控制范围内，所以该文件名是不是.gitkeep根本不重要；

## 配置

* 显示当前Git配置
git config --list
* 设置提交代码时的用户信息
git config [--global] user.name '[name]'
git config [--global] user.email '[email address]'

## 创建仓库

* 在当前目录创建一个文件夹
mkdir [project-name]
* 在当前目录创建一个Git代码库
git init
* 新建一个目录，并将其初始化为Git代码库
git init [project-name]
* 下载一个项目和它的整个代码历史
git clone [url]

## 提交文件

* 添加当前目录所有变动到暂存区
git add .
* 提交暂存区到仓库区
git commit -m [message]
* 为远程Git更名为origin
git remote add origin git@github.com:barefootChild/A-frame_demo.git
* 推送本次修改，首次推送需要加上-u，origin是远程Git名字，可以自定义，master是默认分支，可以根据具体所在分支进行修改
git push -u origin master
* 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a
* 使用一次新的commit，替代上一次提交，如果代码没有任何变化，则用来改写上一次commit的提交信息
git commit --amend -m [message]

## 分支

* 列出所有分支
git branch
* 列出所有远程分支
git branch -r
* 列出所有本地分支和远程分支
git branch -a
* 新建一个分支，但仍然停留在当前分支
git branch [branch-name]
* 新建一个分支并切换至该分支
git checkout -b [branch-name]
* 新建一个分支，指向指定commit
git branch [branch-name] [commit]
* 新建一个分支，与指定的远程分支建立追踪关系
git branch --track [branch] [remote-branch]
* 切换到另一个分支，并更新工作区
git checkout [branch-name]
* 合并指定分支到当前分支
git merge [branch-name]
* 选择一个commit，合并进当前分支
git cherry-pick [commit]
* 删除分支
git branch -d [branch-name]
* 删除远端分支
git branch -dr [branch-name]
git push origin --delete [branch-name]

## 标签

* 列出所有标签
git tag
* 新建一个tag在当前commit
git tag [tag]
* 新建一个tag在指定commit
git tag [tag] [commit]
* 删除本地tag
git tag -d [tag]
* 提交指定tag
git push [remote] [tag]
* 提交所有tag
git push [remote] --tags
* 新建一个分支，指向某个tag
git checkout -b [branch-name] [tag]

## 撤销某些更改

* 撤销对某个文件的修改
git checkout [file]
* 撤销暂存的文件
git reset [file]
* 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --hard [commit]
* 新建一个commit，用来撤销指定的commit
git revert [commit]
* 暂时将未提交的变化移除，稍后再移入
git stash
git stash pop

