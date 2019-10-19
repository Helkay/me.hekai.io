---
title: mysql快速复制数据库
categories:
  - 技术笔记
tags:
  - Mysql
abbrlink: 30553
date: 2017-06-20 00:00:00
---

某些时候，为了搭建一个测试环境,需要复制一个已存在的MySQL数据库。

**创建表**

``` bash
CREATE DATABASE dbtest DEFAULT CHARACTER SET utf8 COLLATE UTF8_GENERAL_CI;
```

**复制数据库**

``` bash
mysqldump db -u root -ppassword --add-drop-table | mysql dbtest -u root -ppassword
```

-ppassword : password为密码 如果秘密中有特殊字符需要转译(字符前加上\)不然会报(mysqldump: Got error: 1045: Access denied for user 'root'@'localhost' (using password: YES) when trying to connect) 

**复制到远程另一台MySQL服务器上**

``` bash
mysqldump db -u root -ppassword --add-drop-table | mysql -h 192.168.1.2  dbtest -u root -ppassword
```
