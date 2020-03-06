---
layout:     post
title:      "vmware esxi 命令行手动克隆虚拟机"
subtitle:   "手动克隆虚拟机"
date:       2016-02-01 12:00:00
author:     "Shubei"
header-img: "img/post-bg-re-vs-ng2.jpg"
header-mask: 0.3
catalog:    true
tags:
  - dev
---

. 使用命令
0. 登录wmware控制端打开宿主机的ssh通道

1. 通过ssh登录ESXi(宿主机)

2. 到存储目录下创建新虚拟机目录
```
# cd /vmfs/volumes/datastore1/  
# mkdir newvm
```
3. 复制虚拟机配置文件(文件名可改) 
```
# cp /vmfs/volumes/datastore1/oldvm/oldvm.vmx /vmfs/volumes/datastore1/newvm/newvm.vmx
```
4. 使用命令克隆磁盘文件
```
#vmkfstools  -i /vmfs/volumes/datastore1/oldvm/oldvm.vmdk -d thin /vmfs/volumes/datastore1/newvm/newvm.vmdk
```

【vmkfstools命令用法】
【vmkfstools -i<source-disk-name.vmdk>-d {thin|thick} <destination-disk-name.vmdk>】
【-i <source-disk-name.vmdk> 参数:源虚拟机vmdk磁盘名
-d {thin|thick}<destination-disk-name.vmdk> 
目标磁盘的格式,thin或thick; 缺省是thick
<destination-disk-name.vmdk>为要生成的目标vmdk磁盘名】


5. 修改新虚拟机配置文件
```
# cd /vmfs/volumes/datastore1/newvm/
# vi newvm.vmx
```
将oldvm.vmdk替换成newvm.vmdk   可使用:%s/oldvm/newvm/g进行全局替换修改


6.虚拟机界面从/vmfs/volumes/datastore1/newvm/newvm.vmx注册新虚拟机