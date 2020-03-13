---  
layout:     post  
title:      "Linux连接Samba网络文件共享服务"  
subtitle:   "smbclient"  
date:       2018-04-15  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 技术  
    - dev    
---  
#### smb是什么
Samba是一个能让Linux系统应用Microsoft网络通讯协议的软件，而SMB是Server Message Block的缩写。

#### mac上连接smb
> smb://172.16.28.6

#### Linux上连接smb

通过smbclient 客户端，它是samba套件，它提供一种命令行使用交互方式访问samba服务器的共享资源

> smbclient -L 172.16.28.6/touyan$ -U discovery-wu
输入密码登录

##### 1.1  列出当前文件状态
> smb:\>ls

##### 1.2 从windows中取得文件
> smb:\>get bbb.log

##### 1.3 将linux中的文件传输到windows之中
> smb:\.>put /linuxso.log    linuxso.log