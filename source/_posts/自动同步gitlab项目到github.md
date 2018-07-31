---
title: 自动同步gitlab项目到github
categories: 自动化
toc: true
date: 2018-07-31 10:30:34
tags: gitlab
---

## 需求
今天想把`gitlab`上到项目发布到`github`上，但是又不想删掉`gitlab`上的项目，那么有没有办法实现`push`一次，同时将`commit`更新到这两个仓库中呢？

## 步骤
![操作步骤](自动同步gitlab项目到github/push-to-a-remote-repository.gif)
1. 登陆gitlab，进入到项目，选择Settings > Repository，展开 `Push to a remote repository`。
2. 勾选 `Remote mirror repository`，每次有人推送时，都会自动更新远程镜像的分支、标签和提交。
3. 填写 `Git repository URL`，即你的github仓库地址，注意：
    - 必须可以通过http://，https://，ssh://或git://访问仓库。
    - 如果需要，请在URL中包含用户名:
`https://username:password@github.com/Tammeny/blog.git`，其中username和password分别为`github`的用户名和密码，注意username必须是用户名，不能是邮箱账号，否则会报错。
    - 更新操作将在10分钟后超时，对于大型存储库，请使用克隆/推送组合。
    - Git LFS对象将不会同步。
4. 测试一波，push变更到`gitlab`之后，`gitlab`会自动push代码到我们所配置的远程仓库中。