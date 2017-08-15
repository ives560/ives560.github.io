---
layout:     post
title:      "树莓派ap配置"
subtitle:   ""
date:       2017-08-02 12:00:00
author:     "Ives"
header-img: "img/in-post/post-eleme-pwa/eleme-at-io.jpg"
header-mask: 0.3
catalog:    true
tags:
- 树莓派
- Wifi
---

# 一、安装hostapd和dnsmasq软件包

`sudo apt-get install dnsmasq hostapd`

# 二、接口配置
## 1.dhcpcd配置文件
`sudo vim /etc/dhcpcd.conf`
并在文件的最后一行添加以下内容：
`denyinterfaces wlan0 `
## 2.interfaces配置文件
`sudo vim /etc/network/interfaces`编辑wlan0部分内容，他看起来像这样：
```shell
allow-hotplug wlan0 

iface wlan0 inet static 
    address 192.168.10.1
    netmask 255.255.255.0
    network 192.168.10.0
    broadcast 192.168.10.255

#    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```
## 3.配置HOSTAPD
创建一个新的配置文件`vim /etc/hostapd/hostapd.conf`,然后加入以下内容：
```shell
interface=wlan0    # This is the name of the WiFi interface we configured above

driver=nl80211    # Use the nl80211 driver with the brcmfmac driver

ssid=RaspberryPi3    # This is the name of the network

hw_mode=g    # Use the 2.4GHz band

channel=6    # Use channel 6

ieee80211n=1    # Enable 802.11n

wmm_enabled=1    # Enable WMM

ht_capab=[HT40][SHORT-GI-20][DSSS_CCK-40]    # Enable 40MHz channels with 20ns guard interval

macaddr_acl=0    # Accept all MAC addresses

auth_algs=1    # Use WPA authentication

ignore_broadcast_ssid=0    # Require clients to know the network name

wpa=2    # Use WPA2

wpa_key_mgmt=WPA-PSK    # Use a pre-shared key

wpa_passphrase=raspberry    # The network passphrase

rsn_pairwise=CCMP    # Use AES, instead of TKIP
```

打开默认的配置文件`vim /etc/default/hostapd`并找到`#DAEMON_CONF=""`将他替换成这`DAEMON_CONF="/etc/hostapd/hostapd.conf"`

## 4.配置DNSMASQ
```shell
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf_bak

sudo vim /etc/dnsmasq.conf 
```

复制粘贴下面的内容进新的文件：
```shell
interface=wlan0      # Use interface wlan0 

listen-address=192.168.10.1 # Explicitly specify the address to listen on 

bind-interfaces      # Bind to the interface to make sure we aren't sending things elsewhere 

server=8.8.8.8       # Forward DNS requests to Google DNS 

domain-needed        # Don't forward short names 

bogus-priv           # Never forward addresses in the non-routed address spaces. 

dhcp-range=192.168.10.2,192.168.10.254,12h # Assign IP addresses between 192.168.10.2 and 192.168.10.254 with a 12 hour lease time 
```

# 参考地址
[使用HOSTAPD使你的树莓派3成为WiFi热点（AP）](http://www.embeddedlinux.org.cn/emb-linux/entry-level/201703/18-6294.html "使用HOSTAPD使你的树莓派3成为WiFi热点（AP）")

[树莓派Pi3设置wifi热点](http://www.jianshu.com/p/1fca72a710d5 "树莓派Pi3设置wifi热点")
