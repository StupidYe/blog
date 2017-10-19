---
title: Docker安装步骤记录
date: 2017-09-21 00:21:42
tags: Docker
toc: true
---

### 0.前言

​	最近接触了Docker，需要学习Docker的相关知识，也拜读了一些书，现在记录下我在学习过程中对Docker安装的步骤和使用阿里云加速器的方法。

<!-- more -->

### 1.Docker安装前提

- ubuntu系统必须是1204或者更高版本。
- ubuntu系统必须是64位的。
- 系统运行的是Linux3.8或者跟高版本的内核

### 2.Docker安装

​	本次安装使用的是Ubuntu14.04,64位系统。

- 更新源

  `sudo apt-get update`

  更新源经常会出现问题，大多是源的问题。可以百度解决。

  建议自己装一个虚拟机，一般装好的直接update是没问题的。

- apt支持https

  `sudo apt-get install apt-transport-https`

- 将Docker库的公钥加入到本地apt中

  `sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80  --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9 ` 

- 再将安装源加入到apt源中，并更新和安装

  `sudo sh -c "echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list "`

  `sudo apt-get update`

  `sudo apt-get install lxc-docker`

### 3.验证安装是否成功

​	验证安装Docker是否成功，可以使用命令测试下：`sudo docker info`

​	有可能后出现一个错误：`Cannot connect to the Docker daemon. Is 'docker -d' running on this host? ` 

​	解决方法：启动docker服务 `sudo service docker start`

​	再使用docker info测试，如下：

```
Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 0
Server Version: 17.05.0-ce
Storage Driver: aufs
 Root Dir: /var/lib/docker/aufs
 Backing Filesystem: extfs
 Dirs: 0
 Dirperm1 Supported: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
......
......
```

​	说明安装成功。

### 4.使用阿里云加速器

​	使用docker下载镜像时候，国内的速度太慢了....

​	推荐使用[阿里云加速器](https://cr.console.aliyun.com/#/accelerator)。

​	登录账号（没有就自己注册一个）

​	然后根据网页下的做法，复制命令到Ubuntu上执行更新docker版本。

​	完成后，再用docker info验证，可能会出现一个错误，具体报错为客户端的API版本高于服务端

​	`Error response from daemon: client and server don’t have same version` 

​	解决方法：重启Ubuntu。

​	之后下载镜像速度会得到飞跃的提高~~

​	

