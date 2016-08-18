---
title: 搭建博客个人博客（一）
date: 2016-08-18 23:33:02
tags:
- Blog
---
# 搭建个人博客（一）


**前言**

作为一个程序员，平时有一些好的想法，或者笔记想要记录；虽然有很多不错的网站，比如博客园，CSDN······· 但作为一个支持开源的用户，和一个技术控，怎能没有一个自己的博客呢？幸运的是，因为公司要求写一篇Markdown语法的博客，就想着索性自己搭一个博客吧。so，利用这两天的时间，在网上看了很多的教程，又在自己的不断摸索下，终于尝试成功了，搭建了一个github + hexo + 自己域名 + Next 主题 的一个博客

地址：
>	Alvin-blog [http://sialvsic.com](http://sialvsic.com)

**感想**

想想发现，确实有很多的博客写的不错，但是因为时间的缘故，技术的更新和网址的变更带来很多的问题，而且博文的水平参差不齐，我看的第一篇文章就是带有错误的文章，很可能一个小小的命令的错误就会带给读者很大的困惑，感觉博主也应该对此负责，时常的更新或者有一些提醒；综上，想分享一下我的过程，希望能够给读者带来一些帮助。

**声明**

本文适合
- linux和mac的用户，对windows的用户有一定的参考价值
- 有一定基础的人员参考，对于0基础人员权当是学习一下

**前提**
- git
- github 特别是gh-pages
- markdown


**START**

我会分阶段逐步完成整个过程，希望大家可以边看边完成

**ToDoList**
- **安装npm**
- **利用hexo搭建本地博客**
- **在GitHub上发布博客**
- **购买域名**
- **用自己的域名关联博客**
- **优化hexo博客**


------------------


## 知识简介
### GitHub简介

> **GitHub** is a Web-based Git repository hosting service。    —— [GitHub](https://github.com/)

即：GitHub就是一个基于Web的托管服务，当然GitHub的功能不单单有这些。


### GitHub Pages 简介
> **GitHub Pages** 本用于介绍托管在 GitHub 的项目， 不过，由于他的空间免费稳定，用来做搭建一个博客再好不过了。    — [GitHub Pages](https://pages.github.com/)

GitHub Pages可以被认为是用户编写的、托管在github上的静态网页。


![Alt text](http://obqvt6b56.bkt.clouddn.com/blog-githubPages.jpg)

### Hexo简介
> 快速、简洁且高效的博客框架    —— [Hexo](https://hexo.io/zh-cn/)

![Alt text](http://obqvt6b56.bkt.clouddn.com/hexo.png)

## 安装必备软件
可以参考 [https://hexo.io/zh-cn/docs/](https://hexo.io/zh-cn/docs/) 安装下列两个软件

### 1.安装Npm
> npm — a package manager for javascript：node 的包管理器

github:  https://github.com/npm/npm

官网：https://www.npmjs.com/

对于Linux和Mac 的用户直接点开上面的github的地址，执行`Fancy Install (Unix)`中的命令即可

实质上安装完node.js后会带有npm，所以可以按照官网的方式安装node.js即可

### 2.安装[Git](http://git-scm.com/)

建议使用包管理器来安装
Linux:  
```
apt-get install git
```
Mac：
```
brew install git
```


## 利用hexo搭建本地博客

### 安装hexo
安装hexo
所有必备的软件安装完成后，即可使用 npm 安装 Hexo
```  bash
$ npm install -g hexo-cli
```
### 本地初始化博客
1.安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件

``` bash
$ hexo init <folder>
$ cd <folder>
$ npm install
```

2.初始化完成后，运行如下命令自动生成hexo 博客相关的文件

``` bash
$ hexo g
```

3.运行如下命令，打开本地的服务器

``` bash
$ hexo s
```

4.在浏览器中打开localhost:4000，既可看到hexo运行的结果，本地的博客也就搭建完成了

### 如何写博客
使用hexo 写一篇博客是非常简单的，输入以下命令


```
$ hexo new <title>    // hexo new firstarticle
```
即可看到在`source/_posts` 文件夹下生成一个`<title>`命名的md（markdown）文件，用自己的喜欢的编辑器打开这个文件就可以书写博客了

```
INFO  Created: ~/xxxx/source/_posts/firstarticle.md
```

### 发布博客
书写完成后，运行
```
hexo g
```
自动生成博客文件，之后运行

```
hexo s
```

可以在localhost:4000看到自己写的博文



## 在github上发布博客
### 建立个人的GitHub的仓库
1.github注册帐号

2.新建库 可以看到有个`绿色`的New按钮

![Alt text](http://obqvt6b56.bkt.clouddn.com/blog-github-person.png)

点击进入后，在此处填入的库名个是为:  账户名.github.io
比如：我的账户名为sialvsic，那么此处填入的库名为 `sialvsic.github.io`

![Alt text](http://obqvt6b56.bkt.clouddn.com/blog-github-new.png)

点击create repository后，即可在github上生成一个git库

*note* ：利用**账户.github.io** 这种方式搭建博客，每个github账户只能创建一个。

### 发步到GitHub上

1.配置xx文件

``` bash
deploy:
type: git
repo: <repository url>
branch: [branch]
message: [message]
```

2.此时执行hexo deploy的命令(注意命令行中的行为)，即可将自己刚刚生成的博客发布到github上，可以登录以下url查看

```
http://账户名.github.io       eg:http://sialvsic.github.io
```

 *note:* 可能deploy之后不会立即看到结果，需要等待10分钟左右。


 当然到现在为止，一个免费的博客已经建立了，但唯一的问题就是以这个以下网址访问显得过low，身为博主，当然得有自己的域名，so 下一篇开始详述设置自己的域名

```
http://xxxx.github.io
```

## 常见问题

1.一开始以为要像平时使用github，要将所有的文件要放在 github 的库中

**对于sialvsic.github.io/库名** 此类项目
 文件的内容push到gh-pages分支，要呈现的页面以index.html为开始页面，github的会自动识别

**对于sialvsic.github.io** 这种，github官方的文档说内容要push在master分支中，我最初不清楚**hexo的工作原理**，将整个hexo的文件夹push到sialvsic.github.io库中的master分支上，但是发现并不起作用，最后我才明白，原来根本不需要自己push到库中，在执行hexo deploy命令时，hexo会自动生成的html + css这些东西push到库中，而这个映射关系就是自己在_config.yml文件中写的这个配置

```  bash
deploy：
type：
repo：<repository url>
branch：[branch]
message：[message]
```
