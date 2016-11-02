---
layout: post
title:  Java Service Wrapper
date:   2016-11-02 14:29:00
categories: 技术 jsw
---

在实际开发过程中很多模块需要独立运行，他们并不会以web形式发布，传统的做法是将其压缩为jar包独立运行，这种形式简单易行也比较利于维护，但是一旦服务器重启或出现异常时，程序往往无法自行修复或重启。解决服务器重启的传统做法是编写一段shell脚本随服务器启动而运行，但是这样做只是治标，那么我们想寻求一种“治本”的方式该怎么办呢？
Java Service Wrapper就轻松而简单的为我们解决了这些问题。"Java Service Wrapper"顾名思义，将我们的Java程序包装成系统服务，这样就可以随着系统的运行而自动运行，当然Java Service Wrapper(下面简称Wrapper)的功能绝不仅于此。

Wrapper下载地址：http://wrapper.tanukisoftware.com/doc/english/download.jsp



通过下载页面我们可以看到Wrapper几乎支持所有的系统环境，说明Wrapper在这方面还是很下工夫的，目前最新版本为3.5.20，我们选择Linux x86版本下载，解压后目录组成如下图所示：



为了更直观的了解Wrapper的目录及文件结构，可以通过"tree"命令列出Wrapper的所有文件树，cmd控制台下输入命令：
Cmd代码  收藏代码
tree /f  

显示目录结构如下：  
```txt
wrapper-linux-x86.  
│  jdoc.tar.gz             //javadoc文件  
│  README_de.txt           //说明  
│  README_en.txt           //说明  
│  README_es.txt           //说明  
│  README_ja.txt           //说明  
│  
├─bin                      //执行文件目录  
│      demoapp             //示例程序  
│      testwrapper         //测试程序  
│      ★wrapper           //主程序(重要)  
│  
├─conf                     //配置文件目录  
│      demoapp.conf        //示例配置文件  
│      ★wrapper.conf      //主配置文件(重要，文件名可修改)  
│  
├─doc                      //说明文档目录  
│      index.html          //首页  
│      revisions.txt       //版本说明  
│      wrapper-community-license-1.1.txt  //许可协议  
│  
├─jdoc                     //javadoc文档目录  
│      index.html          //首页  
│  
├─lib                      //依赖类库目录  
│      ★libwrapper.so     //wrapper linux文件(.so：用户层的动态库)  
│      ★wrapper.jar       //wrapper主程序(重要)  
│      wrapperdemo.jar     //示例程序  
│      wrappertest.jar     //测试程序  
│  
├─logs                     //日志目录  
│      wrapper.log         //日志文件  
│  
└─src                      //源代码目录  
├─bin                  //执行程序目录  
│      ★sh.script.in  //shell脚本源代码(重要)  
└─conf                 //配置目录  
    wrapper.conf.in    //原始配置  
```

以下是官方给出的一些Wrapper的优点：
(1) 使用我们的产品无须在你的程序中添加任何额外的代码。
(2) 当你的程序或JVM出现问题时会自动响应事先定制的策略。
(3) 当出现问题时会及时进行通知。
(4) 完善的日志记录功能可以更好为您提供支持。
(5) 在不同的系统上你可以指定一个标准的流程相同流程，也就是说相同的程序可以不必修改即运行于不同系统。
(6) 可以将你的应用安装成windows或unix的服务或守护进程。

看到Wrapper有这么多好处，那么我们就通过Wrapper自带的示例程序来进一步了解Wrapper吧：
1.创建服务工作目录，以操作系统为Linux，目录结构为usr/local/wrapper为例，按照上面的目录结构，在其下创建"bin","conf","lib","logs"这四个相同名称的文件夹。
2.将配置及程序文件复制至相应目录（也就是上面标★的文件）；
(1)bin 目录下的wrapper 文件复制到usr/local/wrapper/bin下。
(2)src\bin 目录下的sh.script.in 文件复制到usr/local/wrapper/bin下，并将.in后缀名删除并修改名称，修改后为javaService.script。
(3)conf 目录下的wrapper.conf 文件复制到usr/local/wrapper/conf下。
(4)lib 目录下的wrapper.jar 和libwrapper.so 文件复制到usr/local/wrapper/lib下。
注：以上是正式环境所需文件的配置方式，这里我们需要运行Wrapper自带的demo程序，所以需要将demoapp,demoapp.conf,wrapperdemo.jar 这三个文件复制到相应目录。
3.进入bin目录执行以下命令：
Shell代码  收藏代码
./demoapp start  

接下来会显示很多提示，最终显示如下页面：



