---
title: VulNyx Ober靶机复盘
author: LingMj
data: 2025-05-04
categories: [VulNyx]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.127	3e:21:9c:12:bd:a3	(Unknown: locally administered)
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-04 07:25 EDT
Nmap scan report for ober.mshome.net (192.168.137.127)
Host is up (0.0062s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 27:21:9e:b5:39:63:e9:1f:2c:b2:6b:d3:3a:5f:31:7b (RSA)
|   256 bf:90:8a:a5:d7:e5:de:89:e6:1a:36:a1:93:40:18:57 (ECDSA)
|_  256 95:1f:32:95:78:08:50:45:cd:8c:7c:71:4a:d4:6c:1c (ED25519)
80/tcp   open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Page not found
|_http-server-header: Apache/2.4.38 (Debian)
8080/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Site doesn't have a title (text/html).
|_http-open-proxy: Proxy might be redirecting requests
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.11 seconds
```

## 获取webshell
![picture 0](../assets/images/5f494a739b509efc56b772504e3700c9c3efcaf958fcf0c02fd5cbf0940da4c6.png)  
![picture 1](../assets/images/5ca7c1e9a7c383c920e144c8424c800bf604c90b3750b0cc209e7af841f3609c.png)  
![picture 2](../assets/images/96fa7195c2cf96bd95babf55cde4ba9d76ee3d24c06ca43043aaa940a2860fd1.png)  

>admin：admin
>

![picture 4](../assets/images/316695d4e421dc39f1ab0d0b330f781f0312c3a20e8e5551b5fcaa3cf0cb8ff4.png)  

![picture 3](../assets/images/4ade4e15544980117c9dbfe1f6b0ff93583f62122f1dc2d915d04aad31b0eb99.png)  
![picture 5](../assets/images/5821fc71b78eabc2a6f04ffa6262363a7f961627b8948e1326aee7c46269a134.png)  
![picture 6](../assets/images/dced148417fdade113634579e61505ff8bf1b7beba6fe0adfef8adfd95d1695f.png)  
![picture 7](../assets/images/59f0dcbcb7e28f660f96689d97695a6dd97aa8fc08e1fc36613c825e1d6ecc9b.png)  


## 提权

![picture 8](../assets/images/8f6a68545effd2d22bcfb96f504ddf7fa4ee40322b36f6f820f5f870cb7206c4.png)  
![picture 9](../assets/images/66dea52a64ed221ebdd98fd7f930d4723c64b5d3815bb810d229a8c7868ccc60.png)  
![picture 10](../assets/images/ec41989c013d15c1de1707006e08cd6bb2e805cb90d5231ab63c40663e6fa42c.png)  

>结束了，不如群主的靶机
>


>userflag:
>
>rootflag:
>