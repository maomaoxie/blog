---
title: Git 常用的指令
date: 2022-02-21 17:14:05
tags:
- git
- github
categories: git
---
這裡記錄一些工作上會用到的 git 指令，太容易忘記了阿（扶額

# 分支
### 查看分支
``` bash
git branch
```

# 遠端 URL
### 更換遠端 URL
因應 PAT 新制，remote origin 格式必須更改如下：
``` bash
git remote set-url origin https://<USERNAME>:<TOKEN>@<GIT_URL>.git
```
### 查看遠端 URL
``` bash
git remote -v
```
### 將專案推上去遠端 URL
``` bash
git push -u origin master
```