---
title: idea 通过跳板机 远程debug
categories:
  - 工具
tags:
  - idea
comments: false
abbrlink: 61110
date: 2019-10-18 00:00:00
---
**首先在 java 启动的时候加上参数:  指定一个端口 (以下为5066 )**
``` bash
Java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5066 -jar 
```
**然后通过远程建立ssh通道将本地的5066,代理到服务机的5066端口上:**
![fbebf4d4e38d675a257ba74843e226ad.png](/home/deploy/Pic/7B7A6C95-FDCE-41EA-81C8-2CBBA264688E.png)
```bash
ssh -l $JUMP_SERVER_USER -L 5066:$SERVER_IP:5066 -p $JUMP_SERVER_PORT $JUMP_SERVER_IP
```
* JUMP_SERVER_USER : 是跳板机账号
* SERVER_IP : 是服务器的ip，跳板机通过该ip可连接到服务器
* JUMP_SERVER_PORT : 是指跳板机的ssh端口
* JUMP_SERVER_IP : 是指跳板机的ip


**然后idea上配置远程debug后启动即可**

![fa01c4506935523fb577211b575963b8.png](/home/deploy/Pic/01C0C1C9-3B6C-401C-9791-77D90805D528.png)

**成功**
![64125b2a2bb457c6c51c64955dd45c5f.png](/home/deploy/Pic/E9755145-FA55-4D07-99B0-D785A224A5C6.png)
