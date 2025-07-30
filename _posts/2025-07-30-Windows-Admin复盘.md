---
title: Windows Admin靶机复盘
author: LingMj
data: 2025-07-30
categories: [Windows]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l         
Interface: eth0, type: EN10MB, MAC: 00:0c:29:fb:0f:16, IPv4: 192.168.137.194
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.239	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.095 seconds (122.20 hosts/sec). 3 responded
```

## 端口扫描

```
Not shown: 65521 closed tcp ports (reset)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
5040/tcp  open  unknown
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: ADMIN, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:63:5f:9d (Oracle VirtualBox virtual NIC)
|_clock-skew: 5h59m54s
| smb2-time: 
|   date: 2025-07-30T17:57:38
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 576.25 seconds
```

## 获取webshell

>端口挺多，可以先尝试永恒之蓝漏洞先
>

![picture 0](../assets/images/ade7f2f1048cd0795183553ef997c6b4837a94dca8168c24122b6eccaee10238.png)  
![picture 1](../assets/images/3f5b057f4f71f8f78fb840c69c6ae2b85535b43373c4e9bdea1273a25e32ec43.png)  
![picture 2](../assets/images/61152c0a4f6092ff8fba58d6300f1924a72b93c74099c8ac236dee86d792ae04.png)  

>貌似不是这个看看网站信息
>

![picture 3](../assets/images/29ad6ad0f13fd1828f67ceef2afe96886eced5631be2e52645b0a9dade302a6c.png)  
![picture 4](../assets/images/bd5bb9cbff2764e85f2c9cb071c5e6e6c0217ed2d3719eba43539d6998da342c.png)  
![picture 5](../assets/images/e60780f6e1262a1a134fc5ba9c4126a21c9a6ee828fce61c3dca0aff4e52041e.png)  
![picture 6](../assets/images/8679a0efbca7fd9afa426dd707ee65b9184af8284d0923029f46f25caa4f9124.png)  
![picture 7](../assets/images/6d539c932dab5b6dd9c06a83af2bb8e9c6564448cfc55affbdf6ad39b0b58a46.png)  



```
crackmapexec smb 192.168.137.239 -u hope -p /usr/share/wordlists/rockyou.txt
nxc smb 192.168.137.239 -u "hope" -p "/usr/share/wordlists/rockyou.txt" --ignore-pw-decoding
```

>这里有2个形式爆破之前出现过crack没有怎么办，所以找了一下留下，他俩没有时间差异一样快
>

![picture 8](../assets/images/1b927cc8b8373bc96948b699c59801654f7574165d35ffb0e11aebbfa89b5c04.png)  
![picture 9](../assets/images/c7882c954ad8eb5f0fd8ce1cf7d97afc1bd0751503552b4ac7526abc83dcde19.png)  

>也不是得利用msf了
>

![picture 10](../assets/images/d2e6a402ffa367de5ba70200b8dcce24be5d3817a1637844e4d64d184bf873e6.png)  
![picture 11](../assets/images/e3a552fb97e86783aa7e195b868fb7cf44d1bc5891802753fd08d9709568baf8.png)  

>看端口查找winrm发现了可以直接连接的
>

![picture 12](../assets/images/79134b74d40ad51afc2e7d5f442fe13d43544707b9432fcb4a7706b3b2863688.png)  

>OK连接上了
>

## 提权

![picture 13](../assets/images/e898ddd39b3ac22db4c6e0e56a7e17700d358181d68281441112eec22cf5f8a7.png)  

>没有tab和基本命令打起来有点难受
>

![picture 14](../assets/images/c61d49b0e8f065cb30ff357edf6f409121cbc4eafec7dc702cf08df256b15963.png)  
![picture 15](../assets/images/e742f34ea4115df3e4fe6167fe3bdf1c9be6b2a4df007a6fbdc3d23ef21ab77a.png)  
![picture 16](../assets/images/da88344e597e2e0b1406bc785f37d6497f18a364671d87d48c3bf5048db3cbb3.png)  

>传一个上来主要是用于和linux一样的linpeas.sh扫描工具
>

![picture 17](../assets/images/f8f4d6f2284b36e3d81f00a4731e817369d6e00d7e23dd245b6f57081a34bceb.png)  

>这里有一个历史文件
>

![picture 18](../assets/images/cc2786e5d3c32beb205933055b0ce803d13f19a7b46cb22e963299fbf445e6f7.png)  

>有密码了登录一下，先退出去
>

![picture 19](../assets/images/455d651c87ef65a5594984493a2f3ac72ea0dac405b088f77630706d3a2ddabb.png)  

>OK拿到权限
>

![picture 20](../assets/images/e02201a6432188c1d8b8a793d9c1416d7ab5adb3fb0af57605beb4361afc4efc.png)  

>不过他能使用cat我是没想到的正常没有cat，OK结束了
>

>userflag:aacd4aebb5743ba45d3b4591ac03ace1
>
>rootflag:fe586ba8f585e1ea97347be057659b81
>