---
title: git命令之git_config
date: 2024-03-13 10:40:07
tags: git
series: git
---
> 换了新电脑之后很多全局的配置都得重新配一遍，有的很容易记不起来，到了要用的时候才发现还没设置，这里先记一下。

## 常用命令

### 查看现有设置
```sh
git config --list // 查看现有git配置的列表
git config -l // 查看git配置的缩写
git config <config-name> // 查看特定配置config-name的值
```

### 设置http/https代理配置
```sh
git config http.proxy http://xxxx:xx // 设置本地http代理URL
git config --global http.proxy http://xxxx:xx // 设置全局http代理URL
git config https.proxy http://xxxx:xx // 设置本地https代理URL
git config --global https.proxy http://xxxx:xx // 设置全局https代理URL
```