---
title: 'git命令之git_remote'
date: 2024-03-13 13:00:08
tags: 'git'
series: 'git'
---

> 之前的github.io仓库里只有构建之后的产物，还需要一个仓库管理静态博客的源文件，所以要给本地仓库重新制定一个远程仓库。下面是用到的git_remote相关的命令。

## 常用命令

### 查看当前远程仓库的地址
```sh
git remote -v
```

### 删除已有的远程仓库关联
```sh
git remote rm origin
```
这里的`origin`是默认的远程仓库的别名。

### 添加新的远程仓库的地址
```sh
git remote add origin https://xxxxxxxxx
```
这里再次使用`origin`作为别名， 也可以用别的名字。

### 将本地分支与远程分支关联起来
```sh
git push --set-upstream origin main
```
这里的意思是将本地的`main`分支和远程的`origin`下的默认分支关联起来。