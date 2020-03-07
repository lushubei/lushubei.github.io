---
layout:     post  
title:      "Serverless"  
subtitle:   "Spring Cloud Function"  
date:       2020-02-29 10:48:00  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 技术  
    - java  
---  

###### Serverless是啥？

是指构建和运行不需要服务器管理的应用程序的概念
 
CNCF 的定义，Serverless翻译为无服务应用，是相对于当前主流的有服务(Server)的应用而言的。  
>Serverless computing refers to the concept of building and running applications that do not require server management. --- CNCF

先来回顾一下Server的应用是如何工作的：  

例如我们要开发一个API，要做哪些事呢  
> 1. 首先,那么我们需要部署一个应用服务器(nginx/tomcat)
> 2. 然后,我们再写一套mvc的代码(spring MVC)
> 3. 最后，我们专心的写业务逻辑部分(说到底就是一个业务方法，记住这个方法哦)
> 4. 另外，为了达到api的安全高可用，我们还要处理公共事务，比如服务发现、分布式、事务、熔断，配置中心等等，也可以选一些优秀的实践，来帮我们做(比如spring cloud/springboot等)  

这4步，在企业内部 1，2，4 是一般是技术架构师的工作。这部分工作，一些框架在极力为我们降低使用门槛，这点java做的比python好的多。好像一批大牛架构师，帮大家把技术框架优化。

但在企业角度和产品价值最相关的部分是什么？ 是第3点，那个**业务方法**。

框架优化的极致是什么呢？  是我们开发第3部分逻辑的时候，不用关注 1，2，4部分。写函数方法即可，于是乎，一些公有云推出了FAAS服务。
例如：Amazon Lambda，Apache OpenWhisk，Microsoft Azure和Project Riff

整个社会组成了一个大企业，这些公有云服务提供商在其中扮演着技术架构，运维，安全等一些共性的部分，企业内部的开发更foucus在企业自身业务功能开发，社会化分工越来越细。

Serverless = FAAS 在加上一些云服务
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-02-29/69205734031453.png?Expires=4736561209&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=0cqK%2BmOKQX6IM81a1A3powX4nkQ%3D)

Serverless 则可以理解为运行在 FaaS 中的，使用了 BaaS 的函数。

######FAAS
FaaS（Function as a Service）
让用户抛弃了对服务器的配置和管理，仅需编写和上传核心业务代码，即可获得对应的数据结果，什么弹性扩容、自动部署这些细节的东西都交给云厂商去做了。

######BAAS 
BaaS（Backend as a Service）则是一些后端云服务，比如云数据库、对象存储、消息队列等。利用 BaaS，可以极大简化我们的应用开发难度。

Serverless 的主要特点有：

* 事件驱动  
  函数在 FaaS 平台中，需要通过一系列的事件来驱动函数执行。
* 无状态  
  因为每次函数执行，可能使用的都是不同的容器，无法进行内存或数据共享。如果要共享数据，则只能通过第三方服务，比如 Redis 等。
* 无运维  
  使用 Serverless 我们不需要关心服务器，不需要关心运维。这也是 Serverless 思想的核心。
* 低成本  
  使用 Serverless 成本很低，因为我们只需要为每次函数的运行付费。函数不运行，则不花钱，也不会浪费服务器资源。  