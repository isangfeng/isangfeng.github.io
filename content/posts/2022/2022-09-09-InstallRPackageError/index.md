---
title: R包安装报错
author: 桑峰
date: '2022-09-09'
categories:
  - 系统/工具
tags:
  - R
  - cxx
format: hugo-md
---


# Error: C++14 standard requested but CXX14 is not defined

参考：https://www.zxzyl.com/archives/1283/

升级gcc，并设置r包编译使用的gcc路径。

设置方法：

首先在～/.R目录下新建文件Makevars；

将
