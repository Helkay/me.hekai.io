---
title: 分支回退
categories:
  - 技术笔记
tags:
  - Git
abbrlink: 50098
date: 2016-12-06 00:00:00
---

### 本地分支回退

查看本地版本信息

```
git reflog
```
回退版本

```
git reset --hard 版本号
```

### 远程分支回退
远程分支回退同本地回退,只需在回退版本操作后推送到远程分支

```
git push -f
```

### 删除远程分支

```
git push origin --delete <branch name>
```