---
title: VulNyx Hunter靶机复盘
author: LingMj
data: 2025-01-17
categories: [VulNyx]
tags: [upload,domain,bsh]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.165  00:0c:29:e3:59:79       VMware, Inc.
192.168.26.254  00:50:56:e2:a3:32       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.604 seconds (98.31 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.165       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-16 22:54 EST
Nmap scan report for 192.168.26.165 (192.168.26.165)
Host is up (0.0011s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u3 (protocol 2.0)
| ssh-hostkey: 
|   2048 f7:ea:48:1a:a3:46:0b:bd:ac:47:73:e8:78:25:af:42 (RSA)
|   256 2e:41:ca:86:1c:73:ca:de:ed:b8:74:af:d2:06:5c:68 (ECDSA)
|_  256 33:6e:a2:58:1c:5e:37:e1:98:8c:44:b1:1c:36:6d:75 (ED25519)
53/tcp open  domain  (unknown banner: not currently available)
| dns-nsid: 
|_  bind.version: not currently available
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|     bind
|_    currently available
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_hunterzone.nyx
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.94SVN%I=7%D=1/16%Time=6789D4CF%P=x86_64-pc-linux-gnu%r(D
SF:NSVersionBindReqTCP,52,"\0P\0\x06\x85\0\0\x01\0\x01\0\x01\0\0\x07versio
SF:n\x04bind\0\0\x10\0\x03\xc0\x0c\0\x10\0\x03\0\0\0\0\0\x18\x17not\x20cur
SF:rently\x20available\xc0\x0c\0\x02\0\x03\0\0\0\0\0\x02\xc0\x0c");
MAC Address: 00:0C:29:E3:59:79 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.90 seconds
```

## 获取webshell

>发现53，查看是否存在domain，80端口出有hunterzone.nyx
>
![图 0](../assets/images/16f82f478fe7cf8f4676b44fbf331f145b9f89e8822b8f6d25d73eb74d303510.png)  

>利用上面的工具完成有关域名的查找
>
![图 1](../assets/images/2b339cc8edec5c190b556432ae69f8d9df300c82d3912c644e3fd02a8d3742d4.png)  

>需要把域名拿出来可以利用正则处理
>
![图 2](../assets/images/5f233d838b2cad28429dfbf9da03b655e61b2cd33c26cb25181d578fea9265f7.png)  

>利用扫描进行域名内容扫描，利用扫描把上面的所以子域名扫会没有线索
>

![图 3](../assets/images/1c98e83c24a8080f851d53c993b3e4ddb9662a7301491341166388c845f9d2db.png)  

![图 4](../assets/images/faaa042ae5ec378a00ae95477384733a87298aeec8e9a40009978923211c4b59.png)  

>这里扫了半天发现domain还有一个域名
>
>?.hunterzone.nyx.       604800  IN      TXT     "devhunter.nyx"
>
>这里把这个域名一起加上去
![图 5](../assets/images/92479ac5c0e8da9aaabf0d28a10f53a22f0bf75c11fb97d43356e9ea09081152.png)  

>这里出现了子域名，我们可以上wbe看一下是什么服务
>

![图 6](../assets/images/44a932732a5be6e3e6c58a306646b42be04bfbefed9f55976b422b95fd387da6.png)  

>是一个文件上传，这里要准备一下上传的php文件
>
![图 7](../assets/images/56a8997b01ce1bf0643a7e6b31a9c65bb54aabc20c6b17f1334e52385f1af9d1.png)  
![图 8](../assets/images/928e4799aeefbb2177a5aa8f7986240e50566a9b648892eefc4f9f898c34b7dd.png) 
![图 10](../assets/images/05495251ecd07f09e34c426527b137ce52e5c231c0153f1fb8276b2594aacea2.png)  
![图 9](../assets/images/a5d929883fd5bb022755db32bbccbf93abda9d033efbbacb92677f5d5ddbf0d4.png)  

>无线索看看，其他部分比如配置文件.htaccess,并且找一下配置这个的方法
>

![图 11](../assets/images/b1a912ded2ef0762b18ff8b2f229420160696e7c9f6bc4ba19d7c021e51be59b.png)  

>找一下上传的路径位置
>
![图 13](../assets/images/d42109291777220a136f2aa7e7429991c81a7cd4f47cbc3a7362583dd67e00be.png)  

![图 12](../assets/images/b883c584a68de0bfb0e0ce5b642adc59b76bf020e4bec0fe57488765aec5eb6f.png)  
![图 14](../assets/images/1f37e4df320ed3a67470f1d3d4a846c91c178019ab8c5f71bd69756ed100880a.png)  

## 提权

![图 15](../assets/images/a3ef065711aae1496ff286525e3bd160e0bd9720fe9ba1ce4fc9f61b5bacbef9.png)  

>bug@hunter:/tmp$ sudo /usr/bin/bsh -h
File not found: java.io.FileNotFoundException: /tmp/-h (No such file or directory)
bug@hunter:/tmp$
>
>没什么东西
>
![图 16](../assets/images/1647f909c9c8ee58c9a4e61f9f970368c5b0dca442152a3a9f414182bf27339e.png)  

>查看一下手册看看有什么能利用
>
![图 17](../assets/images/9c9bffa1d6c62face675bafc0b941a4c971bf891cbf98e2d524df9d264d9555b.png)  
![图 18](../assets/images/6a2dbb83dc6ea75b7e4d65be9d7b5c84c481e5f67a8eae17c8e73665029c1237.png)  

>这里可以看到可以读取文件，也可以修改文件
>
```
└─# openssl passwd 111111
$1$O4S5rKPu$NLZeHWyGZBSlySU7AlIu6/
```
![图 19](../assets/images/eaccc11f642e3e166959643b29b798304f5dd228739f305461bfe5e484547036.png)  
>没改成功

![图 20](../assets/images/6c5e46b3bcfc3432cd4919990aef411017c4e18990f491820a3665781c3f0761.png)  

![图 21](../assets/images/706fefb81e2665975a76a69f5b43940e7e546e8069d69941c46b05cc764fd1b2.png)  

>这里出现的有用的命令执行是写java文件，继续查找
>
![图 22](../assets/images/aae36605aaed1800732589421c8ddc4075e5cbc69e98f8a6d8a9d5d6bf6d03a6.png)  
![图 23](../assets/images/d55322b1668eea5bd88a846da012e6052ef191f08f426a8a605bac695bc576bf.png)  
>这里出现命令执行完成操作
>
>userflag:4dbd02025cadc283bf3d5cfe95e40ce3
>
>rootflag:39edf8061c93d9a4173c9fe110841ad3