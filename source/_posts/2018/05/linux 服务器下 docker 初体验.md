---
title: linux 服务器下 docker 初体验
categories:
  - 技术笔记
tags:
  - Docker
comments: false
abbrlink: 61110
date: 2018-05-10 00:00:00
---

系统版本:** CentOS-7-x86_64-Minimal-1804


## 安装docker

使用 `yum install docker` 命令安装docker

使用非centos 7的版本可能会报以下错误
 `No package docker available`

没有找到docker包,需要第三方软件库`epel`,使用一下以下命令安装

```bash
☁  ~  sudo yum install epel-release
```

然后再安装

```bash
☁  ~  sudo yum install docker-io
```

## 修改国内docker加速配置

```bash
☁  ~ vim /etc/docker/daemon.json
```
修改 `"registry-mirrors"` 为相应的国内加速地址

修改好配置文件后，重新加载并启动

```bash
☁  ~ systemctl daemon-reload
☁  ~ systemctl restart docker
```

## 以下我会以在docker中使用jenkins为例,列举docker的一些基本使用方法

* 下载jenkins镜像

```bash
☁  ~  docker pull jenkins
```

* 创建jenkins文件夹 用来做挂载磁盘

```bash
☁  ~  mkdir /home/hiko/jenkins
```

***注意：在安装jenkins时候，挂在文件夹/home/hzq/jenkins/的归属用户id必须是1000，否则会抛出无操作权限异常。***

* 查看文件夹属性

```bash
☁  ~  ls -nd /home/hiko/jenkins/
```

* 修改文件夹 `归属者` 和 `组`

```bash
☁  ~  chown -R 1000:1000 jenkins/
```

* 启动jenkins 

```bash
☁  ~  docker run -itd -p 8080:8080 -p 50000:50000 --name jenkins --privileged=true  -v /home/hiko/jenkins:/var/jenkins_home jenkins
```

`-p 8080:8080 -p 50000:50000` : 映射端口

`--privileged=true` : 在CentOS7中的安全模块selinux把权限禁掉了，参数给容器加权。

`-v /home/hiko/jenkins:/var/jenkins_home` : 磁盘挂载


## 查看containers储存地址

```bash
☁  ~  cd /var/lib/docker/containers/ {container_ID}
☁  ~  vi config.v2.json
```

自定义挂载:

```json
{
    "MountPoints": {
        "/var/jenkins_home": {
            "Source": "/home/hiko/jenkins",
            "Destination": "/var/jenkins_home",
            "RW": true,
            "Name": "",
            "Driver": "",
            "Type": "bind",
            "Propagation": "rprivate",
            "Spec": {
                "Type": "bind",
                "Source": "/home/hiko/jenkins",
                "Target": "/var/jenkins_home"
            }
        }
    }
}
```

如果不挂载磁盘,则默认启动后地址:

```json
{
    "MountPoints": {
        "/var/jenkins_home": {
            "Source": "",
            "Destination": "/var/jenkins_home",
            "RW": true,
            "Name": "d48f1035be1c76267a404c4fea29ef2f709bdff0ddf0736356cbd4897c7bc87b",
            "Driver": "local",
            "Type": "volume",
            "Spec": {}
        }
    }
}
```
```bash
☁  ~  cd /var/lib/docker/volumes
```


## docker进入容器的几种办法

1.  **docker attach**
    使用 `docker attach` 想要进入命令行界面,有个前提是这个容器必须是用 `/bin/bash` 创建的。
    **参考资料:**  [difference between docker attach and docker exec
](https://stackoverflow.com/questions/30960686/difference-between-docker-attach-and-docker-exec)

2. **SSH**

3. **nsenter**

 [github链接](https://github.com/jpetazzo/nsenter)


4. **docker exec**

```bash
☁  ~  docker exec -it 775c7c9ee1e1 /bin/bash
```

##docker rm命令-删除一个或多个容器

* 显示所有的容器，过滤出Exited状态的容器，取出这些容器的ID，

```bash
☁  ~  docker ps -a | grep Exited|awk '{print $1}'
```

* 查询所有的容器，过滤出Exited状态的容器，列出容器ID，删除这些容器

```bash
☁  ~  docker rm `docker ps -a | grep Exited | awk '{print $1}'`
```


