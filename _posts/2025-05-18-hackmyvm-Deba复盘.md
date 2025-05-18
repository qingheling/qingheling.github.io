---
title: hackmyvm Deba靶机复盘
author: LingMj
data: 2025-05-18
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.42	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.049 seconds (124.94 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sV -sC 192.168.137.42
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-18 07:14 EDT
Nmap scan report for debian.mshome.net (192.168.137.42)
Host is up (0.043s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 22:e4:1e:f3:f6:82:7b:26:da:13:2f:01:f9:d5:0d:5b (RSA)
|   256 7b:09:3e:d4:a7:2d:92:01:9d:7d:7f:32:c1:fd:93:5b (ECDSA)
|_  256 56:fd:3d:c2:19:fe:22:24:ca:2c:f8:07:90:1d:76:87 (ED25519)
80/tcp   open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
3000/tcp open  http    Node.js Express framework
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.47 seconds
```

## 获取webshell

![picture 0](../assets/images/b161d350ef4c2dddcf4a01e539c4fdb437c4b7d2322b9a17dafc8c95b09a6a67.png)  
![picture 1](../assets/images/ca30382486793832357fd0f5bf3234e19d47f88cba522512f476492ac7a8e077.png)  

>没收获
>

## 提权



>userflag:
>
>rootflag:
>
