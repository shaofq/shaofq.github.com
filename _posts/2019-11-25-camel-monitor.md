---
layout: post
title:  "camel监控jmx"
categories: camel
tags: camel
author: runningshao
---

* content
{:toc}

# 打开jolokia
最后一位是执行java程序的进程id

```
查看进程
java -jar jolokia-jvm-1.6.2-agent.jar list
进入进程
java -jar jolokia-jvm-1.6.2-agent.jar start 18315
```
# java的main方法执行，添加参数

```
-Dorg.apache.camel.jmx.mbeanObjectDomainName=oragpa
```
# 启动hawtio

```
java -jar hawtio-app-2.8.0.jar
```
# 在hawtio 添加remote conection