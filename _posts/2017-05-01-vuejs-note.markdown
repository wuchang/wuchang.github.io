---
layout: post
title:  "vuejs笔记"
date:   2017-05-1 
categories: main
---

# 1. 环境配置

* npm 全局代理代理
```bash
npm config set proxy http://127.0.0.1:1081
npm config set https-proxy http://127.0.0.1:1081
```
* npm 临时代理
```bash
npm --proxy http://proxy-server:8080/ install
```

## 1.1 生成模板
```bash
npm install -g vue-cli
vue init webpack-simple my-project-name
cd my-project-name
npm install
npm run dev
```

访问  http://localhost:8080
