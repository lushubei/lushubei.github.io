---
layout: post
title: "Git常用命令"
subtitle: 'Git commond'
author: "Shubei"
header-style: text
tags:
  - Git
---
git 常用命令

## git 拉取远程分支到本地
```
git fetch origin test
```

## git 查看所有分支
```
git branch -l
```

## git 切换分支
```
git checkout b1
```

## git 创建本地分支
```
git checkout -b test remotes/origin/test
```

## git merge b1到b2
1. 先进入b2分支
2. 再meger其他分支到当前分支
```
git checkout b2
git merge b1
```

## git 删除本地分支b1
1. 离开b1分支
2. 删除b1分支
```
git checkout b2
git branch -d b1
```

## git 查看当前配置信息

```
git config -l
```

## git 查看本地修改

```
git status
```

## git 添加本地修改

```
git add . --all
```

## git 提交本地修改

```
git commit -m 'bugfix[9883] 解决删除死锁'
```

## git push到远程仓库
   
   ```
   git push
   ```

## git 强行pull并覆盖本地文件
```
git fetch --all  
git reset --hard origin/develop 
git pull
```