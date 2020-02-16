---
layout:     post
title:      "linux top命令使用"
subtitle:   "常用的命令"
date:       2015-03-10 12:00:00
author:     "Shubei"
header-img: "img/post-bg-apple-event-2015.jpg"
tags:
    - 知乎
    - 产品
---
# top 查看内存
```shell
top - 10:06:27 up 180 days, 22:11,  7 users,  load average: 0.09, 0.17, 0.21
Tasks: 205 total,   1 running, 204 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.2 us,  0.1 sy,  0.0 ni, 98.6 id,  0.0 wa,  0.0 hi,  0.1 si,  0.0 st
KiB Mem : 16267512 total,  1896704 free,  6719636 used,  7651172 buff/cache
KiB Swap:  4063228 total,  3776408 free,   286820 used.  8292772 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
16812 root      20   0 6179520 639248  14200 S   4.7  3.9   6:24.40 java
11425 root      20   0 6332676 722092  14280 S   1.7  4.4  13:04.84 java
16260 1000      20   0 1270264 144236   3056 S   1.3  0.9 756:22.25 beam.smp
```
## 头部信息
### 1. 第一行，任务队列信息，同 uptime 命令的执行结果
系统时间：19:27:01
运行时间：up 54 min
当前登录用户：  1 user
负载均衡(uptime)  load average: 0.02, 0.03, 0.00
     average后面的三个数分别是1分钟、5分钟、15分钟的负载情况。
load average数据是每隔5秒钟检查一次活跃的进程数，然后按特定算法计算出的数值。如果这个数除以逻辑CPU的数量，结果高于5的时候就表明系统在超负荷运转了

### 2. 第二行，Tasks — 任务（进程）
总进程:159 total, 运行:1 running, 休眠:157 sleeping, 停止: 1 stopped, 僵尸进程: 0 zombie
### 3. 第三行，cpu状态信息
0.0%us【user space】— 用户空间占用CPU的百分比。
0.1%sy【sysctl】— 内核空间占用CPU的百分比。
0.0%ni【】— 改变过优先级的进程占用CPU的百分比
99.9%id【idolt】— 空闲CPU百分比
0.0%wa【wait】— IO等待占用CPU的百分比
0.0%hi【Hardware IRQ】— 硬中断占用CPU的百分比
0.0%si【Software Interrupts】— 软中断占用CPU的百分比

### 4. 第四行,内存状态
1012288k total,   234464k used,   777824k free,    24084k buffers【缓存的内存量】

可用内存=free + buffer + cached
### 5. 第五行，swap交换分区信息
614396k total,        0k used,   614396k free,    72356k cached【缓冲的交换区总量】

### 6. 第六行，空行
### 7. 第七行以下：各进程（任务）的状态监控
```shell
PID — 进程id
USER — 进程所有者
PR — 进程优先级
NI — nice值。负值表示高优先级，正值表示低优先级
VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
SHR — 共享内存大小，单位kb
S —进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
%CPU — 上次更新到现在的CPU时间占用百分比
%MEM — 进程使用的物理内存百分比
TIME+ — 进程使用的CPU时间总计，单位1/100秒
COMMAND — 进程名称（命令名/命令行）
```

## 2. 其他使用技巧
1. 高亮显示当前运行进程：敲击键盘“b”（打开/关闭加亮效果）
2. 进程字段排序：敲击键盘“x”（打开/关闭排序列的加亮效果）
3. 通过”shift + >”或”shift + <”可以向右或左改变排序列
4. top交互命令
```
h 显示帮助画面，给出一些简短的命令总结说明
k 终止一个进程。
i 忽略闲置和僵死进程。这是一个开关式命令。
q 退出程序
r 重新安排一个进程的优先级别
S 切换到累计模式
s 改变两次刷新之间的延迟时间（单位为s
f/F 从当前显示中添加或者删除项目
o/O 改变显示项目的顺序
l 切换显示平均负载和启动时间信息
m 切换显示内存信息
t 切换显示进程和CPU状态信息
c 切换显示命令名称和完整命令行
M 根据驻留内存大小进行排序
P 根据CPU使用百分比大小进行排序
T 根据时间/累计时间进行排序
W 将当前设置写入~/.toprc文件中
```