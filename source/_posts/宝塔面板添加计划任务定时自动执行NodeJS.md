---
title: 宝塔面板添加计划任务定时自动执行NodeJS
categories: 自动化
toc: true
date: 2019-03-16 11:55:33
tags: 服务器
---

## 介绍
有时我们需要在服务器上自动执行nodejs程序，如定时爬取某个网站的数据。在服务器已经装有宝塔面板的前提下，使用计划任务无疑是最方便快捷的，但是第一次使用此功能还是遇到了一些问题，这篇文章将此设置过程记录下来，方便自己或广大网友排坑。

## 环境
- CentOS 6.8
- 宝塔面板免费版 5.9.1
- Node v10.15.3

## 过程
1. 服务器先安装`node`，或者直接通过宝塔面板里的“软件管理”安装`pm2`；
2. 进入“计划任务”，选择shell脚本并设定任务名称、执行周期，最后填写脚本内容如`node /www/wwwroot/spider/index.js`并保存。

## 问题与解决方案
但执行后，通过日志看到提示`node: command not found`，说明脚本并没有成功执行，编辑刚刚创建的脚本，在`export PATH`之前加一行`. ~/.bashrc`，保存即可。
> bash 是一个能解释你输入进终端程序的东西，并且基于你的输入来运行命令。

## 完整的脚本
```shell
#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
. ~/.bashrc
export PATH
node /www/wwwroot/spider/index.js
echo "----------------------------------------------------------------------------"
endDate=`date +"%Y-%m-%d %H:%M:%S"`
echo "★[$endDate] Successful"
echo "----------------------------------------------------------------------------"
```
