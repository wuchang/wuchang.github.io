---
layout: post
title:  "Sentry docker-compose.yml"
date:   2017-05-15 10:00:00
categories: docker, sentry
---

# Sentry docker-compose.yml

1. 在sentry目录添加 docker-compose.yml 文件，文件内容在最下方
2. 替换 '!!!SECRET!!!' 为32位随机字符串
1. 执行 ```docker-compose up -d```
1. 执行 ```docker exec -it sentry_sentry_1 sentry upgrade``` ，创建数据库和初始化数据
1. 如果需要slack插件就执行 ```docker exec -it pip install sentry-slack ``` ，否则忽略
1. slack ```docker restart sentry_sentry_1```
1. 默认映射端口是9000，如有冲突请修改


# docker-compose.yml

```yml
version: '2'

volumes:
   pgdb:

services:
  redis:
    image: redis

  postgres:
    image: postgres:alpine
    environment:
      POSTGRES_USER: sentry
      POSTGRES_PASSWORD: sentry
      POSTGRES_DBNAME: sentry
      POSTGRES_DBUSER: sentry
      POSTGRES_DBPASS: sentry
    volumes:
     - pgdb:/var/lib/postgresql/data

  sentry:
    image: sentry
    links:
     - redis
     - postgres
    ports:
     - 9000:9000
    environment:
      SENTRY_SECRET_KEY: '!!!SECRET!!!'
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis

  cron:
    image: sentry
    links:
     - redis
     - postgres
    command: "sentry run cron"
    environment:
      SENTRY_SECRET_KEY: '!!!SECRET!!!'
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis

  worker:
    image: sentry
    links:
     - redis
     - postgres
    command: "sentry run worker"
    environment:
      SENTRY_SECRET_KEY: '!!!SECRET!!!'
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis
```