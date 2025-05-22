---
title: hackmyvm TheFool靶机复盘
author: LingMj
data: 2025-05-22
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
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.181	3e:21:9c:12:bd:a3	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.069 seconds (123.73 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.181
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-22 04:28 EDT
Nmap scan report for jarjar.nyx (192.168.137.181)
Host is up (0.036s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 1000     1000           37 Oct 22  2021 note.txt
|_-rw-r--r--    1 1000     1000        44515 Oct 22  2021 thefool.jpg
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.137.190
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    nginx 1.18.0
|_http-server-header: nginx/1.18.0
|_http-title: Site doesn't have a title (text/html).
9090/tcp open  http    Cockpit web service 221 - 253
| http-title: Loading...
|_Requested resource was https://jarjar.nyx:9090/
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 45.98 seconds
```

## 获取webshell

![picture 0](../assets/images/0a5b6ca11563cb10c80dc4c83cd2b0b958d9021a091678767c92881618203d5a.png)  
![picture 1](../assets/images/92c94d28464db8cb95331593077677b854b177c89f602a1fbd5701a0d968488b.png)  

>这个猜谜
>

![picture 2](../assets/images/82f73f0fa2e1dc45adeaa2b08434a1dc45691332a88d6b1c2500ef45bac8bd56.png)  
![picture 3](../assets/images/b07dee62504364c1416ec0176f8c722ea9373d12ca0594bef97ec3fbc677eebf.png)  

>一个用户名：minerva
>

![picture 4](../assets/images/9f65ecdf9c69a8fd136bac63488c847905c761293fbe92f3c781fe3fd3284315.png)  
![picture 5](../assets/images/2b2f546559d8971920918e09acc9233bc71ad43cd593252e9a9d219560c75889.png)  
![picture 6](../assets/images/cb8ee1c5c81263efd9b5a3a6a96c405db08584ca6eb7e1be226cf242566fb5d1.png)  
![picture 7](../assets/images/4f48b608cbf8370f9e84eafa0673b6104f876da1656c15ddebed4f87574f8b94.png)  
![picture 8](../assets/images/820a1561e881d46e830c08a286cccf5aec8d4d05a2596c469bc7c92d73b8fbe2.png)  
![picture 9](../assets/images/9d58650dce6de211c32ac63d672ef955c23417347205d2db7fdbb2555569e6b0.png)  
![picture 10](../assets/images/cfdc9ded37ae368e471e8208e86ded75c65ed1c547efe14991b200d8f2708adb.png)  
![picture 11](../assets/images/b2c3d912b366345d82aa58cd5fcce4c58423fd5e5b8c06c57eb9308915a1c8b7.png)  
![picture 12](../assets/images/4ca8875be8460a5d84ce287ca97686f38ab28ec692962e1844a07d50921f3a23.png)  

>minerva:tweety
>

![picture 13](../assets/images/ff4084b4b9c133bc198932c852de87f5782f59e5dcf8ed96771496337851bcfc.png)  
![picture 14](../assets/images/b02c3a70bf266f4e59decfd00777d2eae213546ea6242c7211dfd723a347bca5.png)  

![picture 15](../assets/images/d58f2d29e5098fa93ec2ad9258efbd2dc408ae7113a4939bdd9f737f58dbd606.png)  

>这个玩意可以知道目录有啥但是没有登录的我很难受
>

![picture 16](../assets/images/4ef269f314d009c133e0cfa30a9d1706e0d57923217860a533036a530bcdb76d.png)  

>自动停了
>

![picture 17](../assets/images/ab22288bd52039abaf28372be080628d1c6e3b0db28653e31e89b3cc56dfbd3b.png)  

>root也没有的话就很难了
>

![picture 18](../assets/images/d51dd8b7086edaf18eb24cb62e7f5ffbef5362f9f1953011883e8e235227a3e2.png)  
![picture 19](../assets/images/410462a62b9427a0a12047fc7497e23cf826c818e75044653af6a766efdc6fe3.png)  
![picture 20](../assets/images/34de1966eac72577d94e13fe7bea04ff93c81ce6ec92d3800afa2ec3ccae31ca.png)  
![picture 21](../assets/images/6a98c40bfecde3a4c29c16a5669c839dbc9e7ea999ea34da01dd85c10bc071c7.png)  
![picture 22](../assets/images/f6c05e9c38cec04ac9c277be5255eb8c175e7a173658c583d2ecd60945170f04.png)  
![picture 23](../assets/images/27e0b56fdcee3e9592feafff0b3545e671bbebda94ff349f0fea4809b0ca01ce.png)  
![picture 24](../assets/images/5ae757c5db4e6d215643d3ee0ed780dd4dd8edb574ec99daa89d2bccd8add780.png)  

>刚刚敲空格所以没触发
>

## 提权

![picture 25](../assets/images/a44fe0a5f6db4bb8ff34320929e87e838381fe18c7a7eafc73cadf22a7deecd5.png)  

>刚刚突然断了，所以我burp一直拦住他
>

![picture 26](../assets/images/243d46105f779215605b5690dd0c217511ef57b1c56c8ac15d42e2f81986fcd9.png)  
![picture 27](../assets/images/929496965793e6ec423802290afef9c09a8fb762b853931514a66865f55f8e22.png)  
![picture 28](../assets/images/5afb6d304ba732c10d39c794c26ed7e6e4b878e6443d5eeeff0f875363fe0cbf.png)  
![picture 29](../assets/images/fb12a5d4811e9d9bd772bff4727ce436144cef088cb50bc26902567af0cc3b28.png)  
![picture 30](../assets/images/205eb27b5d26f060405c5d77cf711fba604aa4122305a507b4fd2a6e74edceac.png)  
![picture 31](../assets/images/4a70180946b8c354817679d4e0e095dcd1052ca4d6cc92220c6ff5269c47178a.png)  

```
From minerva@thefool Thu May 22 04:44:06 2025
Return-path: <minerva@thefool>
Envelope-to: root@thefool
Delivery-date: Thu, 22 May 2025 04:44:06 -0400
Received: from minerva by thefool with local (Exim 4.94.2)
	(envelope-from <minerva@thefool>)
	id 1uI1X3-0000Iq-11
	for root@thefool; Thu, 22 May 2025 04:44:05 -0400
To: root@thefool
Auto-Submitted: auto-generated
Subject: *** SECURITY information for thefool ***
From: minerva <minerva@thefool>
Message-Id: <E1uI1X3-0000Iq-11@thefool>
Date: Thu, 22 May 2025 04:44:05 -0400

thefool : May 22 08:44:04 : minerva : user NOT in sudoers ; PWD=/run/user/1000 ; USER=root ; COMMAND=/usr/bin/cockpit-bridge --privileged


From minerva@thefool Thu May 22 04:48:24 2025
Return-path: <minerva@thefool>
Envelope-to: root@thefool
Delivery-date: Thu, 22 May 2025 04:48:24 -0400
Received: from minerva by thefool with local (Exim 4.94.2)
	(envelope-from <minerva@thefool>)
	id 1uI1bD-0000N5-9I
	for root@thefool; Thu, 22 May 2025 04:48:23 -0400
To: root@thefool
Auto-Submitted: auto-generated
Subject: *** SECURITY information for thefool ***
From: minerva <minerva@thefool>
Message-Id: <E1uI1bD-0000N5-9I@thefool>
Date: Thu, 22 May 2025 04:48:23 -0400

thefool : May 22 08:48:22 : minerva : user NOT in sudoers ; PWD=/run/user/1000 ; USER=root ; COMMAND=/usr/bin/cockpit-bridge --privileged


From minerva@thefool Thu May 22 06:05:57 2025
Return-path: <minerva@thefool>
Envelope-to: root@thefool
Delivery-date: Thu, 22 May 2025 06:05:57 -0400
Received: from minerva by thefool with local (Exim 4.94.2)
	(envelope-from <minerva@thefool>)
	id 1uI2oG-0003yZ-MC
	for root@thefool; Thu, 22 May 2025 06:05:56 -0400
To: root@thefool
Auto-Submitted: auto-generated
Subject: *** SECURITY information for thefool ***
From: minerva <minerva@thefool>
Message-Id: <E1uI2oG-0003yZ-MC@thefool>
Date: Thu, 22 May 2025 06:05:56 -0400

thefool : May 22 10:05:55 : minerva : 1 incorrect password attempt ; TTY=pts/0 ; PWD=/home/minerva ; USER=root ; COMMAND=list


From minerva@thefool Thu May 22 06:08:20 2025
Return-path: <minerva@thefool>
Envelope-to: root@thefool
Delivery-date: Thu, 22 May 2025 06:08:20 -0400
Received: from minerva by thefool with local (Exim 4.94.2)
	(envelope-from <minerva@thefool>)
	id 1uI2qY-0006Kp-Ju
	for root@thefool; Thu, 22 May 2025 06:08:19 -0400
To: root@thefool
Auto-Submitted: auto-generated
Subject: *** SECURITY information for thefool ***
From: minerva <minerva@thefool>
Message-Id: <E1uI2qY-0006Kp-Ju@thefool>
Date: Thu, 22 May 2025 06:08:18 -0400

thefool : May 22 10:08:17 : minerva : a password is required ; TTY=pts/0 ; PWD=/home/minerva ; USER=root ; COMMAND=list
```

![picture 32](../assets/images/ecc377173123a6008b714c3fd2b8614c49f27acac21dcab34ed038d39d3f90ef.png)  

>这是什么问题
>

![picture 33](../assets/images/2efcf400f6a108e505ec0a055085bbb27827cc6a9da98bd6058f6a9e3299c309.png)  
![picture 34](../assets/images/34f5130f2f16ba8ce1f26af798f0ae83a2ec9a2f4b65ad76f0bdde70f3928852.png)  
![picture 35](../assets/images/ec9b124026f4066c95f405efee8d15142c9c2159015c0c25157a4e884df48da9.png)  
![picture 36](../assets/images/95b6457ee7d6ddf6370d509d831552307f944a424e177de22fab2e79777c5015.png)  

>结束那个shell挺难找我看wp做的
>


>userflag:GUY6dsaiuyUIYHz
>
>rootflag:BMNB6s67tS67TSG
>

