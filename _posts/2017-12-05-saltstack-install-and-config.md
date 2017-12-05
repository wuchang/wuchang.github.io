---
layout: post
title:  "saltstack 安装配置"
date:   2017-12-25 10:00:00
categories: 运维, linux
---


#2. 附加

## 2.1 Salt-Minion改名 

1. 停止客户端 Salt-Minion Service ```service salt-minion stop```
2. 删除客户端当前KEYS (```rm /etc/salt/pki/minion/minion.pub and minion.pem```
3. 在客户端用记事本打开 ```/etc/salt/minion_d``` 修改ID
4. 服务端，删除对应客户端的KEY ```Salt-key –delete <key>```
5. 重启客户端服务Salt minion service ```service salt-minion start```
6. 在服务端重新接受客户端的KEY ```salt-key –accept-all (or key)```