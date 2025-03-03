---
title: VulNyx Bunker靶机复盘
author: LingMj
data: 2025-01-22
categories: [VulNyx]
tags: [sctp,tomcat,gcore,pid]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.197  00:0c:29:1b:da:11       (Unknown)
192.168.26.254  00:50:56:f7:71:cb       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.958 seconds (130.75 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.197
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-21 22:33 EST
Nmap scan report for 192.168.26.197 (192.168.26.197)
Host is up (0.0019s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.59 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.59 (Debian)
MAC Address: 00:0C:29:1B:DA:11 (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 50.09 seconds
```

## 获取webshell
![图 0](../assets/images/a7c2f4c3e76aaca35295e50b83bfe74f9196beafb407314a2cb3cae89e510ea0.png)  
![图 1](../assets/images/8100ed3ab5d7ba9f21b58ae86bf9ade5e0aae382ce19a18d284be076e809b284.png)  


```
└─# nmap -p- -sY 192.168.26.197
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-21 22:59 EST
Nmap scan report for 192.168.26.197 (192.168.26.197)
Host is up (0.0012s latency).
Not shown: 65533 closed sctp ports (abort)
PORT      STATE SERVICE
22/sctp   open  ssh
8080/sctp open  unknown
MAC Address: 00:0C:29:1B:DA:11 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 62.58 seconds
```
>发现存在这个sctp，但是http://不能直接访问
>

![picture 0](../assets/images/f5e165fe9a24a60b27d91be4f7fb2c1ea4fcd33a2b464a5eb0fbe6edeb29f2b7.png)  

>这里端口转发利用socat就好了非常简单
>

![picture 1](../assets/images/2b53d431b666135673393f0a3d00a7d475a5516403a21bafe380db767e639888.png)  

![picture 2](../assets/images/cdb5d637644d2a33c2d061eaa49e59d04d8275a3bfda78bbd8611cd4b471a203.png)  

>是一个tomcat的网站，我查了默认账号密码可能为admin，password和tomcat，tomcat所以我挨个试一下
>

![picture 3](../assets/images/93cb5a7786ed30232b0f3bb9c4b321fbcf42fc7284c8eef55f8debd111be80e4.png)  

>发现账号密码为tomcat，tomcat,这里上传是war直接找相应的reverse poc就行了
>

![picture 4](../assets/images/1c1a16c50d705fb588c0c42e0c91d087bc5dfbb3218e6ed4ad6993e8636443d5.png)  

>可选择其他方案为选了另外的reverse方案，这个没成功
>

![picture 5](../assets/images/9fd7a31238aae926b398897d1628a5c3fad2020ca8cc04b82517cc74c0e2a755.png)  
![picture 6](../assets/images/237f44f17401249f54b6d97eff823b0655924da28c66bf42e45410510106d3d4.png)  
![picture 7](../assets/images/d0e88459372f89ebe9ebcca5e0dc1adcf52cc48c9cd544a0e9c77f7ac7262ddd.png)  

## 提权
![picture 8](../assets/images/4016b4a9a590cf9ee7011a63a8af9ace167844b6597b2ff5564e428bf822d8f7.png)  
![picture 9](../assets/images/c8dda81eb41f7463d22724d0a49f47a0f4f6977deb854cda71cd2ecb3800435b.png)  
![picture 10](../assets/images/24f2bd85ff5f398a8451c161b2f353be1dcf710ea296ac48dc938f35d143958e.png)  

>无密码直接连接
>

![picture 11](../assets/images/0c3e1cbbf2e36c2fe1cb674bbf56774210f5f257cb055a7a4faec1ab1ec53b41.png)  
![picture 12](../assets/images/aa721cc4085ccea4894b373443159b1fbe30ac92eceacd4adacc45ad7e9f2e86.png)  
![picture 13](../assets/images/8a75d1497abadc15cfba4155c6bbfa8b7048161cb1d815c8e28c6d6e752905be.png)  

![picture 14](../assets/images/2d2fa41064e45727b89052d3e50ee24a27a55fe04f44eaa7e2cac374be85fa00.png)  
![picture 15](../assets/images/b4b6010e8ed92cf3268270863fd7cfd8102866f47045d6b741aeb9f6401da566.png)  


>好了这个靶机结束了，整体还是非常简单的没有任何弯弯绕绕
>


>userflag:a1617ca7d069c13ee365471dec5a389c
>
>rootflag:390a25fd99cfb340eff6c51665109e52
>