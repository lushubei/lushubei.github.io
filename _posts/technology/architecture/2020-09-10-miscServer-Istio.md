---
layout:     post  
title:      "下一代微服务框架Istio"  
subtitle:   "微服务框架"  
date:       2020-09-10  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 技术  
    - 架构    
---  

#### 起源

在精彩的软件容器世界中，原本早已解决的问题又有新的解决方案出现，给人有种恍惚的感觉。在许多情况下这些问题很久以前就得到了解决，但现代云原生架构的出现，推动部署更大规模的应用程序，这就需要新的工具和方法来管理这些服务。

微服务就是一个很好的例子。在此模型下，典型的应用程序或服务将被分解为可独立部署的功能模块，这些模块可以彼此分开扩展和维护，并且链接在一起以提供整个应用程序或服务的全部功能。

在使用容器开发微服务时，后者可能是比较棘手的部分。当所有组件可能分布在服务器节点集群中，并且它们的实例不断上线并被更新的版本替换时，如何将它们连接起来？在面向服务的体系结构（SOA）中，微服务可以被看作是进化的继承者，这种任务类似于企业服务总线（ESB）所处理的任务。因此，我们需要的是ESB的云原生版本。

这是一个相对较新的开源项目Istio旨在填补的工作。它被正式描述为服务网格，因为它的一部分与基础设施一起分布在它管理的容器旁边，并且它开始着手满足服务发现、负载均衡、消息路由、遥测和监控的要求，当然还有安全。

Istio源自IBM和谷歌之间的合作，实际上包含了一些现有的组件，特别是由打车服务公司Lyft开发的组件。它以某种形式存在至少一年，但最终在当前7月底达到1.0版的里程碑(截止2020.9月已经1.7版本了)，这意味着它现在终于被认为足够成熟，可以作为生产基础设施的一部分来运行。

云原生生态系统已基本确定容器作为核心打包和运行时构造，而Kubernetes则作为管理容器的编排系统。但McGee解释说，还有第三块谜题还在空中，Istio旨在满足这一要求。

“如何管理在容器平台上运行的应用程序或服务之间的交互？”,McGee说他几年前在IBM的团队已经开始研究这个问题了，当时他遇到了谷歌的同行并发现他们正走在同一条道路上，但IBM主要关注流量路由、版本控制和A/B测试方面，Google专注于安全和遥测。两者决定合并各自的努力成果，这样就诞生了Istio。

#### 解决了什么问题
云平台为使用它们的组织提供了很多好处。但是，不可否认的是，采用云技术会对DevOps团队造成压力。开发人员必须使用微服务来构建可移植性，同时运营商正在管理超大型混合和多云部署。Istio使您可以连接，保护，控制和观察服务。

从较高的角度来看，Istio有助于降低这些部署的复杂性，并减轻开发团队的负担。它是一个完全开源的服务网格，可以透明地分层到现有的分布式应用程序上。它也是一个平台，包括可将其集成到任何日志记录平台，遥测或策略系统中的API。Istio的各种功能集使您能够成功，高效地运行分布式微服务体系结构，并提供一种统一的方式来保护，连接和监视微服务。

Istio是一个Service Mesh开源项目，是Google继Kubernetes之后的又一力作，主要参与的公司包括Google，IBM和Lyft。

凭借Kubernetes良好的架构设计及其强大的扩展性，Google围绕Kubernetes打造一个生态系统。Kubernetes用于微服务的编排（编排是英文Orchestration的直译，用大白话说就是描述一组微服务之间的关联关系，并负责微服务的部署、终止、升级、缩扩容等）。其向下用CNI（容器网络接口），CRI（容器运行时接口）标准接口可以对接不同的网络和容器运行时实现，提供微服务运行的基础设施。向上则用Istio提供了微服务治理功能。

由下图可见，Istio补充了Kubernetes生态圈的重要一环，是Google的微服务版图里一个里程碑式的扩张。
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-09-10/544269147489900.png?Expires=4753299921&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=N%2B85vT1WkW1elmx2KLxVQBVdSns%3D)

#### 核心技术原理
Istio可以轻松创建带有负载平衡，服务到服务的身份验证，监视等功能的已部署服务网络，而服务代码中的代码更改很少或没有更改。通过在整个环境中部署特殊的sidecar代理来拦截微服务之间的所有网络通信，然后使用其控制平面功能配置和管理Istio，可以为服务添加Istio支持，包括：

- HTTP，gRPC，WebSocket和TCP通信的自动负载平衡。  
- 通过丰富的路由规则，重试，故障转移和故障注入对流量行为进行精细控制。  
- 可插拔的策略层和配置API，支持访问控制，速率限制和配额。  
- 群集内所有流量的自动指标，日志和跟踪，包括群集的入口和出口。  
- 通过强大的基于身份的身份验证和授权，确保群集中服务之间的通信安全。  

Istio专为可扩展性而设计，可满足多种部署需求。它通过拦截和配置网状网络流量来做到这一点，如下图所示 
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-09-10/543297379236100.png?Expires=4753298953&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=UOm07Zot%2BBzQWQ9UrgK2C%2FDxaAw%3D)


##### Istio与Kubernetes架构结合
Kubernetes提供了部署、升级和有限的运行流量管理能力，但并不具备熔断、限流降级、调用链治理等能力。Istio是基于Kubernetes构建的开放平台，它很好的补齐了Kubernetes在微服务治理上的诸多能力。
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-09-10/545433482476800.png?Expires=4753301085&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=4Q9lW%2Fde%2FHqXj8dOLAUSPDSAl7o%3D)

架构结合  
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2020-09-10/545614047581700.png?Expires=4753301266&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=AKBLm2994rd3FU3aLznTbSoLzgw%3D)

##### 核心功能

- 流量管理
  > 简单的规则配置和流量路由器，可以控制服务之间的流量和API调用流.Istio简化了诸如断路器，超时和重试之类的服务级别属性的配置，并使其轻而易举地设置灰度发布，A/B测试等.

- 安全性
  > Istio提供基础安全通信通道，大规模管理服务同学的身份验证、授权和加密。在基础网络代理层面实现，各种流量管控，不需要修改应用程序.
        
- 可观察性
  > 提供强大的追踪、监视和日志记录功能使大家深入了解服务网络部署。
       
#### 当前发展情况  

#### 未来发展方向 

Istio目标是成为容器化微服务的网状管道  
应该注意的是，其他服务网状平台已经存在，例如开源端的Linkerd或Conduit，而微软有一项服务，被称为Azure Service Fabric Mesh，目前作为其云平台的技术预览。  
谷歌已经添加了Istio支持，尽管它只是将其标记为alpha版本，作为托管服务的一部分，该服务在其云平台上的客户的Google Kubernetes Engine（GKE）集群中自动安装和维护。  
Istio也获得了业内其他公司的支持，尤其是Red Hat，几年前Red Hat以Docker容器和Kubernetes为基础，重新设计了OpenShift应用程序平台。Red Hat打算将服务网格集成到OpenShift中，但Red Hat希望在提交之前能够看到改进的一些粗略的地方，例如多租户支持。

#### 扩展阅读
[官网](https://istio.io/)    
[github地址](https://github.com/istio/istio)  
[Istio架构与技术](https://blog.csdn.net/fly910905/article/details/103880756)  
[IBM云计算CTO讲述Istio项目的起源、分工及目标](https://www.servicemesher.com/blog/istio-aims-to-be-the-mesh-plumbing-for-containerized-microservices/)
