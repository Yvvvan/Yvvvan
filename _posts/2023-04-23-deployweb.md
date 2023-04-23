---
layout: post
title: "用Docker部署Pantheon"
subtitle: ""
date: 2023-04-23
author: "Yvan"
header-img: "img/bg/wp-bg.jpg"
header-mask: 0.3
header-img-credit: "sheavi from Wallhaven"
header-img-credit-href:  "https://whvn.cc/426qqy"
no-catalog: false
mathjax: true
category: "code"
tags:

- Docker
- 前端
- 后端
- Html
---

1. Dockerfile修改
   
   1. 根据计算机架构的不同 应该修改alpine的版本
      
      在树莓派上部署需要将amd64改为arm64
      
      如果忘记更改会导致Dockerfile第39行开始RUN apk ...的报错
   
   2. localhost修改

2. 将大多数localhost修改成正确地址

3. 使用PGAdmin连接到数据库
