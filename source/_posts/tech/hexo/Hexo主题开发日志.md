---
title: Hexo主题开发日志
date: 2024-02-20 13:09:21
tags: hexo
series: hexo
---

# Hexo 主题开发日志

## 繁星点点
  - 在主题HTML模板pug中频繁提到的theme，是一个类似全局变量的东西，目测应该是hexo框架通过读取主题配置文件`themes/_config.yml`生成的用户配置变量
  - 之前打开那个`handsome`主题文件的时候发现是php语法，感觉看不懂就没看，今天上午详细阅读一番，发现php真实比幻兽帕鲁更加缝合怪的存在，类方法他用`class::method`,类属性他用`class->property`，对象属性又用`obj['propertyName']`，不知道有没有泛型的概念。。可谓集百家之所长！

## 疑问
  - 为什么主题中的`aplayer.pug`模板通过直接引入aplayer资源就能直接生成一个音乐播放器？另外目前的音乐播放器只能播放非vip的音频资源，我想让他播放vip歌曲（我有vip）。
    - 现在我从之前用过的一个php实现的主题中找到一点端倪，应该能有帮助，源码在本地叫`handsome`主题，相关文档在[这里](https://auth.ihewro.com/user/docs#/preference/player) [这里](https://auth.ihewro.com/user/docs#/preference/shortCode?id=%e9%9f%b3%e4%b9%90)

## hexo常用命令

新建一篇文章
 ```sh
 hexo new [layout] <title>
 hexo new [layout] <title> --path ./<文件夹名>/文件名
 ```
- layout：设置新文章要使用的布局模板，可以是页面、帖子、草稿等。
- title：文章的文件名

本地启动服务
```sh
hexo s
```
- 默认端口4000

构建内容并部署到github.io
```sh
hexo cl ## 清除项目中/public文件夹
hexo g ## 生成hexo站点根目录下public文件夹的内容
hexo d ## 部署站点，本地生成`.deploy_git`文件夹，并将站点内容上传到github
```
需要更新github上的站点内容时，也可以使用如下命令：
```sh
hexo clean && hexo g && hexo s
```