---
title: VulnVM Corrosion靶机复盘
author: LingMj
data: 2025-03-29
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
192.168.137.72	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.172	62:2f:e8:e4:77:5d	(Unknown: locally administered)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.107 seconds (121.50 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.72 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-28 20:22 EDT
Nmap scan report for corrosion.mshome.net (192.168.137.72)
Host is up (0.0063s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Ubuntu 5ubuntu1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 0c:a7:1c:8b:4e:85:6b:16:8c:fd:b7:cd:5f:60:3e:a4 (RSA)
|   256 0f:24:f4:65:af:50:d3:d3:aa:09:33:c3:17:3d:63:c7 (ECDSA)
|_  256 b0:fa:cd:77:73:da:e4:7d:c8:75:a1:c5:5f:2c:21:0a (ED25519)
80/tcp open  http    Apache httpd 2.4.46 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.46 (Ubuntu)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.19 seconds
                                                               
```

## 获取webshell
![picture 0](../assets/images/a9f8a8fa2040dec1d969f064e625c322ff6754b3236bce55b3bb05cccd3cb240.png)  

>这个靶机刚在这个平台出的时候就打了，一直没复盘不过时间有点久了忘了重新打是一个Low-Easy的靶机
>

![picture 1](../assets/images/bd25879e7042ac4274a14e73caf4ad2ef155e3dc7f228f50587edfc5e73b6572.png)  
![picture 2](../assets/images/7943f119f92566f563c5de0a58d80216b7bcf6f1ca2928b7495291975af170fb.png)  
![picture 3](../assets/images/c2e178ada9810fbd4efbf3c733aee710079e104c099a812e96d935063e7da495.png)  
![picture 4](../assets/images/6310e685b6919a4193af0c4cecdc3c493c6106eecd5af8e410c79ff468acb31e.png)  

>直接php-filter了
>

![picture 6](../assets/images/515930343d77c01d73fc4d0a9fe077b14e2ab50bd0977f8ec2db4d0fcf02bf6e.png)  

![picture 5](../assets/images/bb5d1bf96d5c8236c284b9108fd54b02991cb56ca7629b699d845ba3967574c6.png)  
![picture 7](../assets/images/410102cdae79a55108b9f4f01afb6ac4fa528a87ebb86fa7578aa6699092a72c.png)  



## 提权
![picture 8](../assets/images/bedb6759e3cfeaf1417ce951435a7d0c279291913bd9dd0e5d67ddf942594ee1.png)  
![picture 9](../assets/images/f44822a65e612b25f632368339e203f85c778346a2f948632bb3e225651ec4be.png)  
![picture 10](../assets/images/568c4003325a8b1697f4621800e56cfd26f28292ed8acdc318f3753d0a23ba91.png)  
![picture 11](../assets/images/acb2c847e3a545d8a62511ea907ae6df06ca3217e1495d2cd9b2ebc6cc0ae914.png)  
![picture 12](../assets/images/2ab935efe568b30b2579515e5d36926ddefcf907d46268abb75ec6a9b18b5d36.png)  
![picture 13](../assets/images/e4d728702be019851d767fbd03189bb56a25e41753010ac96b82925ee7214878.png)  

>不用多说直接王炸就好了
>

![picture 14](../assets/images/97d6ecb1673bc0d42f87e1a43a19388b85a65bb2d038d2437a2f769729b25b46.png)  

>结束了
>

>userflag:98342721012390839081
>
>rootflag:4NJSA99SD7922197D7S90PLAWE 
>