---
layout: post
title: "LINUX auto settime"
subtitle: ""
author: "Shubei"
date: 2017-12-12 12:00:00
header-img: "img/post-bg-infinity.jpg"
header-mask: 0.3
mathjax: true
tags:  
  - 技术  
  - dev
---
# 安装ntp
```
sudo yum install ntpdate
```
# 编辑crontab配置
vi /etc/crondtab

最先面添加：
每周日12点同步ntp中国授时中心

```
0 12 * * sun root ntpdate -u cn.pool.ntp.org
```
# 重启crontab服务
```
sudo service crond restart
```