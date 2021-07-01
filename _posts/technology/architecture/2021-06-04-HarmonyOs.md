---  
layout:     post  
title:      "鸿蒙系统的介绍"  
subtitle:   "最简单的鸿蒙系统介绍"  
date:       2021-06-04  
author:     "Shubei"  
header-img: "img/post-bg-2015.jpg"  
catalog: true  
tags:  
    - 技术理解  
---  

#### 鸿蒙系统与Android、Linux、Ios什么关系

把Windows比做狮子，那linux就是老虎，ios就是豹。安卓是老虎的一个品种孟加拉虎，鸿蒙是老虎的另一品种东北虎，东北虎和孟加拉虎没有生殖隔离（可以做到兼容，例如鸿蒙兼容apk）。你可以说东北虎是老虎linux，但你说东北虎是孟加拉虎就显得你无知了。这只是一个类比，大致准确不是绝对准确，杠精绕道。linux开发者明确表态过世界上任何科技公司都可以开发魔改linux，linux属于世界，美国和谷歌无权过问

![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2021-06-04/9269495198660.png?Expires=4776371235&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=8oL945cVUgSn3gELqbAWMWJWscM%3D)

#### 起源
![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2021-06-04/10638694068769.png?Expires=4776372604&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=%2B97md2AHYUmXPpy2yvFIz03GDP0%3D)

#### 定位
任正非说：“我们是为了万物互联、将来走向智能社会所做的一个操作系统。”
华为消费者业务CEO余承东的解释则更为详细：“不同于安卓系统，鸿蒙系统不只是一个智能手机操作系统。还是一款面向下一代技术而设计的多元化操作系统，一套全面打通了手机、电脑、平板、电视、汽车、智能穿戴等诸多平台的操作系统。”
就定位而言，鸿蒙从来都不是安卓的替代品，而是继任者。

#### 软肋
“做一个操作系统的技术难度不大，难度大的是生态。”2019年5月，任正非对记者表示。

#### 机遇
IoT（物联网）时代现在正处于黎明前夕，再加上美国的制裁

#### 鸿蒙系统的发展路径。

事实上，早在2019年的华为开发者大会上，余承东就清晰介绍了鸿蒙系统的发展路径。

在发展早期，鸿蒙必须考虑生态兼容的问题，表现形式就是在内核上保留了Linux内核（从而可以兼容安卓生态），以及LiteOS（从而可以与前期华为推广的物联网布局进行衔接），以及鸿蒙系统（从而具备鸿蒙的优势性能）。

但随着IoT进程的发展，华为将逐步拿掉Linux和LiteOS，以一套内核代替所有其他。

#### 鸿蒙如何兼容android

![](http://shubei-blog.oss-cn-beijing.aliyuncs.com/pasteimageintomarkdown/2021-06-04/30442735988980.png?Expires=4776392409&OSSAccessKeyId=LTAI4Fv8o4J1qrtFrYcJsmA2&Signature=ZQdpzBfH632sWBooS9a8UgtsoNs%3D)

通过这样的一个架构实现：

1、安卓应用不用重写即可在鸿蒙上运行。

2、开发者可以经过小幅度修改，即可开发鸿蒙应用，鸿蒙应用执行效率大大优于安卓版本。

3、经过时间的推移，逐渐做大右侧，做薄左侧，实现掏空安卓的目的。
#### 竞争产品

事实上，早在2016年谷歌也提出了面向物联网时代的操作系统Fuchsia，并且也是基于微内核（Zircon）。