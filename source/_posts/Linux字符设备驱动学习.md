---
title: Liunx驱动学习
date: 2017-09-16
categories: 
tags: [linux,驱动,tag3] 
toc: true
---

 

### 0.前言

​	在Linux2.6版本之后注册字符设备有新的接口了，新的接口使用dev_t 类型来描述设备号，dev_t 是32位的数值类型，其中高12位表示主设备号，低20位表示次设备号。

`typedef __kernel_dev_t	dev_t;`

`typedef __u32 __kernel_dev_t;`

<!-- more -->
### 1.获取设备号

- Linux内核中使用两个宏定义来获取主设备号和从设备号

  - MAJOR

    `#define MINORBITS	20`

    `#define MAJOR(dev)	((unsigned int) ((dev) >> MINORBITS))`

  + MINJOR

    `#define MINORMASK	((1U << MINORBITS) - 1)`

    `#define MINOR(dev)	((unsigned int) ((dev) & MINORMASK))`





















