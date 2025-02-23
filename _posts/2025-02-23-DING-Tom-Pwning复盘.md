---
title: DING-Tom Pwning靶机复盘
author: LingMj
data: 2025-02-23
categories: [HomemadeDrone]
tags: [Pwn]
description: 难度-Medium
---

## 网段扫描
```                                      
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:0a:12:7f       (Unknown)
192.168.56.162  08:00:27:91:56:86       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.865 seconds (137.27 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.162
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-22 21:57 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.162
Host is up (0.0015s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 93:a4:92:55:72:2b:9b:4a:52:66:5c:af:a9:83:3c:fd (RSA)
|   256 1e:a7:44:0b:2c:1b:0d:77:83:df:1d:9f:0e:30:08:4d (ECDSA)
|_  256 d0:fa:9d:76:77:42:6f:91:d3:bd:b5:44:72:a7:c9:71 (ED25519)
80/tcp   open  http    Apache httpd 2.4.59 ((Debian))
|_http-title: Don't Hack Me
|_http-server-header: Apache/2.4.59 (Debian)
6666/tcp open  irc?
|_irc-info: Unable to open connection
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port6666-TCP:V=7.95%I=7%D=2/22%Time=67BA8EDE%P=x86_64-pc-linux-gnu%r(He
SF:lp,25,"\n\[!\]\x20\xe6\x8d\x95\xe8\x8e\xb7\xe4\xbf\xa1\xe5\x8f\xb7:\x20
SF:11\xef\xbc\x8c\xe6\x9c\x8d\xe5\x8a\xa1\xe7\xbb\x88\xe6\xad\xa2\n");
MAC Address: 08:00:27:91:56:86 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 50.52 seconds
```

## 获取webshell
![图 0](../assets/images/4b303f6655f2d0b42828101aa4a43a69a6521ba50ea980eb54a0d4d8886bdf9b.png)  
![图 1](../assets/images/4dc4f165faa7d4b8daa4bdae150f60838e2bb1492fae592f9a20b8c919e591fc.png)  
![图 2](../assets/images/d9504c32b5acb45bb8854565dd49f69ffd2b5fead9be29e8e591e8e6d49342d6.png)  

>枚举目录么，我直接爆破一下吧，但好像测试服没加其他字典奥
>

![图 3](../assets/images/a69cbc32a051749f48b6935ff8b27e5a976b0420445d6512077d7f6ead4cf39a.png)  
![图 4](../assets/images/727363104ffc1a4ba42f6284d27aabe4c168d4526b8bbd5962ac452ba0c2fa22.png)  
![图 5](../assets/images/b0b12dba4eb39aaa2bd8c45cad831907262c29f8a266ce3a7a66de9c45412cc4.png)  

>端口么？我只见一个6666还访问不了，他不能连接目前看
>

![图 6](../assets/images/d9be4783defbdcf81f7d699257c4025e53e433e24c75eb28f442fefee556edb9.png)  
![图 7](../assets/images/0dd28604ef48456621e3d7c186c569bde127681c441a212c79937fa4dda76916.png)  

>好像只能在6666这块花费功夫了
>

![图 8](../assets/images/ea4a78b0e6bdc4c4d965e89e0a4690e1ecb272ca66bc65966cd1acb7b1ba6181.png)  
![图 9](../assets/images/76605f66f1b0340c3ab0ede500c2c13c222c7bcf4c5aedd0c9c9c167da7b1b99.png)  

>没想法，看来前面这个入口我得花点时间了
>

![图 10](../assets/images/52b96cf7d6a8010e7eb6f0dd37c84b69e2b3cccdc1afceb4b7bbe4d7c7625267.png)  

![图 11](../assets/images/1b59a52efef700153c760dea9a0f617825380eb979f5dd64b78d7f6f5b17dc9f.png)  



## 提权



>userflag:
>
>rootflag:
>