---
title: 安装Centos minimal及一些服务设置
categories:
  - 技术笔记
tags:
  - Linux
comments: false
abbrlink: 61110
date: 2018-04-05
---

**系统版本：** CentOS-6.8-x86_64-minimal


#### 因为装的是minimal版本,所以系统装好后,并连不上网络需要设置一下如下:

```bash
☁  ~  cd /etc/sysconfig/network-scripts/
☁  ~  vi ifcfg-eth0
```

将ONBOOT=no改为了yes

然后重启网络服务

```bash
☁  ~  service network restart
```

#### 查看是否安装ssh

```bash
☁  ~  yum list installed | grep openssh
```

如没有安装 则使用以下命令:

```bash
☁  ~  yum install openssh
```

#### 新建用户

```bash
☁  ~  useradd -d /home/hiko hiko
☁  ~  passwd hiko
```

若新建用户无ssh权限

```bash
vim /etc/ssh/sshd_config 
```

添加 `AllowUsers:hiko`

**赋予root权限**
方法一：修改 `/etc/sudoers` 文件,找到以下两行,去掉第二行的 `#`去掉

```bash
##Allows people in group wheel to run all commands
%wheel            ALL=(ALL)           ALL
```
然后修改用户，使其属于`root`组（wheel），命令如下:

```bash
☁  ~  usermod -g root hiko
```

修改完后，使用hiko帐号登录，命令` su - `，即可获得root权限。

方法二：修改 `/etc/sudoers` 文件,找到以下内容在,root下面一行添加 hiko用户配置:

```bash
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
hiko    ALL=(ALL)       ALL
```

修改完后，使用hiko帐号登录，命令` su - `，即可获得root权限。


#### ssh登录免密

使用ssh-keygen命令创建密钥对
假设 我现在需要在A机(192.168.1.1)上免密ssh登录到B(192.168.1.2)机

在A机用户路径中的.ssh文件夹内创建密钥对

```bash
☁  ~  ssh-keygen -t rsa -f id_rsa.xxx  -P ''
```

- ***-f 命名生成的文件名称***
- ***-P ‘’ 无密码 （不加则需要输入三次回车）***

这时候.ssh文件夹下会生成两个文件(id_rsa.xxx id_rsa.xxx.pub)

将id_rsa.xxx.pub 用scp命令传输的B上

```bash
☁  ~  scp id_rsa.xxx.pub root@192.168.1.2:/(B机上.ssh的路径)
```

将id_rsa.xxx.pub里的内容写入到 .ssh中的authorized_keys文件中(没有则创建),并修改authorized_keys文件和.ssh文件夹的权限

```bash
☁  ~  cat id_rsa.xxx.pub>>authorized_keys
☁  ~  chmod 600 authorized_keys
☁  ~  chmod 700 -R .ssh
```




