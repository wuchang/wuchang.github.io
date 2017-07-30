---
layout: post
title:  "linux系统笔记"
date:   2017-07-30 10:00:00
categories: main
---

# 1. apt 使用代理

```sudo vi /etc/apt/apt.conf```，添加

```
Acquire::http::Proxy "http://127.0.0.1:1080";
Acquire::https::Proxy "https://127.0.0.1:1080";
```
