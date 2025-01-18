---
title: VulNyx Mux靶机复盘
author: LingMj
data: 2025-01-17
categories: [VulNyx]
tags: [rsh,strings]
description: 难度-Low
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
192.168.26.162  00:0c:29:ad:63:7a       (Unknown)
192.168.26.254  00:50:56:e2:a3:32       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.921 seconds (133.26 hosts/sec). 4 responded
```
## 端口扫描
```
└─# nmap -p- -sC -sV 192.168.26.162       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-16 20:03 EST
Nmap scan report for 192.168.26.162 (192.168.26.162)
Host is up (0.0012s latency).
Not shown: 65531 closed tcp ports (reset)
PORT    STATE SERVICE    VERSION
80/tcp  open  http       Apache httpd 2.4.56 ((Debian))
|_http-title: Monna Lisa
|_http-server-header: Apache/2.4.56 (Debian)
512/tcp open  exec?
513/tcp open  login
514/tcp open  tcpwrapped
MAC Address: 00:0C:29:AD:63:7A (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 127.88 seconds
```
>这里我们可以获取到开放的端口显眼的是512，513，514，它们像是一种登录操作的描述
>
![图 0](../assets/images/21019d598ec844a996b63897d90fe95269e44ece82604330fda2c0f31492680d.png)  
![图 3](../assets/images/ca7c8c17835c120ae36c61737440df1228d2167a997cfcd34619218cba2d54e7.png)

## 获取shell

>这里我们可以把图片下载下来进行steg的一系列操作
>

![图 4](../assets/images/45268dc4d4101495b93e69b6d0a8ae0aa1d168abdf6279c2619606f9982591f4.png)  

>这里我们可以获取到一个像是账号密码的东西继续进行操作
>

![图 5](../assets/images/91c21562cd25df70fc1a76200514c3a6249c57a41bb0e698b6b835cfce3db869.png)  

>操作了一下并没有特殊的回显内容
>

![图 6](../assets/images/9778e6b407eb8786801f5476c4c5756c58be520ff722af665a7168d0622ede2f.png)  

![图 7](../assets/images/d167ada97c336264ee92c5dc7d1caef8e70ae030e24696a684b77e3f7b48b658.png)  

>没有收获，需要查找对应的一些资料
>

![图 8](../assets/images/2dbc06f2dd9188494b0f5424cb1b50d889b91d386059e70a0ddf4dc156aae6a8.png)  

>到这里大概有思路了
>

![图 9](../assets/images/1f29e03fecdd75e7de796ebab9459e108a491f61cc2be336dcccb8663fa159a5.png)  

>有点问题登录，这里有点卡住，先扫描目录缓缓思路
>

![图 10](../assets/images/89892f19bc5a4a6730f16b85f6dc210fd493f9463060a43bd5088c15b0d93d53.png)  

>有点没头绪，唯一线索是图片，上面我只做了exif可以试试其他方法
>

![图 11](../assets/images/fec938d530c2b1ebcf3bbfaa3a36196d453a95d0a4806c017af2bc8cd1d21767.png)  

>这里还有一个密码尝试一下
>

![图 12](../assets/images/b15605cb7e2e2d352f10c6179cc8f3cabe0830e1ff95ce1997fe464a710ebcdf.png)  

>看来主要的图片查询方式为strings
>

## 提权

>先进行sudo -l
>
```
lisa@mux:~$ sudo -l
Matching Defaults entries for lisa on mux:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User lisa may run the following commands on mux:
    (root) NOPASSWD: /usr/bin/tmux
```
>发现不存在与ctfobins，这里利用-h进行操作
>

![图 15](../assets/images/30f630d559f552a8a8bfede7045ed1316543433241719a329b152a2c5573ad61.png)  


>好像可以执行命令
>

![图 14](../assets/images/99761e7ad5a6116e7cbc2d1dc1a094a85ce27d15fcd922344943b7c084351ff6.png)  

>到这里靶场复盘结束
>
>uesrflag:be2034f028ebe41244687a8498c7cd3d
>
>rootflag:bcb441bf0878dca6f6d4d2c7787c6f4b
