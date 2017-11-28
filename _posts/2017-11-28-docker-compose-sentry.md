---
layout: post
title:  "docker-compose sentry"
date:   2017-11-28 10:00:00
categories: main
---

## docker-compose.yml
```yml
version: '2'

services:
  redis:
    image: redis

  postgres:
    image: postgres
    environment:
      POSTGRES_USER: sentry
      POSTGRES_PASSWORD: sentry
      POSTGRES_DBNAME: sentry
      POSTGRES_DBUSER: sentry
      POSTGRES_DBPASS: sentry
    volumes:
     - .data/pgdb:/var/lib/postgresql/data

  sentry:
    image: sentry
    links:
     - redis
     - postgres
    ports:
     - 80:9000
    environment:
      SENTRY_SECRET_KEY: '=^#22%-gy7ls6b57exl+vr!fv*1d!b)dijkzn=ad_lahxfxx^t'
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
      SENTRY_SECRET_KEY: '=^#22%-gy7ls6b57exl+vr!fv*1d!b)dijkzn=ad_lahxfxx^t'
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
      SENTRY_SECRET_KEY: '=^#22%-gy7ls6b57exl+vr!fv*1d!b)dijkzn=ad_lahxfxx^t'
      SENTRY_POSTGRES_HOST: postgres
      SENTRY_DB_USER: sentry
      SENTRY_DB_PASSWORD: sentry
      SENTRY_REDIS_HOST: redis
```

## 步骤
1. 初始化 ```docker-compose run sentry sentry upgrade```
2. 执行 ```docker-compose up -d ```