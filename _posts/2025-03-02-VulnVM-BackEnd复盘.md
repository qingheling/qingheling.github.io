---
title: VulnVM BackEnd靶机复盘
author: LingMj
data: 2025-03-02
categories: [VulnVM]
tags: [jenkins,socat,java]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~# arp-scan -l   
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.66	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.147	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.063 seconds (124.09 hosts/sec). 3 responded
```

## 端口扫描
```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.147
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-02 04:07 EST
Nmap scan report for backend.mshome.net (192.168.137.147)
Host is up (0.037s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:ec:8d:c2:a6:1e:52:43:62:44:29:36:58:73:15:6b (RSA)
|   256 0d:39:f5:86:a1:fc:7d:ba:c6:55:14:37:2c:91:fe:37 (ECDSA)
|_  256 d6:91:b0:62:48:85:9c:51:dd:f9:20:35:d2:53:a6:25 (ED25519)
8080/tcp open  http    Jetty 10.0.18
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
|_http-server-header: Jetty(10.0.18)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.40 seconds
```

## 获取webshell

![picture 0](../assets/images/8ddd764fffa3e8f10fa6b5c8f4b851d3648310b403714c5b208b2fffc41e358e.png)  
![picture 1](../assets/images/5b0f6b9d246d6a6be3770781aa80b79d89af5239c19062e7008a0ef6c880ff3a.png)  

>这里的版本是有LFI漏洞，但是有问题它只能读取一行，我看了一篇关于这个的文章：https://www.leavesongs.com/PENETRATION/jenkins-cve-2024-23897.html
>

![picture 2](../assets/images/8698c6ef2e5e3f90cdf79710d6488835d2429f99bd334b0aee4ed7c2d9e4751a.png)  

>我得到的结论是我们被匿名关闭了，并且加了权限设置，所以我们如果要突破必须进行密码读取，这里我找到了密码的明文路径：/secrets/initialAdminPassword，用户admin
>

![picture 3](../assets/images/35329f4bb79ac9294d4ebac0286c7e7c7660c8b34fe5d5f03bc0e9f0a640ba4c.png)  
![picture 4](../assets/images/20acbd907ed6b673af09ec72dde7e4a1e90e4178a98b8b8a3bf75c1992b93541.png)  
![picture 5](../assets/images/44910b5f841902d3cb0dfcc70d1534d687c08fc6c5cacc4e38e0ad000fa4e147.png)  
![picture 6](../assets/images/96b4d86f52c574755a6043bec24c79c2b85100bf2eda3ac38bfcd6a120b57614.png)  
![picture 7](../assets/images/a51c59abe2e4d67cec9579318d60bdc168d179e1ba9f7ffb6758f6a931eef219.png)  

>我后面直接给自己加ssh省得调终端
>

## 提权

![picture 8](../assets/images/772a056d8e8ba6ff47eaec893e6353cdf2287fc2211447fb3f7354e8327ef3a8.png)  
![picture 9](../assets/images/881d46be558c4f397b85c6026c5ced26fcde9b28df5e01c95ef5ca1e67b65f01.png)  

![picture 10](../assets/images/8f82a615b5d7f97f9f504c86f720647a45b725fd31cc232d6d40d6ecba1f6424.png)  

>需要进行端口转发
>

![picture 11](../assets/images/9f7caf80e28516f5a36de4bfb855877be6e145f86fe9fe00f7c84347e8edcd72.png)  
![picture 12](../assets/images/a22384baff037d6d6ef575b3d444d616f6e98450bc8b4700faa24ca360f90f6e.png)  
![picture 13](../assets/images/eb08f607a86c007b964412b45649a725e8379ea6a8d96b42131a16c26fae686f.png)  
![picture 14](../assets/images/b3e999c3e3ed62330cc7d8710e272530b079b97d2a8562651af8b83a3ebc3a0f.png)  
![picture 15](../assets/images/5c21f68c72f41195d0572ae3c4450c641d0e3752d5495f29317e57f299b3d1ec.png)  
![picture 16](../assets/images/320c8e970bba1a6939e44e470be67a9da23794d2bbc3677c53babefa91f40e9d.png)  
![picture 17](../assets/images/b3ad05be011e117d75fcc9c9fb1dc5a41920fdc42acba1d5b0519f46e29d50be.png)  
![picture 18](../assets/images/f6da1da3042894b22b2f1a3e91df0cdb791b5cdc6cfd574fc7799b7153581c3f.png)  

>无密码登录，OK
>

![picture 19](../assets/images/e4d809ee04b5e34c41cdcc991c70741798d94be0441c0632c38e52a07cd06716.png)  

>到这里结束了本来我以为我写个java脚步运行就成功了，但是脚步失败还是最朴素的方案吧
>

![picture 20](../assets/images/c7f300efb0183b8f452f730304bac7c269d503c97568e3d2fe4e00daad308be1.png)  
![picture 21](../assets/images/5355440ddaa5ffb274839df6f412d4ccb83c1b17da030664bcb2920fbba71037.png)  
![picture 22](../assets/images/dfdd725f64da04e713abde8a773b3a58aaebc42a8217dc2182a44b8427164291.png)  


>好了靶机到这里就结束了，整体来说是一个easy的靶机后面，前面可能费点劲
>

>userflag:bd78ee****98ae0570388c****d17e58
>
>rootflag:37589**c75122**c17af**17467294f2
>