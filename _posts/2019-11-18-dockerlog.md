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
docker默认是没有开启Remote API的，需要我们手动开启:
编辑/lib/systemd/system/docker.service文件：
找到下面行，修改为：
ExecStart=/usr/bin/dockerd -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375
重启服务：
sudo systemctl daemon-reload
sudo service docker restart
portainer安装：
```
docker pull portainer/portainer
docker volume create portainer_data
docker run -d -p 9000:9000 --name portainer --restart always  -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data -v /public:/home/public --name prtainer portainer/portainer
```
*  在portainer的stack中编排安装服务
这里在编排的时候指定了overlay网络，实际上是把所有服务设置在同一个局域网内，实现跨主机跨容器访问，程序访问的配置和数据库配置指向的ip为容器的名称

```
version: '3.1'
services:
  hhwebredis:
    image: redis
    container_name: hhredis
    command: redis-server /etc/redis.conf
    volumes:
      - /home/redis/data:/data
      - /home/redis/conf/redis.conf:/etc/redis.conf
    ports:
      - 6379:6379 
    networks:
      - my-network
  #网站系统tomcat
  hhwebsite:
    image: runningshao/tomcat:latest
    volumes:
      - /home/website:/usr/local/tomcat/webapps
    ports:
      - 80:8080 
    depends_on:
      - hhwebredis
    network_mode: "host"
    environment:
      CATALINA_OPTS: -Djava.security.egd=file:/dev/./urandom
  #业务系统tomcat    
  hhwebbiz:
    image: runningshao/tomcat:latest
    volumes:
      - /home/biz/web:/usr/local/tomcat/webapps
      - /home/biz/logs:/usr/local/tomcat/logs
    ports:
      - 83:8080 
    depends_on:
      - hhwebredis
    networks:
      - my-network
    environment:
      CATALINA_OPTS: -Djava.security.egd=file:/dev/./urandom
  #edi业务处理    
  hhedibiz:
    image: runningshao/tomcat:latest
    volumes:
      - /home/biz/web:/usr/local/tomcat/webapps
      - /home/biz/logs:/usr/local/tomcat/logs
    ports:
      - 8787:8080 
    depends_on:
      - hhwebredis
    networks:
      - my-network
    environment:
      CATALINA_OPTS: -Djava.security.egd=file:/dev/./urandom
  #tomcat自动处理    
  hhautobiz:
    image: runningshao/tomcat:latest
    volumes:
      - /home/biz/web:/usr/local/tomcat/webapps
      - /home/biz/logs:/usr/local/tomcat/logs
    ports:
      - 8989:8080 
    depends_on:
      - hhwebredis
    networks:
      - my-network
    environment:
      CATALINA_OPTS: -Djava.security.egd=file:/dev/./urandom     
 #tomcat报表处理    
  hhreport:
    image: runningshao/tomcat:latest
    volumes:
      - /home/biz/web:/usr/local/tomcat/webapps
      - /home/biz/logs:/usr/local/tomcat/logs
    ports:
      - 8989:8080 
    depends_on:
      - hhwebredis
    networks:
      - my-network
    environment:
      CATALINA_OPTS: -Djava.security.egd=file:/dev/./urandom  
networks:
  my-network:
    driver: overlay
```

创建zookeeper ,分别在三台服务器执行

```
version: '3.1'
services:
    zoo2:
        image: zookeeper
        restart: always
        environment:
        #不同服务器的zoo_my_id不同
            ZOO_MY_ID: 1
            ZOO_SERVERS: server.1=172.16.36.123:2888:3888 server.2=172.16.36.124:2888:3888 server.3=172.16.36.125:2888:3888
        network_mode: host
```
