---
title: Git命令
categories:
  - Blog
tags:
  - Git
abbrlink: 19993
date: 2017-05-12 00:00:00
---

## Git user.name uesremail设置

``` bash
#全局
git config --global user.name  "your_name"
git config --global user.emial "your_email"

#局部
git config user.name  "your_name"
git config user.emial "your_email"

#取消
git config --unset --global user.name
git config --unset --global user.email       

```