---
title: nginx session保持
categories:
  - Blog
tags:
  - Nginx
abbrlink: 44626
date: 2017-04-18 00:00:00
---


今天在使用JCaptcha做验证码功能的时候,在本地是可以正常验证的,可是发布在服务器上的时候验证码功能就失效了,于是打上日志,看输出信息,发现验证码生成的时候从session里取的id跟校验的时候从session的id不同,发生了改变.

由于我的服务器上用了nginx,去网上查,很多都是讲的是负载均衡导致的session不共享,需配置

```
upstream cms{
	server 127.0.0.1:8080;
	ip_hash;
}

```

但是由于我只用了一个服务,再查找了一下资料,发现主要是cookie路径的转换问题,如果只是host、端口转换，则cookie不会丢失,如果路径也变化了，则需要设置cookie的路径转换，nginx.conf的配置如下

```
location /proxy_path {
        proxy_pass   http://127.0.0.1:8080/project;
        proxy_cookie_path  /project /proxy_path;
}
```