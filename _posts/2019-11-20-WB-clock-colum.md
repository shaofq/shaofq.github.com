---
layout: post
title:  "WB列锁定问题"
categories: WB
tags: WB
author: runningshao
---

* content
{:toc}

# WB列锁定问题

WB列锁定后，如果列的总数量比较大，在动态拖拽列宽度后，会产生列消失问题，解决办法是设置列的resizeable为false,即设置不能动态改变固定列的宽度


