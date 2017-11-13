---
layout: post
title:  "Windows Subsystem for Linux使用笔记"
date:   2017-11-13 10:00:00
categories: main
---

# 1. apt使用代理

## 临时代理
```bash
export http_proxy=http://yourproxyaddress:proxyport
```


## 全局代理

```sudo vi /etc/apt/apt.conf```
```bash
Acquire::http::proxy "http://127.0.0.1:1080/";
Acquire::ftp::proxy "ftp://127.0.0.1:1080/";
Acquire::https::proxy "https://127.0.0.1:1080/";
```

## BASH rc method
```sudo vi ~/.bashrc```
```bash
http_proxy=http://yourproxyaddress:proxyport
export http_proxy
```

```source ~/.bashrc```

## 需要登录的代理
```http_proxy=http://username:password@yourproxyaddress:proxyport```
