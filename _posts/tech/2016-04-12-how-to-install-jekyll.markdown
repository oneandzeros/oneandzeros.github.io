---
layout: post
title:  "安装jekyll准备环境"
date:   2016-04-12 16:43:26
categories: 技术
---
参考 [安装jekyll准备](http://www.cnblogs.com/ee2213/p/3915243.html?utm_source=tuicool&utm_medium=referral)|[nodejs下载](https://nodejs.org/en/)




### 问题1 i386sudo
在准备安装ruby时 一直报错，提示apt-get 命令有问题

然后我执行了一下sudo apt-get update 确实出现了如下问题

	忽略 http://mirrors.yun-idc.com trusty InRelease
	命中 http://mirrors.yun-idc.com trusty-updates InRelease
	命中 http://mirrors.yun-idc.com trusty-backports InRelease
	命中 http://mirrors.yun-idc.com trusty-security InRelease
	命中 http://mirrors.yun-idc.com trusty Release.gpg
	命中 http://mirrors.yun-idc.com trusty-updates/main amd64 Packages
	忽略 http://dl.google.com stable InRelease
	命中 http://mirrors.yun-idc.com trusty-updates/restricted amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-updates/universe amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-updates/main i386 Packages
	命中 http://mirrors.yun-idc.com trusty-updates/restricted i386 Packages
	命中 http://mirrors.yun-idc.com trusty-updates/universe i386 Packages
	命中 http://mirrors.yun-idc.com trusty-backports/main amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-backports/restricted amd64 Packages
	命中 http://dl.google.com stable Release.gpg
	命中 http://mirrors.yun-idc.com trusty-backports/universe amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-backports/main i386 Packages
	命中 http://mirrors.yun-idc.com trusty-backports/restricted i386 Packages
	命中 http://mirrors.yun-idc.com trusty-backports/universe i386 Packages
	命中 http://mirrors.yun-idc.com trusty-security/main amd64 Packages
	命中 http://dl.google.com stable Release
	命中 http://mirrors.yun-idc.com trusty-security/restricted amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-security/universe amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-security/main i386 Packages
	命中 http://mirrors.yun-idc.com trusty-security/restricted i386 Packages
	命中 http://dl.google.com stable/main amd64 Packages
	命中 http://mirrors.yun-idc.com trusty-security/universe i386 Packages
	命中 http://mirrors.yun-idc.com trusty Release
	命中 http://mirrors.yun-idc.com trusty/main amd64 Packages
	命中 http://mirrors.yun-idc.com trusty/restricted amd64 Packages
	命中 http://mirrors.yun-idc.com trusty/universe amd64 Packages
	命中 http://mirrors.yun-idc.com trusty/main i386 Packages
	命中 http://mirrors.yun-idc.com trusty/restricted i386 Packages
	命中 http://mirrors.yun-idc.com trusty/universe i386 Packages
	W: 无法下载 http://mirrors.yun-idc.com/ubuntu/dists/trusty-updates/InRelease Unable to find expected entry 'main/binary-i386sudo/Packages' in Release file (Wrong sources.list entry or malformed file)

	W: 无法下载 http://mirrors.yun-idc.com/ubuntu/dists/trusty-backports/InRelease Unable to find expected entry 'main/binary-i386sudo/Packages' in Release file (Wrong sources.list entry or malformed file)

	W: 无法下载 http://mirrors.yun-idc.com/ubuntu/dists/trusty-security/InRelease Unable to find expected entry 'main/binary-i386sudo/Packages' in Release file (Wrong sources.list entry or malformed file)

	W: 无法下载 http://mirrors.yun-idc.com/ubuntu/dists/trusty/Release Unable to find expected entry 'main/binary-i386sudo/Packages' in Release file (Wrong sources.list entry or malformed file)

	W: 无法下载 http://dl.google.com/linux/deb/dists/stable/Release Unable to find expected entry 'main/binary-i386/Packages' in Release file (Wrong sources.list entry or malformed file)

	E: Some index files failed to download. They have been ignored, or old ones used instead.

解决这个问题的过程很艰辛，下面只说最后结果：

1. 去掉软件和更新中的勾选：有版权和合法性问题的软件
2. 在/etc/apt/目录下找到更新源文件，注释掉第三方的源，类似上面“http://dl.google.com/..”的源。
3. 系统架构中删除i386sudo

	dpkg --print-foreign-architectures   
	dpkg --remove-architecture i386sudo

删除sudo 后重新执行命令 sudo apt-get update  成功！

------

### 问题2 rvm is not a function

安装jekyll时会提示rvm is not a function
参考 https://ruby-china.org/topics/3705
https://rvm.io/integration/gnome-terminal
在terminal中勾选“以登陆shell方式允许命令”后重启terminal就可以继续了。
