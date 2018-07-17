---
title: 自动部署hexo博客到阿里云服务器
categories: Hexo
toc: true
date: 2018-07-17 23:03:44
tags: 自动化
---

# 前言
之前博客是托管在GitHub Page，访问速度不太乐观，后来买了台阿里云ECS，把博客迁了过来，作为一个程序员，过多的手动操作简直是对键盘的侮辱，下面介绍如何将博客直接推送到阿里云ECS(CentOS系统)，实现自动部署。

# 环境
1. 博客网站在服务器上已经搭建好并且可以正常访问；
2. 服务器上已经安装git；
3. 本地hexo能够正常运行。

# 思路
在阿里云服务器上搭建git仓库，本地博客目录下运行`hexo g -d`生成静态文件，并提交到git仓库，从而触发git hook，最后再执行bash命令将文件拷贝到博客网站目录。

# 创建仓库
在阿里云服务器上创建git仓库，注意不要漏掉`--bare`参数。
```
mkdir blog.git && cd blog.git
git init --bare
```

# Hexo配置
修改本地博客目录下的`_config.yml`配置，其中`xx.xxx.xx.xxx`是你的服务器ip地址，`/www/blog.git`是你上一步创建的git仓库路径，`master`是分支。
```
deploy:
  type: git
  message: update
  repo: root@xx.xxx.xx.xxx:/www/blog.git,master
```

# 插件安装
此插件的作用是执行deploy时，将hexo生成的静态文件提交到`_config.yml`配置中的`deploy.repo`地址，即上一步中的`root@xx.xxx.xx.xxx:/www/blog.git,master`。
```
npm install hexo-deployer-git --save
```

# 自动部署
本地的deploy命令只是把静态文件提交到git仓库，既然有git hooks，那么我们可以在有文件提交上来时，再将文件拷贝到博客网站目录。
进入到git仓库hooks目录，并创建钩子`post-receive`。
```
cd /www/blog.git/hooks
touch post-receive
vim post-receive
```
然后输入下面脚本：
```
#!/bin/bash -l
GIT_REPO=/www/blog.git
TMP_GIT_CLONE=/www/tmp/blog
PUBLIC_WWW=/www/blog
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```
其中`/www/blog.git`为仓库路径，`/www/blog`为你的博客网站路径，`/www/tmp/blog`是临时目录，git会先将文件拉到临时目录，然后再将所有文件拷贝到博客网站目录`/www/blog`。

更改目录权限：
```
chmod +x post-receive
chmod 777 -R /www/blog
```

# 运行
完成上述步骤之后，就可以测试一下了，在本地博客目录下运行`hexo g -d`，此时可能还需要输入服务器密码，最后输出以下结果说明部署成功：
```
...
INFO  Deploy done: git
```