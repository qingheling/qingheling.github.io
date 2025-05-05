---
title: Self-VM Kakeru复盘
author: LingMj
data: 2025-05-05
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---


## 网段扫描
```
root@LingMj:~/tools# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.84	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.036 seconds (125.74 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-05 01:08 EDT
Nmap scan report for Kakeru.mshome.net (192.168.137.84)
Host is up (0.033s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: User welcome's password is here.
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.50 seconds
```

## 获取webshell

![picture 0](../assets/images/00074a396553ab1e780ae5a3bc08cadcfef7cf48b4680d4b541e75456ae17f5d.png)  
![picture 1](../assets/images/4b2765cf98de6d6a16b0bd77dd3552a850d3d7ac9329734a257b755eb1ee2162.png)  

>x-开头en结尾部分
>

![picture 2](../assets/images/e8ab06fa97fab97d9e63613c258a2c05bfc24ea80b688abb47f1061cfd351fe9.png)  
![picture 3](../assets/images/0d82a4a24c231c71a87605a8b8cdf92ddd8305ee6791d5026fd28e56c3df4d13.png)  



## 提权


![picture 4](../assets/images/801d3608937017aa45e8e1337bf3248ec20631f74d0710d043795389089221a5.png)  

![picture 5](../assets/images/ee9593b85100cfdc0fee0a4c04e8e188a2e7a52d69106c0abb37b409cf283d6b.png)  
![picture 6](../assets/images/041a231bebc3865b65c7bd6becef6dac14b7204865368e25a89afece42d8830c.png)  

>好吧gtp是dashazi了，压根没给我正确答案
>

![picture 7](../assets/images/05ac27139e0514b0143d1eb04bed83e841484aad027b2fd8ea7cd97a66f664f1.png)  

>查了半天挨个试命令了
>

![picture 8](../assets/images/09faaa7b124d7e5f4afe3e83d52e3000fd01dfe81effbf96882e3fd182d3e3fd.png)  
![picture 9](../assets/images/73b49c965054bedca37ce00a5e64425ee8e2fd0f2ec55b9b93f1484e176bcc53.png)  
![picture 10](../assets/images/466712bca142f79d2f2be14357ac7ee58922afb3b50e0df78dba6bd7f1752975.png)  

>这是什么相同文件
>

![picture 11](../assets/images/c35a17e85c2a57c7b2479004cf67f95948881766ac158f70fcfa5e0854996168.png)  

>会自动停而且刷没权限
>

![picture 12](../assets/images/599868177e29aa4a97953ee222c72e18365ab2390a6620ce67c2f45503237ebf.png)  

>好像自己执行这个东西吧，主要没看到里面我也不知道输入啥，但是自己停了就执行里面的话大概率一个定时5秒吧执行home/test文件
>

![picture 13](../assets/images/93c30cf65633d7fb278369bb17e5f13155ce34840c401c9fa58b925133a41f79.png)  
![picture 14](../assets/images/bfb45c7d04bcd7e688ff4cbc225594d3376a1d78ae63e8fb0d8bd39baa2f860a.png)  

>还真是，结束了
>


>userflag:
>
>rootflag:
>
