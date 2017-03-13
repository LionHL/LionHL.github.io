---
title: git 分支操作
date: 2017-03-13 14:35:58
tags:
---

# git 分支
### 创建一个名为 dev 的分支  
 
```git
git checkout -b dev
```
### 推送本地的dev分支，将其作为远程的dev分支 （本地分支名:远程分支名）
```git
git push origin dev:dev
```
or  

```git
git push origin dev
```
### 作了修改之后提交到远程dev分支
```git
...
git push -u origin dev  
//此时有了跟踪分支，本地dev分支跟踪了 origin/dev
```
```git
//查看跟踪分支
git branch -vv
```
```git
//查看远程分支
git branch -r
```
```git
//查看所有分支
git branch -a
```

### 变基rebase
```git
git checkout dev
git rebase master
//上面两步可合并为 git rebase master dev
git checkout master
git merge dev
git push
```

### 删除分支
```git
git branch -D dev
git push origin --delete dev
```
### 远程更新取回本地git fetch
```git
git fetch <远程主机名>
//清理本地分支
git fetch -p
```
### 建立远程跟踪分支
```git
git checkout -b serverfix origin/serverfix
```