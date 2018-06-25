---
layout: post
title:  "saltstack笔记"
date:   2018-05-21 10:00:00
categories: salt, 运维


#saltstack笔记

# 1. master

## 1.1 安装 

* 方法一 Install the Stable Version from the Official PPA

```bash
sudo apt-get install python-software-properties
sudo apt-get update
sudo add-apt-repository ppa:saltstack/salt 
sudo apt-get update
sudo apt-get install salt-master salt-minion salt-ssh salt-cloud salt-doc
```

* 方法二 Install the Stable Version Using Salt-Bootstrap

```bash
curl -L https://bootstrap.saltstack.com -o install_salt.sh
sudo sh install_salt.sh -P -M
```

* 方法二 Install the Development Version Using Salt-Bootstrap

```bash
curl -L https://bootstrap.saltstack.com -o install_salt.sh
sudo sh install_salt.sh -P -M git develop
```


## 1.2 master配置

默认配置文件位置： ```/etc/salt/master```


## 1.3 run

```
salt-master --log-level=debug
```

# 2. minion

## 2.1 ubuntu安装 

```bash
sudo apt-get install python-software-properties
sudo apt-get update
sudo add-apt-repository ppa:saltstack/salt 
sudo apt-get update
sudo apt-get install salt-minion 
```

## 2.2 windows 安装

下载安装包 https://repo.saltstack.com/#windows
```ps
Salt-Minion-2018.3.0-Py3-AMD64-Setup.exe /S /master=yoursaltmaster /minion-name=yourminionname
```

## 配置

```
#指定master，冒号后有一个空格
master: 192.168.2.22
id: minion-01
user: root

#-------以下为可选--------------
# master通讯端口
master_port: 4506
# 备份模式，minion是本地备份，当进行文件管理时的文件备份模式
backup_mode: minion
# 执行salt-call时候的输出方式
output: nested 
# minion等待master接受认证的时间
acceptance_wait_time: 10
# 失败重连次数，0表示无限次，非零会不断尝试到设置值后停止尝试
acceptance_wait_time_max: 0
# 重新认证延迟时间，可以避免因为master的key改变导致minion需要重新认证的syn风暴
random_reauth_delay: 60
# 日志文件位置
log_file: /var/logs/salt_minion.log
# 文件路径基本位置
file_roots:
  base:
    - /etc/salt/minion/file
# pillar基本位置
pillar_roots:
  base:
    - /data/salt/minion/pillar

```

