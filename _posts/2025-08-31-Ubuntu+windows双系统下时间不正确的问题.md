---
title: "Ubuntu+windows双系统下时间不正确问题"
date: 2025-08-31 11:33:00 +0800
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories: Ubuntu Windows TimeSettings
---
### 问题描述：
在同一台电脑上安装ubuntu+windows双系统时，会出现某个系统的时间不正确的问题，而由于windows同步时间实在是太慢了，如果不去解决，windows上的时间大概率一直都是不对的。

问题分析：
电脑安装了windows/ubuntu双系统，每次从Ubuntu切换到Windows系统，都会发现Windows系统的时间显示不对，比实际时间慢8个小时。上网查询后了解到是由于两个系统对时间的识别方式不同造成的，Ubuntu 默认硬件时间为UTC（Coordinated Universal Time）即协调世界时，中国时间为UTC+8；而Windows则认定硬件时间为系统时间。这就造成了当先开启Ubuntu系统时，系统从网络得到本地时间例如为8点钟，然后其修改硬件时间为0点，再次启用Windows时，Windows读取硬件时间为本地时间，这就造成了系统显示时间比实际时间慢8小时的问题。

### 解决方法：
方法一：尝试在Linux上修复这个问题
1. 在Ubuntu终端输入如下指令：
```bash
sudo apt install ntpdate
```
2. 同步时间：
```bash
sudo ntpdate time.windows.com
```
3. 如果没有安装util-linux-extra，安装util-linux-extra：
```bash
sudo apt install util-linux-extra
```

4. 将系统时间同步机制由UTC改为与Windows一样的LocalTime
```bash
sudo hwclock --localtime --systohc
```

参考链接：
[完美解决ubuntu+windows双系统下时间不正确问题](https://blog.csdn.net/qq_42475234/article/details/136275280)
