---
layout: post
title:  "mysql引擎修改"
categories: Mysql
tags: Mysql
author: runningshao
---

* content
{:toc}

# mysql引擎修改

```
SELECT CONCAT('ALTER TABLE ',table_schema,'.',table_name,' ENGINE=InnoDB;') 
FROM information_schema.tables
WHERE table_schema='mysql' AND ENGINE='MyISAM';
```



