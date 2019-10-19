---
title: ssh登录免密
categories:
  - 技术笔记
tags:
  - Linux
abbrlink: 30185
date: 2017-05-10 00:00:00
---

### 使用ssh-keygen命令创建密钥对

假设 我现在需要在A机(192.168.1.1)上免密ssh登录到B(192.168.1.2)机

在A机用户路径中的.ssh文件夹内创建密钥对

``` bash
ssh-keygen -t rsa -f id_rsa.xxx  -P ''
```

* -f 命名生成的文件名称
* -P ''  无密码 （不加则需要输入三次回车）

这时候.ssh文件夹下会生成两个文件(id_rsa.xxx  id_rsa.xxx.pub)

将id_rsa.xxx.pub 用scp命令传输的B上

``` bash
scp id_rsa.xxx.pub root@192.168.1.2:/(B机上.ssh的路径)
```

将id_rsa.xxx.pub里的内容写入到 .ssh中的authorized_keys文件中(没有则创建),并修改authorized_keys文件和.ssh文件夹的权限

``` bash
cat id_rsa.xxx.pub>>authorized_keys

chmod 600 authorized_keys

chmod 700 -R .ssh   
```

