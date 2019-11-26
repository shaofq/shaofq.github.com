---
layout: post
title:  "解决javacpu高问题"
categories: Mysql
tags: Mysql
author: runningshao
---

* content
{:toc}

# 查看cpu高进程

```
top
```
根据这种故障的一般处理思路，先找出问题进程内CPU占用率高的线程，再通过线程栈信息找出该线程当时在运行的问题代码段
根据思路查看高占用的“进程中”占用高的“线程”，追踪发现7163的进程中16298的线程占用较高，使用命令

```
top -Hbp 21988 | awk '/java/ && $9>50'
```