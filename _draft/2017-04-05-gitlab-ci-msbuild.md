---
layout: post
title:  "windows配置gitpage环境"
date:   2017-04-05 12:50:00
categories: main
---

## 1. 准备工作 
* 安装 build tools 2015 
```powershell
Invoke-WebRequest "https://download.microsoft.com/download/E/E/D/EEDF18A8-4AED-4CE0-BEBE-70A83094FC5A/BuildTools_Full.exe" -OutFile "$env:TEMP\BuildTools_Full.exe" -UseBasicParsing
& &  "$env:TEMP\BuildTools_Full.exe" /Silent /Full
```

* 下载配置nuget 
```powershell
Invoke-WebRequest "https://dist.nuget.org/win-x86-commandline/latest/nuget.exe" -OutFile "C:\windows\nuget.exe" -UseBasicParsing
```
如果项目有用到非官方渠道的包，则需要此项配置，配置文件位于```%AppData%\Roaming\NuGet```，或者是方案目录```.nuget\NuGet.Config```
```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
    <add key="xx" value="https://xxx/" protocolVersion="3" />
  </packageSources>
</configuration>
```


* 下载gitlab runner并改名为```gitlab-runner.exe```  https://docs.gitlab.com/runner/install/windows.html
* 安装 powershell msbuild 模块 ```Install-Module -Name Invoke-MsBuild```

## 2. 配置 .gitlab-ci.yml