---
layout: post
title: Ubuntu安装Java
category: 技术
tags: ubunt java
keywords: ubunt java
---

最近在虚机上新安了一个ubuntu，准备装jenkins。
习惯了在ubuntu上使用apt-get命令，就直接敲了个* sudo apt-get install java * 结果不管用啊！

网上搜了搜，发现这里有坑。

### 不好使的命令

```
//添加java的ppa
sudo add-apt-repository ppa:webupd8team/java
//更新软件源
sudo apt-get update
//安装java8
sudo apt-get install oracle-java8-installer
```
错误来源[ubuntu下搭建Jenkins 并配置部署环境](https://www.cnblogs.com/shuoer/p/9471839.html)

问题：
上面命令中的PPA源有问题webupd8team
```
nmtech@nmtech-virtual-machine:~$ sudo add-apt-repository ppa:webupd8team/java
 The Oracle JDK License has changed for releases starting April 16, 2019.

The new Oracle Technology Network License Agreement for Oracle Java SE is substantially different from prior Oracle JDK licenses. The new license permits certain uses, such as personal use and development use, at no cost -- but other uses authorized under prior Oracle JDK licenses may no longer be available. Please review the terms carefully before downloading and using this product. An FAQ is available here: https://www.oracle.com/technetwork/java/javase/overview/oracle-jdk-faqs.html

Oracle Java downloads now require logging in to an Oracle account to download Java updates, like the latest Oracle Java 8u211 / Java SE 8u212. Because of this I cannot update the PPA with the latest Java (and the old links were broken by Oracle).

For this reason, THIS PPA IS DISCONTINUED (unless I find some way around this limitation).

Oracle Java (JDK) Installer (automatically downloads and installs Oracle JDK8). There are no actual Java files in this PPA.

Important -> Why Oracle Java 7 And 6 Installers No Longer Work: http://www.webupd8.org/2017/06/why-oracle-java-7-and-6-installers-no.html

Update: Oracle Java 9 has reached end of life: http://www.oracle.com/technetwork/java/javase/downloads/jdk9-downloads-3848520.html

The PPA supports Ubuntu 18.10, 18.04, 16.04, 14.04 and 12.04.

More info (and Ubuntu installation instructions):
- http://www.webupd8.org/2012/09/install-oracle-java-8-in-ubuntu-via-ppa.html

Debian installation instructions:
- Oracle Java 8: http://www.webupd8.org/2014/03/how-to-install-oracle-java-8-in-debian.html

For Oracle Java 11, see a different PPA -> https://www.linuxuprising.com/2018/10/how-to-install-oracle-java-11-in-ubuntu.html
 更多信息： https://launchpad.net/~webupd8team/+archive/ubuntu/java
按回车继续或者 Ctrl+c 取消添加

gpg: 钥匙环‘/tmp/tmpo42fd61g/secring.gpg’已建立
gpg: 钥匙环‘/tmp/tmpo42fd61g/pubring.gpg’已建立
gpg: 下载密钥‘EEA14886’，从 hkp 服务器 keyserver.ubuntu.com
gpg: /tmp/tmpo42fd61g/trustdb.gpg：建立了信任度数据库
gpg: 密钥 EEA14886：公钥“Launchpad VLC”已导入
gpg: 没有找到任何绝对信任的密钥
gpg: 合计被处理的数量：1
gpg:               已导入：1  (RSA: 1)
OK

```
重要的是这两句
1. There are no actual Java files in this PPA.不解释。
2. 安装Java11的介绍[https://www.linuxuprising.com/2018/10/how-to-install-oracle-java-11-in-ubuntu.html](https://www.linuxuprising.com/2018/10/how-to-install-oracle-java-11-in-ubuntu.html)




### 好使的命令

```
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer
```


参考 
[ubuntu16.04下安装Java8详细教程](https://blog.csdn.net/mucaoyx/article/details/82949450)
[ubunt下安装不同版本的jdk(openjdk  oraclejdk)](https://www.cnblogs.com/guxiaobei/p/8556586.html)
[Ubuntu中PPA源](https://www.cnblogs.com/EasonJim/p/7119331.html)
