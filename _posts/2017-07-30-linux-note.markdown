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

# 2. pip使用socks5代理安装

```bash
apt-get install proxychains 
vi /etc/proxychains.conf # 加入类似内容：socks5 127.0.0.1 9051 
ssh -D 1080 xxxx.com 
proxychains pip install django
```