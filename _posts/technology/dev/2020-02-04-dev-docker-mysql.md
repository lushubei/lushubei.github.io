---
layout: post
title: "Docker安装mysql"
subtitle: 'Docker使用简要'
author: "Shubei"
date: 2019-02-04 12:00:00
header-style: text
tags:
  - Docker
  - 笔记
---
# Docker使用简要(eg:docker方式安装mysql)

## 安装docker
mac 下载 docker.dmq
安装完成后，查看安装信息
```
docker info
```
配置镜像加速

```
graph LR
Docker图标--点击--> B[Preferences]
B-->Adanced
Adanced-->C[Insecure registries] 
C--点击号-->填入URL
```
registry.mirrors.aliyuncs.com

重启 docker
## docker方式安装mysql
2. 搜索mysql
```
docker search mysql
```

3. 拉取mysql
```
docker pull mysql:8.0
```
4. 启动mysql

```
docker run --name mysql8.0 -p 3307:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:8.0

备注 ：--name 服务启动别名设置  -p端口映射 宿主机端口：镜像运行端口  -d 镜像名：tag 使用守护进程模式启动 -e：设置root帐号密码
```

5. 查看运行镜像
```
~ docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
e2e84126cfd9        mysql:8.0           "docker-entrypoint.sh"   13 minutes ago      Up 13 minutes       33060/tcp, 0.0.0.0:3307->3306/tcp   mysql8.0
```

6. 进入镜像，运行mysql
```
docker exec -it mysql8.0 /bin/bash

备注：exec docker进入容器命令   -it 容器中服务别名 /bin/bash   表示命令行模式  与 -d 后台守护进行模式启动 形成两种运行方式   
进入容器中如图所示变化如下

root@e2e84126cfd9:
```

登录mysql
```
root@e2e84126cfd9:# cd /usr/bin

root@e2e84126cfd9:/usr/bin# mysql -u root -p

输入密码root 进入

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| spring5study       |
| sys                |
+--------------------+
```


---
## docker常用命令

#### 查看帮助
```
docker --help
某参数的帮助
docker run --help
```

#### 查看容器
```
docker ps -a
```

#### 启动/停止容器
```
docker start mysql8.0
docker start mysql8.0
```

#### 进去容器bash
```
~ docker exec -it mysql8.0 /bin/bash
root@e2e84126cfd9:/#
```

#### 拷贝文件到容器中

从宿主机拷贝到虚拟机
从虚拟机拷贝到宿主机
```
docker cp toContenor mysql8.0:/home/
docker cp mysql8.0:/home/toContenor .
```

#### 将容器commit成为一个镜像
```
docker commit containerA  newImageB
```

#### 运行容器
```
docker run  -p 8080:8080 -p 8081:8081 -v /home/data/:/home/data/ -dt newImageB
```