---
title: Self-VM awker复盘
author: LingMj
data: 2025-05-04
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---


## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.106	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.229	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.113 seconds (121.15 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-06 21:46 EDT
Nmap scan report for Awker.mshome.net (192.168.137.229)
Host is up (0.036s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: PHP 8.3.19 - phpinfo()
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.23 seconds
```

## 获取webshell
![picture 0](../assets/images/23b348ebe032bb878f36c7ae2cf4c8adad584e72ed18fca27e5d961c97492763.png)  
![picture 1](../assets/images/65e65b855acf733ebc96a988f6290799e1bf7e487423db268f35878778cbe033.png)  
![picture 2](../assets/images/cb7245760fc7073e1581aea689d26ca8bb5e5592677736cc2aea45cf91a065e9.png)  
![picture 3](../assets/images/67896cb72e8bfba1b90bac56c3063006d50d503e970412162a6c54d8c0c3b34f.png)
![picture 5](../assets/images/0d54e46fe0fcf86ab6c300f84d45fa58cdf15a7f33f2ab6380d9721f82bf5c26.png)  
![picture 6](../assets/images/9567bf5822f055b82d3beba7faf019aaad14b4d098bc4a36f960991dd1f6c271.png)  

![picture 4](../assets/images/69d197f5fede266ada657e0144371365896f618fe2c3f30aef8acdc5affcc406.png)  



## 提权

![picture 7](../assets/images/4972f34b67fb17a51795a5b109d1259d6fc27677d4f1bb3568292aabe188fc9a.png)  
![picture 8](../assets/images/2decc78169c2df3585bb8cd34f317ba1044b810347108efb1e560a733556b142.png)  

>要了提示奥
>

>马后炮一下gtp给的答案
>

![picture 10](../assets/images/6236ecf3cd25e2531a8605b67f4a4cc4174f1b6363ae0a09a4f0bbaf1f2a2e31.png)  


![picture 9](../assets/images/a8632e693506d03b1ee12ceb284ff5377142f0f1e947d39f4a6029d6b504250b.png)  

![picture 11](../assets/images/10436b579cf9af7e650a6d7f60fbe442c272d40a7979a9c45be8c828067c4456.png)  


>主要还是这个东西我想应该arp欺骗能做但是我arp欺骗的水平我是知道的所以我去找了类似的wp看
>

![picture 12](../assets/images/fab9dc7b58818d410d0aa427fbba86a099c1f9ce6f8ce5c8841122e2b667cb41.png)  

![picture 13](../assets/images/a23b51f94e458b5898d3b06193c002bdc995241ec23227d109bf10236ea6e2f7.png)  

>发现确实是欺骗了,虽然arp欺骗没成功不过又是提示，可以读文件
>

![picture 14](../assets/images/89cd2617f939476fdeaa33fcaea9576ef1d58998f484aabe31a8a3583a6c55c1.png)  





>userflag:
>
>rootflag:
>