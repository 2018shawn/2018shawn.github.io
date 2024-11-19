---
title: "Linux上添加grub引导项"
date: 2024-11-19 10:27:00 +0800
top: false
cover: false
password:
toc: true
mathjax: true
summary:
tags:
categories: linux grub
---

由于我想在linux启动引导项里添加Windows 11的启动引导项，所以有了这篇博客。
   
下面我将一步步添加引导项：

## 1. 编辑grub配置
- 在终端执行以下命令行：

    `sudo vim /etc/default/grub`

    然后，在这个文件底部找到`#GRUB_DISABLE_OS_PROBER=false`取消这行代码的注释

- 终端执行以下命令行，使grub配置生效:
    grub-mkconfig -o /boot/grub/grub.cfg

## 2. 查看Windows引导分区的uuid：
打开终端执行如下命令：

`sudo fdisk -l`

得到如下信息：

```
Disk /dev/sda：931.51 GiB，1000204886016 字节，1953525168 个扇区
磁盘型号：ST1000LM035-1RK1
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 4096 字节
I/O 大小(最小/最佳)：4096 字节 / 4096 字节
磁盘标签类型：dos
磁盘标识符：0x88a56b29

设备       启动  起点       末尾       扇区   大小 Id 类型
/dev/sda1        2048 1953521663 1953519616 931.5G  7 HPFS/NTFS/exFAT


Disk /dev/nvme1n1：931.51 GiB，1000204886016 字节，1953525168 个扇区
磁盘型号：Samsung SSD 980 PRO 1TB                 
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：gpt
磁盘标识符：8DF46D04-9ADE-4DAA-BD92-922D09A81730

设备                起点       末尾       扇区   大小 类型
/dev/nvme1n1p1      2048    1640447    1638400   800M EFI 系统
/dev/nvme1n1p2   1640448  211355647  209715200   100G Linux 文件系统
/dev/nvme1n1p3 211355648 1953523711 1742168064 830.7G Linux 文件系统


Disk /dev/nvme0n1：1.86 TiB，2048408248320 字节，4000797360 个扇区
磁盘型号：aigo NVMe SSD P7000Z 2TB                
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：gpt
磁盘标识符：E3BE8B22-6A05-40A8-971A-5613DE5A8EC2

设备                 起点       末尾       扇区   大小 类型
/dev/nvme0n1p1       2048     421478     419431 204.8M EFI 系统
/dev/nvme0n1p2     421888     454655      32768    16M Microsoft 保留
/dev/nvme0n1p3     466944 1048130848 1047663905 499.6G Microsoft 基本数据
/dev/nvme0n1p4 1049655296 4000793740 2951138445   1.4T Microsoft 基本数据
```

可以看到/dev/nvme0n1p1就是Windows的引导分区，使用以下命令获取其UUID

`sudo blkid /dev/nvme0n1p1`

其输出信息如下，将引导分区的UUID复制下来，这里是`E251-1FC0`：

`UUID="E251-1FC0" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI system partition" PARTUUID="2dc3f483-020c-11ee-8036-d8bbc16ded90"`

## 3. 对/boot/grub/grub.cfg进行修改
打开终端执行如下命令：

`sudo vim /boot/grub/grub.cfg`

找到`/etc/grub.d/30_os-prober`

```
### BEGGIN /etc/grub.d/30_os-prober ###
### END /etc/grub.d/30_os-prober ###
```

将以下内容添加到这两行中间

```
memuentry 'Microsoft Windows 11' {
    insmod part_gpt #磁盘格式
    insmod fat 
    insmod chain
    search --fs-uuid --no-floppy --set=root XXXX-XXXX  #前面找到的UUID替换掉这些XXX。
    chainloader (${root})/EFI/Microsoft/Boot/bootmgfw.efi #注意大小写
}
```
保存退出，重启系统。Done！

参考文档：

1. [[arch Linux] grub引导Linux和Windows双系统](https://blog.csdn.net/weixin_43801670/article/details/125106208)
2. [ArchLinux设置grub引导](https://blog.csdn.net/Roger_Spencer/article/details/134348620)
