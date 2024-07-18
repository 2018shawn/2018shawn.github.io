---
title: "Git代理设置"
date: 2024-07-28 10:56:00 +0800
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories: linux bluetooth
---

# Win10 + Ubuntu 19.10 双系统自动连接相同蓝牙设备 

## 0x00 技术原理
通常情况下双系统的蓝牙地址并不会改变，但连接信息（电脑与蓝牙设备交换的密钥）不同，导致双系统无法自动连接蓝牙设备，切换系统后需要删除设备后重新连接

本文通过修改 Windows 下蓝牙设备的连接信息使双系统均可自动连接蓝牙设备

## 0x01 环境
OS 1: Windows 11 专业版 

OS 2: Arch Linux 6.9.9-arch1-1

蓝牙设备: 小米小钢炮蓝牙音箱

通常情况下双系统的蓝牙地址并不会改变，但连接信息（电脑与蓝牙设备交换的密钥）不同，导致双系统无法自动连接蓝牙设备，切换系统后需要删除设备后重新连接

本文通过修改 Windows 下蓝牙设备的连接信息使双系统均可自动连接蓝牙设备

## 0x02 获取蓝牙设备信息
首先在 Ubuntu 下连接蓝牙设备，并访问如下目录

/var/lib/bluetooth/80:XX:XX:XX:XX:3C/74:XX:XX:XX:XX:CB
其中 80:XX:XX:XX:XX:3C 为电脑蓝牙地址

74:XX:XX:XX:XX:CB 为音箱蓝牙地址

在该目录中执行 cat info 打印蓝牙设备信息，格式如下
```
[General]
Name=小米小钢炮蓝牙音箱
Class=0x240404
SupportedTechnologies=BR/EDR;
Trusted=true
Blocked=false
Services=00001108-0000-1000-8000-xxxxxxxxxxxx;0000110b-0000-1000-8000-xxxxxxxxxxxx;0000110c-0000-1000-8000-xxxxxxxxxxxx;0000110d-0000-1000-8000-xxxxxxxxxxxx;0000110e-0000-1000-8000-xxxxxxxxxxxx;0000111e-0000-1000-8000-xxxxxxxxxxxx;

[LinkKey]
Key=33 46 xx xx xx xx xx xx xx xx xx xx xx xx 5B E9
Type=4
PINLength=0
```

记录 [LinkKey] 块中的 Key 值，并切换到 Windows

## 0x03 修改 Windows 中的蓝牙连接信息
在 Windows 中连接蓝牙设备，并下载 [PsExec](https://learn.microsoft.com/en-us/sysinternals/downloads/psexec), 使用管理员身份运行 cmd，执行 PsExec.exe -s -i regedit

修改 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys\80XXXXXXXX3C\74XXXXXXXXCB` 的值为上一步记录下的 Key 值

## 0x04 archlinux 设置自动连接蓝牙音箱
将 `/etc/bluetooth/main.conf` 最后的 AutoEnable 值修改为 true

参考链接:
1. [Win10 + Ubuntu 19.10 双系统自动连接相同蓝牙设备](https://www.leviatan.cn/archives/26/)
2. [在archlinux中使用蓝牙耳机](http://blog.lujun9972.win/blog/2017/07/18/%E5%9C%A8archlinux%E4%B8%AD%E4%BD%BF%E7%94%A8%E8%93%9D%E7%89%99%E8%80%B3%E6%9C%BA/)