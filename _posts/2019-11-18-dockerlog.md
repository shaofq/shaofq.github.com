---
layout: post
title:  "docker中应用环境建立(tomcat+redis+dubbo)"
categories: skill
tags: 效率 vscode markdown
author: runningshao
---

* content
{:toc}

# 应用服务器配置说明

![image.png](https://i.loli.net/2019/11/18/TPv8f4shLyrRDot.png)

# docker部署说明
*  安装docker，依次执行相应的命令
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
```
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
```
sudo yum makecache fast
sudo yum -y install docker-ce
sudo systemctl start docker
systemctl enable docker
systemctl restart docker
```
*  初始化swarm，建立swarm集群
```
docker swarm init
```
创建完swarm后，控制台显示对应worker添加命令，执行相应命令添加worker节点
*  安装docker compose

*  安装portainer,开放服务器的远程docker api实现portianer集中管理
*  在portainer中编排安装服务