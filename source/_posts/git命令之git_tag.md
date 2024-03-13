---
title: git命令之git_tag
date: 2024-02-29 21:31:44
tags: git
series: git
---
> 最近维护组件库经常需要给一发布版本打上标记tag，但是又经常忘记命令，所以在这里记一下。

## 常用命令

### 查看本地所有tag
```sh
git tag
```

### 查看符合`pattern`格式的本地tag
```sh
git tag -l <pattern>
```

### 查看某个tag的详细信息
```sh
git show <tag-name>
```

### 查看远程仓库的tag
```sh
git tag -r <tag-name>
```

### 新增一个tag
```sh
git tag -a <tag-name> -m 'tag-message' ## 创建一个新的标签，名为 <tag-name>。该命令会将在当前提交（HEAD）上创建一个标签, 并附带一个message信息
git tag -a <tag-name> -m 'tag-message' <commit-id> ## 与上面的命令相似, 但是会在指定的<commit-id>的提交上创建标签
```

### 删除一个标签
```sh
git tag -d <tag-name> ## 删除名为<tag-name>的本地标签
```

### 将本地标签推送到远程仓库
```sh
git push origin <tag-name> ## 将名为<tag-name>的一个标签推送到远程
git push origin tags/<tag-name> ## 将名为<tag-name>的**所有**本地标签推送到远程
```