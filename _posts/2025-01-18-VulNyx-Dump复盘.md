---
title: VulNyx Dump靶机复盘
author: LingMj
data: 2025-01-18
categories: [VulNyx]
tags: [samdump,bak,https]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.171  00:0c:29:1b:e0:1f       VMware, Inc.
192.168.26.254  00:50:56:e0:6d:ff       VMware, Inc.

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.579 seconds (99.26 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.171
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-18 02:00 EST
Nmap scan report for 192.168.26.171 (192.168.26.171)
Host is up (0.00097s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
21/tcp   open  ftp      pyftpdlib 1.5.4
| ftp-syst: 
|   STAT: 
| FTP server status:
|  Connected to: 192.168.26.171:21
|  Waiting for username.
|  TYPE: ASCII; STRUcture: File; MODE: Stream
|  Data connection closed.
|_End of status.
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxrwxrwx   2 root     root         4096 Feb 09  2024 .backup [NSE: writeable]
80/tcp   open  http     Apache httpd 2.4.38 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
4200/tcp open  ssl/http ShellInABox
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=dump
| Not valid before: 2024-02-09T11:53:57
|_Not valid after:  2044-02-04T11:53:57
|_http-title: Shell In A Box
MAC Address: 00:0C:29:1B:E0:1F (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.85 seconds
```
## 获取Webshell
>发现存在ftp，可以利用ftp获取关键信息
>
![图 0](../assets/images/74054c457273bf816428545d6ebe189499230ebd2b616101f3e90f2a822a28af.png)  
![图 1](../assets/images/d1e530781d16aab3a989ee55e0b850154a14a484a27ae3c42a656581c57172ef.png)  
![图 2](../assets/images/3c68aae6b8b248abd307ed0d5de0db5e0848a223744d01395bbeb0351bf326f6.png)  

>目前这2个bak文件没有获取对应有用的信息，去扫一下web目录，查看web信息
>
![图 3](../assets/images/fd4116e417853e270f0ef47f99085206e81f075ed8fc168c9dff942788e23c91.png)  
![图 4](../assets/images/1ba557fa8288706000ab38b80f9d0e704ba1a26c8ba0bb074770e36e70ae5b1d.png)
![图 6](../assets/images/28309ace8b5f5593add99e38cf5de2bf21d2ec7cca359c22ee07e7f0107d30fc.png)  
![图 5](../assets/images/18143ab36c6d3ff3bb07f8e9231080316d62042e0b39068231e3d2809ef2bd86.png)  

>此时无线索，查看bak的用法吧。
>
![图 7](../assets/images/7a7e5f07df40519674f841b375ed052e7040a173d09b61a55d554da6bd0ca48d.png)  
>查到一个工具是用于dump bak文件的,看一下手册
>
![图 8](../assets/images/cf9322a893bd3fa530b6c63ee6851ed9c55e93860f0459665dcc4b65da3e8ec0.png)  
![图 9](../assets/images/cba08b7f329caced2a17ea29124c9a279b6eaaff72795cd30b3d3d797d2af204.png)  
>获取的信息是这个样子的
>
![图 10](../assets/images/bdc4a1bfa88423c087d60bfefb2879a24e98df8d9334f65702f114a83c28bcdc.png)  
>这里没有登录的地方,选择尝试4200端口突破
>
![图 11](../assets/images/1c85133483eaf2d004859bb066a5e80cc0db675733f46b5a5b5afab0a2f88f49.png)  

>需要https进行操作
>
![图 12](../assets/images/ffb1690fa4e18da656bbda7c9ca7834945fc68074e42955be1d8de3944470a62.png)  

>这里发现大写的无法进行登录选择使用对应的东西再次爆破。
>
![图 14](../assets/images/a731ff9ca7aef1c412a8bba29c939d5986ff08ac2f1996c3f757ba143a6fe248.png)  
![图 15](../assets/images/1c816deee2e0cc5e10416cd9987b54743d55da742f07acdd32de3e0e50239015.png)  

## 提权
![图 16](../assets/images/9884923e898835e92c39afa181c98c9d58be8a80d5dba2b5421ebd55e747dbcf.png)
>利用工具找一下对应的文件
>
![图 18](../assets/images/637360fe08437bf155a8642bb4852191fcae3f46441cffc31d0dd3d20be6e79f.png)  
![图 19](../assets/images/805bd548c05130ad6f1036407bd4828787f4bf6463f996a47f9e66fc89dea419.png)  
![图 17](../assets/images/6329ee8a802e816d1ff7d46d6d3f9da43200e3f627aa32d9b03e2d6dc32df806.png)  
![图 20](../assets/images/6d5ea526a94ee940d42d4ea5821ee2c413c3cdc69b54084684769419bef38d47.png)  
![图 21](../assets/images/f85a69960742d4f2d8e578b75e60179fe873483a3fadf9b22b4b87db94cc1c26.png)  
![图 22](../assets/images/594ce061517a510e8705f2e65f38b27d403a16e06356e4cbc871dd6aab298580.png)  
>这里需要把端口开出来
>
![图 23](../assets/images/5851f9f91d9027f8c093019b48a8a5cf2da287af9e15fbcb2019a0023555ae6f.png)  
![图 24](../assets/images/3225b1ce772a9defbde660ce0c59c01144bec02b3dfb2cdd0471fbf629b34576.png)  

>到这里靶场复盘结束
>
>userflag:cfbe86765c16e9bf8ddc3739f4f270a9
>
>rootflag:60c60f8e926b65a55bf8bd6239bb616d
>


























































