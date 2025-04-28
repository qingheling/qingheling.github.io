---
title: VulnVM GET靶机复盘
author: LingMj
data: 2025-04-28
categories: [VulnVM]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.135	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.149	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.251	3e:21:9c:12:bd:a3	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.035 seconds (125.80 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.251 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-27 20:59 EDT
Nmap scan report for 192.168.137.251
Host is up (0.015s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 69:dc:67:49:10:2a:a4:26:a8:9f:c4:5d:a3:b8:a1:3e (ECDSA)
|_  256 6a:2b:e4:44:29:78:62:fb:61:0b:09:2f:9c:bc:18:c6 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.64 seconds
                                                               
```

## 获取webshell

![picture 0](../assets/images/860830b21cdea6630f44b6b713c5e85acaeec0a681d12812855d7fd9837fc4e9.png)  
![picture 1](../assets/images/e27c31f0644fc7de9030428a5013261b5da59fada99a470dbe2399ad50780465.png)  
![picture 2](../assets/images/f8d51a0deee64206f93896132683b0db92bbd16ea068f407ca84ddf9c95feeb4.png)  
![picture 3](../assets/images/3d59c8d29f25ea4ed5d0bf199048d9bb75e754ad5da9a921672773152171250f.png)  

>看看是不是LFI
>

![picture 4](../assets/images/8c5edb47cb66ae9b1d5b0a89b12e6b03aa32b439f2ad86a7b037398b6e393265.png)  
![picture 5](../assets/images/5c0f36fd5428e471cfbff3f09b476f2b44601c5a0e3736d42a98d02876e7720e.png)  

>要秒了没意思
>

![picture 6](../assets/images/35bab3c64b170454225d8696b8f28e0d1894400ee2cb2559a25a3dde2a4739b7.png)  


## 提权

![picture 7](../assets/images/1cd1c313a0840a23a8d0d7a693af5a39dbec22053e5879c8826a3cfef33df4bc.png)  
![picture 8](../assets/images/7a9555b1beb471db62bbf438a0f3ab1c9cfce08e2ee76faeeede8f0c6f3fae73.png)  
![picture 9](../assets/images/7fce0a5a09b33573b96ae75153455169eaa3b69fc0256f922472230fbc3737b9.png)  
![picture 10](../assets/images/733d01191d81e32740a825bef8223a9f262206f33ff171ae3814262d4be3bd4d.png)  
![picture 11](../assets/images/537d9d449be7203a665321c685da6caecdc72f6ab7126665db86633ed3968582.png)  

>10分钟靶机吧我用一下fuzz了，真水不如LingMj的靶机，哈哈哈哈哈哈哈哈哈哈
>





>userflag:e90bf53819288566309a9a2757ebaa5c
>
>rootflag:6cc1e28198647796d8b450542d1847ff
>