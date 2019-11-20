---
layout: post
title:  "linux系统文件同步工具 lsyncd "
categories: skill
tags: 效率 vscode markdown
author: runningshao
---

* content
{:toc}

### 安装lsyncd
yum -y install lsyncd

### 设置服务之间免密登录
在Server A上执行

```
#ssh-keygen -t rsa
#cat  ~/.ssh/id_rsa.pub | ssh root@172.16.36.123 "cat - >> ~/.ssh/authorized_keys"
```

在Server B上执行

```
#ssh-keygen -t rsa
#cat  ~/.ssh/id_rsa.pub | ssh root@10.10.200.172 "cat - >> ~/.ssh/authorized_keys"
```

### 配置lsyncd 
建立lsyncd 主配置文件，假设放置在/etc/lsyncd.conf:

```
settings {
    nodaemon = false,
    logfile = "/var/log/lsyncd.log",
    statusFile = "/var/log/lsyncd.status",
    inotifyMode = "CloseWrite",
    maxProcesses = 8
}

-- 可以有多个sync，各自的source，各自的target，各自的模式，互不影响。
sync {
    default.rsyncssh,
    source    = "/home/wwwroot/web1/",
    host      = "111.222.333.444",
    targetdir = "/home/wwwroot/web1/",
    -- 忽略文件路径规则，可用table也可用外部配置文件
    -- excludeFrom = "/etc/lsyncd_exclude.lst",
    exclude = {
        ".svn",
        "Runtime/**",
        "Uploads/**",
    },
    -- maxDelays = 5,
    delay = 0,
    -- init = false,
    rsync = {
        binary = "/usr/bin/rsync",
        archive = true,
        compress = true,
        verbose = true,
        _extra = {"--bwlimit=2000"},
    },
}
```
### 启动lsyncd
lsyncd -log Exec /etc/lsyncd.conf
