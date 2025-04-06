---
title: vulnhub Finding My Friend靶机复盘
author: LingMj
data: 2025-04-06
categories: [vulnhub]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.165	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.082 seconds (122.96 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.165
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-05 22:02 EDT
Nmap scan report for findingmyfriend.mshome.net (192.168.137.165)
Host is up (0.041s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ce:19:b7:da:b3:c5:10:73:a7:43:3c:7e:93:50:74:3d (RSA)
|   256 35:25:f6:bb:df:1d:b6:fd:cd:0b:df:4b:30:14:3d:3b (ECDSA)
|_  256 ac:c6:71:53:6b:b5:4a:0a:3a:85:ae:67:32:5d:e2:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.03 seconds
```

## 获取webshell
![picture 0](../assets/images/2582e28655636ddaecb5f276ba648fab58d364c6d53d8f2ce77f6ce775ea89b3.png)  
![picture 1](../assets/images/7ef6391744d9c2b275a176d94092ab90cbc2bcc6eeba0169bcd09a6211ecd47a.png)  
![picture 2](../assets/images/6dab03c54648bcf8f44e097447ce6c705f84531ac3c4dfa042c8e1c0cbccddd1.png)  
![picture 3](../assets/images/8513b457b36799d6eb634b17052640aac38d13698bc51f9c2431b9fab520d3aa.png)  
![picture 4](../assets/images/780ca1c5a744aea2e9329139daf82f9ba59310d2b3ac6f110a071eec6e89201e.png)  
![picture 5](../assets/images/b0314b203458a84704e321b9d34257e9549261307a08c246d8431c2edd1016d8.png)  
![picture 6](../assets/images/14d1e0efab85d18ec8737f29add25b9947b4232f680f3a9badd9ea16d0cc01fc.png)  
![picture 7](../assets/images/43c0f42645daae2173db506db489f56ddd52426ab97d2e66d5b598964258f41b.png)  

## 提权

![picture 8](../assets/images/a3b4739b65dd8595cb5098fd308471c45ac918c32b793a77cb2239b97fac94ca.png)  
![picture 9](../assets/images/fbf1f4321d21ad7ef54bfb00c31e81d46ed36349cae0e91a49240dc08de79a11.png)  
![picture 10](../assets/images/a6839912e5da2aef14bf805757e00ff4c1b688f978a5a4a2891b0bc8d09b9a09.png)  
![picture 11](../assets/images/2298936f5e81f803d06a4c4f98c43bb6c9992c9e3b410d15811db1baf98ec18b.png)  
![picture 12](../assets/images/d2d4894027de527796facd65d796cd7a240382447ac119f3776a27efe3c075c8.png)  
![picture 13](../assets/images/570c2f659ff94e7d20a5783f50fa7b72e22617fb4aa430b11a41c0f987d745bd.png)  
![picture 14](../assets/images/47703cf79dac3e1729b441b34bc8aeb739c2e365240c6fd2db1fb69431d1b5e9.png)  
![picture 15](../assets/images/97da73d068b58c168e129e42a3999fc64e771da16f2ab60902508e0ff7f65cd9.png)  



>没啥好说的都是研究过的知识，整体是个low只不过看花多长时间，我花时间挺长预期之外
>