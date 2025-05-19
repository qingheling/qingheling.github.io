---
title: VulNyx Denied靶机复盘
author: LingMj
data: 2025-05-18
categories: [VulNyx]
tags: [doas, ssh]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.144	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.044 seconds (125.24 hosts/sec). 3 responded
```

## 端口扫描

```
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
```

## 获取webshell

>前面跑了一个点的字典没有正确应该是ssh用户匹配，大佬的发现
>
![picture 0](../assets/images/f81e78e0acaeccbef1e10e266b46bc7b242848ca9aa6a41c95c579613b9e1243.png)  
![picture 1](../assets/images/acf1ece9b9edda5b94e4128e0eb53b7f66f268a5c17037becb75fa1975860a81.png)  
![picture 2](../assets/images/7e59a08b5e27aebe5d569594e743f1a92640ebc325bdc28ac0fa949ab995f98c.png)  

>这里交获取ssh
>


## 提权

![picture 3](../assets/images/32677a2c94294857d703a34a380f8e115ca19bc19ec624dabc2a568eb599192a.png)  
![picture 4](../assets/images/1d8672be30293d34246642ff4354954f4f24d17ecaf62f10fb447bbc6728d5e8.png)  
![picture 5](../assets/images/f33eab8f5385f3c5741ba1c221088f1a7fc87eeddb1f62665c910cf35441a6e9.png)  

>里面没有还是得爆破
>

![picture 6](../assets/images/7db9bcb7ad111849c653d3103c164e4c5361cb53c63a5a3531b7daff3ff40231.png)  
![picture 7](../assets/images/ab62c017155616c1494985f9a07334c2f2e97d9e06d4f57a6f37cae421e6cec2.png)  
![picture 8](../assets/images/fbadf4936890eb882699fcb0d0a4f8bbdafffe3a7770812cb4a4b036b25bf9c7.png)  

>好了结束了，全是大佬思路
>

>userflag:
>
>rootflag:
>
