---
title: Self-evaluation Anjv靶机复盘
author: LingMj
data: 2025-04-25
categories: [Self-evaluation]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.107	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.55	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.135	a0:78:17:62:e5:0a	Apple, Inc.

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.990 seconds (128.64 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.107 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-25 03:46 EDT
Nmap scan report for Anjv.mshome.net (192.168.137.107)
Host is up (0.043s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: \xF0\x9F\x9B\xA1\xEF\xB8\x8F ULTRA SECURITY SCANNER v12.7
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.95 seconds
```

## 获取webshell
![picture 0](../assets/images/d7120668b9fddde6f3c36a763d103045a49fe6c9a3b455833dd5ce0d98a89bfd.png)  
![picture 1](../assets/images/07fc3f8f3128c5ef6d8e9e646c38232fad7546b2c46e7cd23dee3704f089f432.png)  
![picture 2](../assets/images/8e5512686ba6a89f0bea2c0fe894810a21e549043913e794a88f89bb96b020e2.png)  

>逻辑是前面这个既可以http://也可以是https://，我测试了一下直接get我们自己，但是没有无法进行文件书写，只能读取文件才行尝试files
>

![picture 3](../assets/images/be9ffcee3e4aeb5b23c2cb05c8bab96a832e3ee96576bfbb4290f51a5f74466f.png)  
![picture 4](../assets/images/77d06f7a83b4174cc33842b321d63a63cc5e0a9fd66fea497329d86b840a470f.png)  
![picture 5](../assets/images/1867d7bafd3b3d752b77cb895d6949952a8ae960946778f725c3028643d6f8ca.png)  

>尝试读取用户信息
>

![picture 6](../assets/images/f4a955d901a4d29272dfd238e8cc00a84f85228f4b39ba181e342144a456b1db.png)  
![picture 7](../assets/images/a53fdbc45f88ede9de4fc8c12bbf57f522261ca22c4d71ec861fe041097d2b79.png)  

>读私钥了
>

![picture 8](../assets/images/318c4cca60ec570db9c86cae579324a9ad3ad8e7d52c2ab3ec27da994842c7f6.png)  

>没有么
>

![picture 9](../assets/images/0771e063b7f02cf253b5d20b6989635640d3822b32a0ec33c23481ca16f6b8b1.png)  
![picture 10](../assets/images/fa8a6fa0bf765ee02a0c09725787b3a0dee21e4a84abcda93d6c7ede20057660.png)  

>OK结束了
>



## 提权
![picture 11](../assets/images/23985a2eb3bddab016e66fa92908b2cf648c7f3ac323f61b0f9d32d74212f902.png)  
![picture 12](../assets/images/9c57327d57c767ebba191ff30f16e39e7af4a6efe1c0def90b7daf3fdc412c27.png)  
![picture 13](../assets/images/d4c2eabac44f84cd5aaecbd13d569041dc012e3d1a4d2b5436655f5576994f2d.png)  
![picture 14](../assets/images/03190963f1f38b5c067f4dfb7f439888a496886b25815ff108dfb4e41bdfb7ae.png)  
![picture 15](../assets/images/045340b8fe69ba9e0a109c94da6cc821a40986b919e1be53868ee748096a271a.png)  

>我好像打过这个东西之前就有这个问题一个vulnyx靶机，所以结束
>





>userflag:flag{user-f15386efb52673b9f3f82fb8e05d09ab}
>
>rootflag:flag{root-f15386efb52673b9f3f82fb8e05d09ab}
>