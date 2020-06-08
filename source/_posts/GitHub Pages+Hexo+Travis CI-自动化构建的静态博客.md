---
title: GitHub Pages+Hexo+Travis CI-自动化构建的静态博客
date: 2020-06-08 21:22:55
tags: 博客
toc: true
---

### 博客说明

本博客托管于GitHub Pages，使用Hexo作为博客框架，使用Travis CI完成自动构建。
博客源码放在master分支，编译生成的静态文件放在gh-pages分支。每次写完文章后push到master上，Travis CI会自动编译出静态文件并部署到gh-pages分支，通过博客左侧 build status可以看到每次构建的过程。

### 搭建步骤

>环境：Windows 10

#### 1.本地安装Node.js，Git

#### 2.在GitHub新建repository
这里有两种创建方式，对应的GitHub pages地址也不一样：
1.仓库名为fuos.github.io
Hexo生成的静态博客文件需要放在master分支，博客地址为 fuos.github.io
2.仓库名为my-blog或者其他任意名称（本博客采用这种方式）
Hexo生成的静态文件需要放在gh-pages分支，博客地址为 fuos.github.io/my-blog
#### 3.安装Hexo

``` bash
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

#### 4.安装theme

本博客选择 indigo 作为主题，并对其中部分内容根据需要做了修改。具体安装和配置请参考官方文档：[文档 | Document](https://github.com/yscoder/hexo-theme-indigo/wiki)

#### 5.推送分支


