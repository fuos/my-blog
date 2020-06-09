---
title: GitHub Pages+Hexo+Travis CI-自动化构建的静态博客
tags:
  - Hexo
  - Travis CI
categories: 博客
toc: true
abbrlink: aab15d5f
date: 2020-06-08 21:22:55
---

### 博客说明📝

1. 博客托管于GitHub Pages，使用Hexo作为博客框架，使用Travis CI完成自动构建。
2. 博客源码放在master分支，编译生成的静态文件放在gh-pages分支。
3. 每次写完文章后push到master上，Travis CI会自动编译出静态文件并部署到gh-pages分支。
4. 因为有Travis CI帮助生成和部署，所以可以在Github上直接编辑文章了。
5. 通过本博客左侧 build status链接可以看到每次构建的过程。

### 搭建步骤📐

>环境：Windows 10

#### 1.本地安装Node.js，Git

#### 2.在GitHub新建repository

这里有两种创建方式，对应的GitHub pages地址也不一样：

①仓库名为fuos.github.io（`GitHub pages`）
Hexo生成的静态博客文件需要放在master分支，博客地址为 fuos.github.io

②仓库名为my-blog或者其他任意名称（`Project pages`，本博客采用这种方式）
Hexo生成的静态文件需要放在gh-pages分支，博客地址为 fuos.github.io/my-blog

#### 3.安装Hexo

本地安装和初始化Hexo，<folder>为任意名称，如my-blog：

``` bash
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

#### 4.安装theme🎨

本博客选择 indigo 作为主题，并对其中部分内容根据需要做了修改。具体安装和配置请参考官方文档：[文档 | Document](https://github.com/yscoder/hexo-theme-indigo/wiki)

这里主要修改了三个地方：

①page-about-me跳转地址

由于我创建的仓库是第二种，所以需要对`themes/indigo/layout/page.ejs`做相应的修改（第二行为修改后）

```javascript
<a href="/" class="avatar waves-effect waves-circle waves-light"><%- image_tag(theme.avatar) %></a>
<a href="<%- config.url %>" class="avatar waves-effect waves-circle waves-light"><%- image_tag(theme.avatar) %></a>
```

②启用gitalk评论插件

owner为github account，repo为刚才创建的用于存放博客的repository，GitHub Application在 Settings -> Developer settings -> OAuth Apps申请

```yaml
# use gitalk://github.com/gitalk/gitalk
gitalk: 
  enable: true
  owner: fuos
  repo: my-blog
  client_id: 'GitHub Application Client ID'
  client_secret: 'GitHub Application Client Secret'
  distractionFreeMode: false
```

③优化文章永久链接

进入my-blog安装插件：

```bash
npm install hexo-abbrlink --save
```

修改_config.yml下permalink信息：

```bash
# permalink: :year/:month/:day/:title/
# permalink_defaults:
permalink: posts/:abbrlink.html
# abbrlink config
abbrlink:
  alg: crc32  #support crc16(default) and crc32
  rep: hex    #support dec(default) and hex
```
#### 5.推送分支

这里将本地博客源文件推送到了master分支

```bash
cd my-blog 
git init 
git remote add origin git@github.com:fuos/my-blog.git
git add . 
git commit -am "init blog" 
# 推送到master
git push -u origin master
```

#### 6.使用 Travis CI 构建和部署

①使用GitHub账号登陆[Travis CI](https://travis-ci.org/)，在github中创建access token，在Travis CI你的repository页面Environment Variables新建环境变量，name为GH_TOKEN，Value 为刚才你在 GitHub 生成的 Token

②在my-blog下新建.travis.yml文件，添加下面的内容：

```yaml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```

③修改my-blog下_config.yml中deploy部分

```yaml
deploy:
  type: git
  # repo: https://github.com/<username>/<project>
  # example, https://github.com/hexojs/hexojs.github.io
  # Hexo静态文件默认推送到gh-pages分支
  branch: gh-pages
```

④将修改推送到远端master

```bash
git add .
git commit -m "add travis ci"
git push origin master
```

这样配置之后，Hexo已经能够自动构建和部署了，可以到travis-ci中看到[Job log](https://travis-ci.org/github/fuos/my-blog/jobs/696306109)

之后写文章时可以按照下面的步骤新建和提交， Travis CI 会将Hexo生成的静态文件自动部署到gh-pages分支。

```bash
cd my-blog 
git checkout master 
hexo new "My First Post - Test" 
git add . 
git commit -am"add test post" 
git push
```

💾博客源码：https://github.com/fuos/my-blog

⚙Travis CI：https://travis-ci.org/github/fuos/my-blog



