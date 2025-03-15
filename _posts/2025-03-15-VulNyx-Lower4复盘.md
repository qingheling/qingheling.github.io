---
title: VulNyx Lower4靶机复盘
author: LingMj
data: 2025-03-15
categories: [VulNyx]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.93	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.100	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.081 seconds (123.02 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.100
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-15 08:33 EDT
Nmap scan report for lower4.mshome.net (192.168.137.100)
Host is up (0.0085s latency).
Not shown: 65532 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
|_auth-owners: root
| ssh-hostkey: 
|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)
|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)
|_  256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)
80/tcp  open  http    Apache httpd 2.4.56 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.56 (Debian)
113/tcp open  ident?
|_auth-owners: lucifer
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 109.24 seconds
```

## 获取webshell

![picture 0](../assets/images/2a4a20b769b6234dc7edc9fc22046020f298fdec40b79e0da9745bfdcb54d565.png)  
![picture 1](../assets/images/8714ba94791c5c2a59198f085e1a916aa7c0c03ecc14a5f01fefbb8b3caad458.png)  

>就找用户名
>

![picture 2](../assets/images/6c065907ec6ef24b733c00f1d00532c423c5d403852c6285856d95c25f99a1e4.png)  
![picture 3](../assets/images/a0a752f472d7f06fce91bc9daeb88a05ccc1dec906d96ab38f63ad508924af2b.png)  
![picture 4](../assets/images/ed940a49071550c3d29df4e6bb6bde7ceec769a595ad4b29b8fcf9c2eef1707c.png)  

## 提权

![picture 5](../assets/images/9bdd9cba273c3d30f7a96b3f27a88c3c9e5016f54d50cf9dd662362c500dd25b.png)  

>具有任意文件读取，并且可以注入命令
>
![picture 6](../assets/images/09a45192fc3da42d0566dc42a626568d322573c2db7b4ddff71c716fc81cd56f.png)  
![picture 7](../assets/images/a7ae20e4efdcf4954808a9ddbe29e8325d77b81f019a193771acb91da8d7d526.png)  

>正常这里已经结束了，我来说说命令注入
>
![picture 8](../assets/images/d72ef5fc75da83d94376c66511bf48bef45b4c2868b325d6f5fa1af40f5be43c.png)  

>当输入就已经出发所以如果你跑出来已经是root，但是习惯按q或者crtl + c｜z会忘记
>


>userflag:8e99e9f5a7d2d7a067314e34d9fd957f
>
>rootflag:c07db370f9e16dcde97d554b38c9c08e
>