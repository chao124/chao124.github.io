---
title: hexo:更换电脑，继续写博客
date: 2018-04-15 20:32:32
tags: [博客,hexo]
---

## 1.将你原来电脑上已经配置好并生成的hexo目录拷到你的新电脑上，注意无需拷全部，只拷如下几个目录：

<!--more-->

```
 _config.yml
package.json
scaffolds/
source/
themes/
```

将这些目录放到一个目录下，如：hexo／

## 2.在你的新电脑上首先配置hexo环境：安装Node.js

## 3.安装hexo，执行命令：

```
npm install -g hexo
```

## 4.安装好之后，进入hexo／目录

## 5.模块安装，执行命令：

```
 npm install
 npm install hexo-deployer-git --save
 npm install hexo-generator-feed --save
 npm install hexo-generator-sitemap --save
```



## 6.部署，执行命令：

```
 hexo g
 hexo deploy
```