出现此页面证明你的程序已经运行成功了，恭喜！
如果启动失败，我们可以查看logs日志内容，如下：
Log代码  收藏代码
```bash
STATUS | wrapper  | 2013/07/30 11:22:47 | --> Wrapper Started as Daemon  
STATUS | wrapper  | 2013/07/30 11:22:47 | Java Service Wrapper Community Edition 64-bit 3.5.20  
STATUS | wrapper  | 2013/07/30 11:22:47 |   Copyright (C) 1999-2013 Tanuki Software, Ltd. All Rights Reserved.  
STATUS | wrapper  | 2013/07/30 11:22:47 |     http://wrapper.tanukisoftware.com  
STATUS | wrapper  | 2013/07/30 11:22:47 |   
STATUS | wrapper  | 2013/07/30 11:22:47 | Launching a JVM...  
INFO   | jvm 1    | 2013/07/30 11:22:47 | WrapperManager: Initializing...  
INFO   | jvm 1    | 2013/07/30 11:22:47 | DemoApp: Initializing...  
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo: start()  
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo: Showing dialog...  
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo:   
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo: ERROR - Unable to display the GUI:  
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo:           java.awt.HeadlessException:   
INFO   | jvm 1    | 2013/07/30 11:22:47 | No X11 DISPLAY variable was set, but this program performed an operation which requires it.  
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo:   
INFO   | jvm 1    | 2013/07/30 11:22:47 | Demo: This demo requires a display to show its GUI.  Exiting...  
INFO   | jvm 1    | 2013/07/30 11:22:48 | Demo: stop(0)  
STATUS | wrapper  | 2013/07/30 11:22:49 | <-- Wrapper Stopped  
```

从日志内容可以查看程序及服务的运行状态，Wrapper日志采用此种格式：类型 | 拥有者 | 时间 | 具体内容
日志内容显示我们的Linux系统没有安装图形界面或者根本没有显卡。
注：这里需要说明一下，Wrapper运行首先需要Java运行环境支持，所以在使用Wrapper前请先确认已安装好了Java

下面我们来尝试一下无参数调用服务的方式，如：
Shell代码  收藏代码
./testwrapper  
./demoapp  
两者的提示相同，都为：
Shell代码  收藏代码
Usage: ./程序名 [ console {JavaAppArgs} | start {JavaAppArgs} | stop | restart {JavaAppArgs} | condrestart {JavaAppArgs} | status | install | remove | dump ]  

Commands:  
console      Launch in the current console.  
start        Start in the background as a daemon process.  
stop         Stop if running as a daemon or in another console.  
restart      Stop if running and then start.  
condrestart  Restart only if already running.  
status       Query the current status.  
install      Install to start automatically when system boots.  
remove       Uninstall.  
dump         Request a Java thread dump if running.  

JavaAppArgs: Zero or more arguments which will be passed to the Java application.  

原来Wrapper提供了很多种参数的选择，如：start为启动，stop为停止。下面为参数的详细解释：
Shell代码  收藏代码
Commands:  
console      启动并显示控制台信息  
start        作为一个守护进程后台启动  
stop         停止程序  
restart      重启程序  
condrestart  重启已经运行的程序，与前者区别是程序必须已经在运行  
status       查看该程序状态  
install      将程序安装为自启动服务，即随系统启动而启动  
remove       卸载自启动服务  
dump         报告运行时的Java thread dump(thread dump百度百科：http://baike.baidu.com/view/5111187.htm)  

我们还发现单独运行wrapper命令时的提示内容与前面两者不同，如下所示：
Shell代码  收藏代码
Java Service Wrapper Community Edition 64-bit 3.5.20  
Copyright (C) 1999-2013 Tanuki Software, Ltd. All Rights Reserved.  
http://wrapper.tanukisoftware.com  

Usage:  
./wrapper <command> <configuration file> [configuration properties] [...]  
./wrapper <configuration file> [configuration properties] [...]  
(<command> implicitly '-c')  
./wrapper <command>  
(<configuration file> implicitly 'wrapper.conf')  
./wrapper  
(<command> implicitly '-c' and <configuration file> 'wrapper.conf')  

where <command> can be one of:  
-c  --console run as a Console application  
-v  --version print the wrapper's version information.  
-?  --help    print this help message  
-- <args>     mark the end of Wrapper arguments.  All arguments after the  
        '--' will be passed through unmodified to the java application.  

<configuration file> is the wrapper.conf to use.  Name must be absolute or relative  
to the location of ./wrapper  

[configuration properties] are configuration name-value pairs which override values  
in wrapper.conf.  For example:  
wrapper.debug=true  

Please note that any file references must be absolute or relative to the location  
of the Wrapper executable.  

因为wrapper是Wrapper运行的主程序也是核心，他无法单独运行需要通过src/bin中的sh.script.in这个shell脚本调用，这个文件的使用我们之后会讲到。
运行wrapper可以按如上提示添加参数，如：./wrapper -c wrapper.properties

以上就是对Wrapper的一个整体认识，希望此文可以帮助大家更快的上手并使用Wrapper，之后几篇文章会详细讲解Wrapper的配置及定制自己的应用。


转自 http://286.iteye.com/blog/1915478
