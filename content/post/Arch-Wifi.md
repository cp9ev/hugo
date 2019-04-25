---
author: "c0ldpure"
title: "Arch Wifi"
# linktitle: Archlinux 实现WIFI开机自动连接
description: "Arch"
date: 2019-04-25T12:43:04+08:00
draft: true
tags: ["Arch"]
---

在按照[ArchLinux Install guide](https://wiki.archlinux.org/index.php/Installation_guide)之后全新的Arch是非常干净的,需要针对个人做一些设置.首当其冲的就是网络问题,默认是没有wifi客户端的,需要你手动进行配置和安装,这时你又没有网络无法使用包管理器pacman安装wpa_supplicant和dhcpd就会非常尴尬.  
这里按照我的解决方式,使用Arch安装的启动盘环境，默认是带有基本wifi包iw和wpa_supplicant的，插上U盘进入Arch安装启动盘:  
`# wpa_supplicant -B -i interface -c <(wpa_passphrase essid passphrase)`  
interface 可以使用`# ip link`命令查看，一般为(wlan0, wlp2s0)   
`# dhcpcd interface`  通过dhcpcd分配地址即可连接网络  
`# mount /dev/sdX /mnt`  
`# arch-chroot /mnt`  
这时再切换到本地系统当中，使用pacman -S wpa_supolicant dhcpcd 安装完毕后重启再进入系统即可使用

#### 如何设置开机自动启动
正常在启动之后再通过刚才的wpa_supplicant和dhcpcd命令之后是可以连接网络的，下面是如何配置开机自动连接网络的方法：
netctl 是 Arch 官方开发的网络管理工具 
dialog 是 wifi-menu的依赖包项目  
`# pasman -S dialog netctl wifi-menu`  
这里建议重启一遍之后再操作：  
首先使用`# wifi-menu` 按照提示连接一遍网络
按照提示会讯问你是否保留wifi的配置文件，选择是  
查看 /etc/netctl/ 目录，确认配置文件是否存在  
#### 启动netctl 服务
`systemctl start netctl-auto@wlp4s0.service`  
`systemctl enable netctl-auto@wlp4s0.service`  
这时再重启login之后就应该是自动连接对应网络了






