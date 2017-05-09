---
layout: post
title:  "docker for windows server "
date:   2017-05-09 10:00:00
categories: docker
---

> 官方文档: https://docs.microsoft.com/zh-cn/virtualization/windowscontainers/index

## 1 install
> 参考：http://fluentbytes.com/deploying-asp-net-4-5-to-docker-on-windows/
> 
```powershell
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Install-Module -Name DockerMsftProvider -Force
Install-Package -Name docker -ProviderName DockerMsftProvider -Force
Restart-Computer -Force
```

```powershell
# Open firewall port 2375
netsh advfirewall firewall add rule name="docker engine" dir=in action=allow protocol=TCP localport=2375

# Configure Docker daemon to listen on both pipe and TCP (replaces docker --register-service invocation above)
Stop-Service docker
dockerd --unregister-service
dockerd -H npipe:// -H 0.0.0.0:2375 --register-service
Start-Service docker
```

## 2 管理

* 输出容器IP
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" [musicstore_web_1]
```

##  3 config
> Windows configuration file  https://docs.docker.com/engine/reference/commandline/dockerd/#configuration-reloading
> or https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-docker/configure-docker-daemon

The default location of the configuration file on Windows is %programdata%\docker\config\daemon.json. The --config-file flag can be used to specify a non-default location.
This is a full example of the allowed configuration options on Windows:
```json
{
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"],
    "graph": "d:\\dockergraph",                               //镜像文件保存位置
    "registry-mirrors": ["https://***.mirror.aliyuncs.com"],  //阿里云代理 dev.aliyun.com
    "tlscacert": "C:\\ProgramData\\docker\\certs.d\\ca.pem",       //证书
    "tlscert": "C:\\ProgramData\\docker\\certs.d\\server-cert.pem",
    "tlskey": "C:\\ProgramData\\docker\\certs.d\\server-key.pem",
}
```

## 4 远程管理docker

## 4.1 非安全
> 参考 https://docs.microsoft.com/zh-cn/virtualization/windowscontainers/deploy-containers/deploy-containers-on-nano

在容器主机上为 Docker 连接创建防火墙规则。 这将是用于不安全连接的端口 2375，或用于安全连接的端口 2376。
```powershell
netsh advfirewall firewall add rule name="Docker daemon " dir=in action=allow protocol=TCP localport=2375

# 修改配置文件
new-item -Type File c:\ProgramData\docker\config\daemon.json
Add-Content 'c:\programdata\docker\config\daemon.json' '{ "hosts": ["tcp://0.0.0.0:2375", "npipe://"] }'

Restart-Service docker
```
在本地执行
```
docker -H tcp://<IPADDRESS>:2375 images
```
## 4.2 安全
> https://stefanscherer.github.io/protecting-a-windows-2016-docker-engine-with-tls/

```powershell
mkdir server  
mkdir client\.docker  

docker run --rm `
  -e SERVER_NAME=$(hostname) `
  -e IP_ADDRESSES=127.0.0.1,192.168.254.123 `
  -v "$(pwd)\server:C:\ProgramData\docker" `
  -v "$(pwd)\client\.docker:C:\Users\ContainerAdministrator\.docker" `
  stefanscherer/dockertls-windows

```

客户端
```
cp client\.docker\*  $env:USERPROFILE\.docker
docker -H tcp://*.*.*.*:23  --tlsverify images
```

服务端 edit c:\ProgramData\docker\config\daemon.json
```json
{    
    "graph": "e:\\dockergraph",
    "hosts": ["tcp://0.0.0.0:2376", "npipe://"],
    "registry-mirrors": ["https://46j6ibfx.mirror.aliyuncs.com"],
    "tlsverify": true,
    "tlscacert": "C:\\ProgramData\\docker\\certs.d\\ca.pem",
    "tlscert": "C:\\ProgramData\\docker\\certs.d\\server-cert.pem",
    "tlskey": "C:\\ProgramData\\docker\\certs.d\\server-key.pem"
}
```


##2.5 使用代理
```powershell
[Environment]::SetEnvironmentVariable("HTTP_PROXY", "http://username:password@proxy:port/", [EnvironmentVariableTarget]::Machine)
Restart-Service docker
```