---
title: mysql ip授权
categories:
  - Blog
tags:
  - Mysql
abbrlink: 43093
date: 2017-05-27 00:00:00
---

``` bash
mysql>GRANT ALL PRIVILEGES ON *.* TO 'username'@'127.0.0.1' IDENTIFIED BY 'password' WITH GRANT OPTION;

mysql>FLUSH PRIVILEGES;
```

**grant语法:**

grant 权限名（所有的权限用all） on 库名（*全部）.表名（*全部） to ‘要授权的用户名’@’%’(%表示所有的IP，可以只些一个IP） identified by “密码”；