---
title: mysql数据库无法插入中文
categories:
  - Blog
tags:
  - Mysql
abbrlink: 31856
date: 2017-03-10 00:00:00
---

MySQL中，数据库的编码是一个相当重要的问题，有时候我们需要查看一下当前数据库的编码，甚至需要修改一下数据库编码。

修改my.cnf

```
[mysqld]
character-set-server=utf8

[client]
default-character-set=utf8
```

####查看数据库信息
```
mysql > show variables;

mysql > set names 'gbk';
```

```
'character_set_connection', 'gbk'
'character_set_database', 'utf8'
'character_set_filesystem', 'binary'
'character_set_results', 'gbk'
'character_set_server', 'utf8'
'character_set_system', 'utf8'
```



修改完毕后重建表加上 character set utf8;
