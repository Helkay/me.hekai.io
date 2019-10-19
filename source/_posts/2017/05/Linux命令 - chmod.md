---
title: Linux命令 - chmod
categories:
  - Blog
tags:
  - Linux
abbrlink: 64754
date: 2017-05-19 00:00:00
---

### 功能：
文件调用权限分为三级 : 文件拥有者、群组、其他。利用 chmod 可以藉以控制文件如何被他人所调用。

### 语法:

``` bash
chmod [-cfvR] [--help] [--version] mode file...

```

### 参数说明:

**mode:** 权限设定字串

``` bash 
[ugoa...][[+-=][rwxX]...][,...]

```

* u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
* + 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
* r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。

### 实例

``` bash 
#将文件 file1.txt 设为所有人皆可读取
chmod ugo+r file1.txt

#将目前目录下的所有文件与子目录皆设为任何人可读取 :
chmod -R a+r *

```

**此外chmod也可以用数字来表示权限如 :**

``` bash
chmod 777 file
```

语法为：

``` bash
chmod abc file
```

其中a,b,c各为一个数字，分别表示User、Group、及Other的权限。
r=4，w=2，x=1

***若用chmod 4755 filename可使此程序具有root的权限***