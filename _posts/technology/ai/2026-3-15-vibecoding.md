---
layout:     post  
title:      "这几天的vibe coding让我头皮发麻"  
subtitle:   "AI的能力已经可以干掉大量程序员了"  
date:       2026-3-15  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - AI
    - vibe coding
---

这几天一直在玩openclaw小龙虾，也在使用claude code进行vibe coding，对于现在AI模型的能力我还抱有一些担忧，但也去拥抱它。

于是我在心里设想了一个场景：使用一个我们曾开发过得，有客户愿意买单的系统，只给它描述，让AI自主来完成开发，然后给它一台服务器，让它扮演一个运维工程师的角色，来部署。

我的目标是写一个WMS，功能和技术栈都要求其尽可能贴近我们之前的项目。 要求它的技术栈为： 
```
后端
- Java 21 + Spring Boot 3.2.3
- MyBatis-Plus 3.5.6
- SQLite（数据库）
- BCrypt（密码加密，仅引入 spring-security-crypto）
- 自定义 JWT 认证

前端
- Vue 3 + TypeScript
- Vite（构建工具）
- Pinia（状态管理）
- Element Plus（UI 组件库）
- Axios（HTTP 请求）
```
结果，他真的实现了：
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2026-03-16/103306712427899.png?Expires=4927190425&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=9SMduiFUJbW%2F%2FSVS1iDBKYRH%2FuI%3D)

然后我提了一些bug，给它修复，又让它改写为适配移动端，它都很好的实现了。

代码编写完成后，我就购买了一台火山云的服务器，把证书下载后给claude code, 给它提要求：
这台服务器是新购买的服务器，还没安装其他组建，帮我安装nginx及其他任何需要的组建, 然后把wms系统部署起来，完成任务后通知我。它的登录秘钥文件为...

大概等了1个小时，它经过了修改后，真的把这项工作给完成了。而且还自动进行了一些可用性测试。

![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2026-03-16/103527366965701.png?Expires=4927190645&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=7PM0LeYN%2FINd3J5CE6YSepDgLME%3D)

当我登录系统，测试正常之后，这着实震惊到我了，所有tokens花费，大概70元，真的让我有种头皮发麻的感觉。我知道，一个新的时代到来了。我们的一套几个人开发几个月的系统，真的可以靠一个人，花1个月把它完成。

今天我做的这个小的验证，基本验证了这一点。程序员从运营魔法语言变成了运营自然语言。识别需求，定义产品的能力变得尤为重要。

抓住这时代的红利，抓住这关键的转型期。