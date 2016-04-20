---
layout: post
title:  "memcached安装"
date:   2016-04-13 16:13:26
categories: 技术 memcached
---

memcache是高性能，分布式的内存对象缓存系统，用于在动态应用中减少数据库负载，提升访问速度。据说官方所说，其用户包括twitter、digg、flickr等，都是些互联网大腕呀。目前用memcache解决互联网上的大用户读取是非常流行的一种用法。



### Linux下安装

#### 下载
到其官网（[memcached.org](http://memcached.org/)）下载，目前最新的stable下载版本是1.4.25

还要再安装libevent这个软件，从官方（http://monkey.org/~provos/libevent/ ）下载，目前最新的稳定版是1.4.14。

下载后，将其上传到了`/home/blue/`下面

执行以下命令

    cd /home/blue
    tar zxvf memcached-1.4.5.tar.gz
    tar zxvf libevent-1.4.14b-stable.tar.gz
    #安装libevent
    cd libevent-1.4.14b-stable
    ./configure --prefix=/home/liuzhy/libevent-1.4.14b-stable
    make
    make install

    #安装memcache
    cd /home/blue/memcached-1.4.5
    ./configure --prefix=/home/blue/memcached-1.4.5 --with-libevent=/home/blue/libevent-1.4.14b
    make
    make install

#### 启动memcache服务

进入bin目录，执行：`./memcached -d -m 1024 -u blue`，但是系统说有一个共享库没有加载，共享库的名称为：libevent-1.4.so.2

首先要查看一下memcached 这个命令用到的链接库地址在哪儿。执行如下命令可以查看：

    LD_DEBUG=libs /usr/local/memcached/bin/memcached -v


显示出memcache从哪些地方找`libevent-1.4.so.2`这个文件，所以，我们只有将`libevent-1.4.so.2`这个文件指定到上面任意一个目录即可。这里我们将其指定到/lib64/下面。做一个软连接即可。命令如下：

    ln -s /usr/local/lib/libevent-1.4.so.2 /usr/lib/libevent-1.4.so.2

在启动一下memcache服务：./memcached -d -m 1024 -u blue就可以了

下面将memcached命令的参数罗伦如下，


    /usr/local/bin/memcached -d -m 200 -u root -l 192.168.1.91 -p 12301 -c 1000 -P /tmp/memcached.pid

相关解释如下：

* -d选项是启动一个守护进程，
* -m是分配给Memcache使用的内存数量，单位是MB，这里是200MB
* -u是运行Memcache的用户，如果当前为 root 的话，需要使用此参数指定用户。
* -l是监听的服务器IP地址，如果有多个地址的话，我这里指定了服务器的IP地址192.168.1.91
* -p是设置Memcache监听的端口，我这里设置了12301，最好是1024以上的端口
* -c选项是最大运行的并发连接数，默认是1024，这里设置了256
* -P是设置保存Memcache的pid文件，我这里是保存在 /tmp/memcached.pid

#### 停止Memcache进程
    kill `cat /tmp/memcached.pid`
也可以启动多个守护进程，但是端口不能重复

一开始说的“-d”参数需要进行进一步的解释：

* -d install 安装memcached
* -d uninstall 卸载memcached
* -d start 启动memcached服务
* -d restart 重启memcached服务
* -d stop 停止memcached服务
* -d shutdown 停止memcached服务


#### 检查服务

1. 查看启动的memcache服务：

    netstat -lp | grep memcached
2. 查看memcache的进程号（根据进程号，可以结束memcache服务：“kill -9 进程号”）

    ps -ef | grep memcached

#### 测试
    [root@localhost /]# telnet 192.168.141.64 12000
    Trying 192.168.141.64...
    Connected to 192.168.141.64 (192.168.141.64).
    Escape character is '^]'.
    set key1 0 60 4
    zhou
    STORED
    get key1
    VALUE key1 0 4
    zhou
    END


------




### WINIOWS 下安装

[下载地址64位地址](http://blog.couchbase.com/memcached-windows-64-bit-pre-release-available)

安装目录
memcached -d install


1、下载后解压到D:\memcached（下载地址：memcached-win64下载地址）

2、安装到windows服务，打开cmd命令行，进入memcached目录，执行memcached -d install命令，安装服务。
**如果在没有安装过的情况下，出现"failed to install service or service already installed"错误，可能是cmd.exe需要用管理员身份运行。**


3、启动服务，执行`memcached -d start` 如果命令行无法启动可以到service管理器启动

4、参数介绍

    -p 监听的端口
    -l 连接的IP地址, 默认是本机
    -d start 启动memcached服务
     -d restart 重起memcached服务
     -d stop|shutdown 关闭正在运行的memcached服务
     -d install 安装memcached服务
     -d uninstall 卸载memcached服务
     -u 以的身份运行 (仅在以root运行的时候有效)
     -m 最大内存使用，单位MB。默认64MB
     -M 内存耗尽时返回错误，而不是删除项
     -c 最大同时连接数，默认是1024
     -f 块大小增长因子，默认是1.25
     -n 最小分配空间，key+value+flags默认是48
     -h 显示帮助

5、修改参数，windows下需要通过修改注册表信息进行设置，打开注册表，找
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\memcached
在其中有一个“ImagePath”项，值为：
"D:\memcached\memcached.exe" -d runservice
在后面加上“-m 1024 -c 2048 -p 11210”。等即可。重启服务后生效
