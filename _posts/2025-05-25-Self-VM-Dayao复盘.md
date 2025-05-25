---
title: Self-VM Dayao复盘
author: LingMj
data: 2025-05-25
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.55	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.056 seconds (124.51 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.55 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-25 04:46 EDT
Nmap scan report for Dayao.mshome.net (192.168.137.55)
Host is up (0.083s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: \xE2\x9C\xA7\xEF\xBD\xA5\xEF\xBE\x9F: *\xE2\x9C\xA7\xEF\xBD\xA5\xEF\xBE\x9F:* FILE TRANSFER *:\xEF\xBD\xA5\xEF\xBE\x9F\xE2\x9C\xA7*:\xEF\xBD\xA5\xEF\xBE\x9F\xE2\x9C\xA7
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.97 seconds
```

## 获取webshell

![picture 3](../assets/images/e295d2324272a080e309b97d56a16ff7fe3f504da5edc0a3937ce4d112bafe39.png)  
![picture 0](../assets/images/80b6903a4f58e08e3c66757f9a8db34c2028b3b6b2e45ea2f5a51664ebb7e052.png)  
![picture 1](../assets/images/ca5f627ae6a07e637943607862fbf45b863293a343f196a9fb9b54804fc4bbc6.png)  
![picture 2](../assets/images/6fa2dc34d196a61c813ce820e42fea5c120158d692dab64300d63da839f19350.png)  

>有LFI和upload的提示，主要看在那里上传文件
>

![picture 4](../assets/images/5bfdac39447f93c14db1b02a46e79447e00357983da8066d975c35804ca8b92b.png)  

>可以看到有tftp
>

![picture 8](../assets/images/ac32dad8d942399866ec37af8ca009165e6677d19c65cfb4e1c144812126c553.png)  
![picture 5](../assets/images/20890b693a48afbbcac646f55687bb4323aa703eeff5fba155826cb84927767c.png)  
![picture 6](../assets/images/8e6574926b0431b0daefd4a45cf263d71da7462afabc591b79f2de134ce9679e.png)  
![picture 7](../assets/images/523b469fe32693a038dcabb9cb4a1442bcb788a4417947bb9c7a0e810c870a8c.png)  
![picture 9](../assets/images/7bf14a10c9edcb0865779ae85177dda72146790bda207e7df42e2db5f205f35b.png)  
![picture 10](../assets/images/3b2ea700187986cc1e4109a064a304d81267b26dc323eb84270a99883d3297b3.png)  

>好了接下来就是busybox拿shell
>

## 提权

![picture 11](../assets/images/ad8f43e11cc2f3e7aae1968b43517e2cad4308f83d8c0556ed8bd48291c8a133.png)  
![picture 12](../assets/images/2d14173e6e49ed1b5c8847938261b282ac7b88cf174d155af11c43c4ae9633c3.png)  
![picture 13](../assets/images/67e3c18c2be91ec22c813f51de916e0289a75a0e30b30f76a6abace0bb3a436a.png)  

>存在定时任务
>

![picture 14](../assets/images/e14cda3a9eec13ab60ac66ef4ef8cde004ed767dd7ffe7c1af40ff95136aefa9.png)  
![picture 15](../assets/images/d867605c099c104cf87a1f28ffb3f113f6e42adfc8484bb6d54eb3822561d10a.png)  
![picture 16](../assets/images/cb63df9f861d5f8a8934730bedcf35f84a2bfe12a0b4cdce75d20c019c1e76d3.png)  

>可以get也可以put
>

![picture 17](../assets/images/01ff954c643497b67fe59ab840f6db3f40ea02ed969d68ba90e921927aeaf95b.png)  

>本地有tftp所以不用跑出去整，可以看到我们get文件权限是root，所以可以利用这个文件覆盖
>

![picture 18](../assets/images/eba9600b51ef766e31fb733010c4bcc2c6f53e8bba14b16fb1aec6781e210359.png)  

>路径在www里所以我们写个给www即可
>

![picture 19](../assets/images/b5fe53b605e00a1156fb4576a5e2cca910f11f330d6d706193b641a590197da5.png)  
![picture 20](../assets/images/2289c46d83a721f33df0f2ac6b931e168cfc20629936eac88d8c7b60868b331b.png)  
![picture 21](../assets/images/942731994f7ca325bd49c86b801422063ef61432bd7e10828b8f9e787186afdc.png)  

>好了方案很多我就不复现了
>

>userflag:
>
>rootflag:
>
