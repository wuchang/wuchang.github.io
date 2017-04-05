---
layout: post
title:  "windows配置gitpage环境"
date:   2017-03-31 12:50:00
categories: main
---

1. 下载安装ruby与DEVELOPMENT KIT,  http://rubyinstaller.org/downloads/
初始化RubyDevKit
```powershell
cd C:\RubyDevKit
ruby dk.rb init
ruby dk.rb install
```
2. 安装jekyll
```
gem install jekyll
gem install bundler
gem install pygments.rb -v '1.1.1'
gem install wdm
bundle install
jekyll s --watch
```
> 默认4000端口被占用，停止FxService服务，或者换端口