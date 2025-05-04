---
title: hackmyvm Disguise靶机复盘
author: LingMj
data: 2025-05-04
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.224	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.111 seconds (121.27 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.224     
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-03 22:00 EDT
Nmap scan report for disguise.mshome.net (192.168.137.224)
Host is up (0.0074s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u4 (protocol 2.0)
| ssh-hostkey: 
|   2048 93:a4:92:55:72:2b:9b:4a:52:66:5c:af:a9:83:3c:fd (RSA)
|   256 1e:a7:44:0b:2c:1b:0d:77:83:df:1d:9f:0e:30:08:4d (ECDSA)
|_  256 d0:fa:9d:76:77:42:6f:91:d3:bd:b5:44:72:a7:c9:71 (ED25519)
80/tcp open  http    Apache httpd 2.4.59 ((Debian))
|_http-generator: WordPress 6.7.2
|_http-server-header: Apache/2.4.59 (Debian)
|_http-title: Just a simple wordpress site
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.48 seconds
```

## 获取webshell
![picture 0](../assets/images/de3d4e055a4448a051b758613af8b6cfc25630122a1cff04f52e11ca7601ba00.png)  
![picture 1](../assets/images/cc4acd692843d63dcbbf7e4d956772d2baae20239eff2b1c7f0037043c158fe0.png)  
![picture 2](../assets/images/9e0826ab70baf268cd6bdc3c7323642f4403d3bb0dd0889ec686d663e8fa11c4.png)  

>wordpress的需要找到密码，而且需要域名
>

![picture 4](../assets/images/803caf9b68272f4b540dceb4d519e44516e49bb429093bc08106970b7427efc9.png)  

![picture 3](../assets/images/4f23799b8a1d1ecc621c94e81d266b028d63eef58bd0cc220dd2b377113fc954.png)  
![picture 5](../assets/images/0d134e21627494bdf717df7b25e2b9a5c2576ba846c5638d081e8a6d77bd4d6f.png)  
![picture 6](../assets/images/92cad7e4ebae7a9c431d20cc71397cc73ccc6ec84e5ee9a5ffdee503d271d99f.png)  
![picture 7](../assets/images/5ea2ed874fb52433488bc27668d324c34737e86fd784f2c4767b59916677ba22.png)  
![picture 8](../assets/images/dd2e1fc14ada5dc3b590c048a6ac25ef7bfd86ea3cae692b17dcfbdd7ba90c47.png)  

>查子域名
>

![picture 10](../assets/images/d061679a17d2fdabf03e3fd2e547687a6e60e1d24229abe7e8b4243a7b447e53.png)  

![picture 9](../assets/images/dcdc091ea3a3ea8f53790e7121ba5bb3021c30942114c25e934cc5d193cb6be1.png)  




## 提权



>userflag:
>
>rootflag:
>