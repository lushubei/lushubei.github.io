---  
layout:     post  
title:      "百度统计调研"  
subtitle:   "百度统计核心功能"  
date:       2018-09-12  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 产品  
---  
#### 路径转化漏斗
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-03-07/163474600437568.png?Expires=4737175265&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=BgMJrsbgQyrfdEqKI8RekiXxmdM%3D)

可方便找出页面间流量丢失情况  

#### 上下游分析
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-03-07/163549022914570.png?Expires=4737175340&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=fx63JmWbF2UDDEReOyVOllbFmuQ%3D)

#### 页面点击热力图  
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-03-07/163628501461092.png?Expires=4737175419&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=FJahOK8J11lZ9wC5e9afhWEP6iU%3D)

可用于优化页面布局  

#### 用户洞察  
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-03-07/163699419932476.png?Expires=4737175491&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=xt6CNnxdWfEud%2BGa6QsNpdUE%2Fw8%3D)

可用于进行典型用户分析    

---
个人评论：  
```
百度统计使用方式：前端页面代码通过引入百度统计的js文件，仅几行代码，入侵性小。 
百度统计的实现原理：当用户在系统中进行操作的时候，触发百度统计的js事件，将操作信息发送到百度统计的后台数据库。
抓取的数据来源主要是http请求header中的数据与少量一些百度js自定义的操作事件. 可以快速满足一些基本的用户数据采集.
```



