---
layout: post
title: "Windows-nginx、flup、django 部署稳定应用"
subtitle: ''
date: 2019-01-15 12:00:00
author: "Shubei"
header-style: text
tags:
  - nginx
  - pyhton
  - dev
---
# 介绍
flup是一款用python开发的实现了UWSGI协议的web服务器，功能与uWSGI类似。因为在windows系统中无os.uname()函数，所以，uWSGI在windows系统中无法安装成功。故选用flup

# 安装nginx
1. Download nginx/windows from http://nginx.org/en/download.html
 
2. Unzip the pckage (like nginx-1.7.2.zip) to a folder in your machine)

nginx在windos系统中是一个exe应用程序

# 双击nginx.exe
闪退，此时服务已经启动，浏览器中输入 127.0.0.1:80 看到nginx服务页面

# 安装flup
```
pip install flup
```

# 配置nginx.conf

```
server {
        listen       8000;
        server_name  localhost;
        
        location ~ ^/ {
            fastcgi_pass 127.0.0.1:8001;
            #fastcgi_index index.html
            
            fastcgi_param PATH_INFO $fastcgi_script_name;
            fastcgi_param REQUEST_METHOD $request_method;
            fastcgi_param QUERY_STRING $query_string;
            fastcgi_param CONTENT_TYPE $content_type;
            fastcgi_param CONTENT_LENGTH $content_length;
            fastcgi_param SERVER_PROTOCOL $server_protocol;
            fastcgi_param SERVER_PORT $server_port;
            fastcgi_param SERVER_NAME $server_name;
            fastcgi_pass_header Authorization;
            fastcgi_intercept_errors off;
        }
    }
```
Nginx would listen on 8000 port, and when you access the django site, it would pass the request to your django application listening on 8001 port. 

Also add the following section to the conf file to tell nginx to render static files in your django application. 

# Start nginx (nginx.exe)
双击nginx.exe(注意，多次启动会启动多个进程，要从服务中停止，再启动)

# Start django, binding to nginx. 
```
python manage.py runfcgi host=127.0.0.1 port=8001 protocol=fcgi method=threaded
```

报错：
==Unknown command: 'runfcgi'==
原因，Django1.8 往后的版本，已经不支持 flup了，**卒**！

It's done, open a browser, and try http://127.0.0.1:8000




---
# 安装 wfastcgi
```
pip install wfastcgi
```

# IIS配置
在控制面板的程序和功能中，打开启用或关闭Windows功能窗口 
勾选Internet Information Services。

https://blog.csdn.net/wxl3105/article/details/81067052


选择新建站点，选择处理程序映射—>添加模块映射请求路径：*模块：FastCgiModule可执行文件：xxxxxxxx|xxxxxxxxx    前半部分为python.exe路径，如果配置虚拟环境，则为虚拟环境的python.exe路径    后半部分为inetpub\wwwroot下，待发布的Django项目里wfastcgi.py文件的路径请求限制中，勾选仅当请求映射至以下内容时才调用处理程序，选择文件


==重点为 模块 和 可执行文件 以及 请求限制中 把 仅当请求映射至以下内容时才调用处理程序 的钩去掉==

# 坑1，为静态文件配置 web.config时
==IIS7错误:不能在此路径中使用此配置节。如果在父级别上锁定了该节，便会出现这种情况。锁定是默认设置的(overrideModeDefault=
IIS7错误:不能在此路径中使用此配置节。如果在父级别上锁定了该节，便会出现这种情况。锁定是默认设置的(overrideModeDefault="Deny")==
因为 IIS 7 采用了更安全的 web.config 管理机制，默认情况下会锁住配置项不允许更改。

**解决办法**：cmd.exe要以管理员身份启动，运行命令行 

C:\windows\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers 。
其中的 handlers 是错误信息中红字显示的节点名称。


#坑2
==在唯一密钥属性“name”设置为“StaticFile”时，无法添加类型为“add”的重复集合项==