---
title: Self-VM Umz复盘
author: LingMj
data: 2025-05-04
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---

## 地址获取

```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.78	3e:21:9c:12:bd:a3	(Unknown: locally administered)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.133 seconds (120.02 hosts/sec). 3 responded
```


## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.78
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-04 04:57 EDT
Nmap scan report for Umz.mshome.net (192.168.137.78)
Host is up (0.0081s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: cyber fortress 9000
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.15 seconds
```

## 获取webshell
![picture 0](../assets/images/006d428589901c5bbf8708b497429fc42daa0cf154cd6f2eaab9d4ad695d94a8.png)  
![picture 1](../assets/images/9ba64b97b575c5f458e71a4ca119a923b3edcca14164c987b82cb2f0be846dd4.png)  

>这里提示是ddos，但是我试了一早没啥用，要了提示方案和工具，这里要用很大的线程操作才行，我原来都给50以为很大哈哈哈
>

![picture 2](../assets/images/0b985a480ddf9a3d07edbe362bf334b8e83b4d9220f55c24605352f2579a0c9f.png)  
![picture 3](../assets/images/2c12be67315dd72719ba280d08aadb19391b95d91abaf21250655421a40694b4.png)  
![picture 4](../assets/images/640b77bf783b11fcbbf37daf73db0a199b9bb0976b9d76117c8a736524a5cfa3.png)  

>密码admin
>

![picture 5](../assets/images/a0b4cdce7d083f0e15e1c82d8b3e03523035f3b0164d67b75d243cf9ebba7e0b.png)  
![picture 6](../assets/images/e87ddab21650d8d7def86a4840f3ffb9299bb8e9cd647ac99e0e9b494b299ead.png)  
![picture 7](../assets/images/f6a574c0f964c0e193f0aedcaadf56f5ad4e455a18402668b8b4b4773ebae5a7.png)  

>这里要注意的问题是字符长度，因为md5sum默认使用换行操作的，我这里一开始没考虑md5比对，我直接suforce跑了很久一半了，但是肯定没有比对快这里说明一下
>

![picture 8](../assets/images/8c98109d93ba6309ae03ce4a9ccd4595dc96b2cbcd3062cf93d14fa1bf6e9ab8.png)  

>简单脚步进行比对因为两遍没有进行换行处理所以直接md5sum
>
![picture 9](../assets/images/104b76a75b523b1d27a7914278d9d69e9d530d0b7e03dc6f15be42e354711293.png)  

>也就2秒出来了
>


## 提权

![picture 10](../assets/images/c81532cf603d286d42fa52482d44c8299fcd0a2af8a82504e54558d628d788e9.png)  

>这里的密码是另外一个用户的
>

![picture 11](../assets/images/2ffe4aa8ecdf7fb4db5a0e49223ba59dc8a470313ef3b7bf105ef41af2ed572f.png)  

>这个我看着很眼熟查了一下是dd
>

![picture 15](../assets/images/7d30fd0e5e4323f4064a92ad6d15c295403ba1dfa19083246dd55c96f2319df0.png)  

![picture 14](../assets/images/d8e1da0a865ae0774f658292e0f3100cebcd5bd6dc6dc7c76bc13c1fad6b81b9.png)  


![picture 12](../assets/images/31f5de57d9ce72caa9a845453503ab262e11d7a45b19f3286bf0221c547e3fd8.png)  
![picture 13](../assets/images/a59867caa023483d7d6b6db26aa5b3a9100c015f56e128fc22d283a3dd9b46ec.png)  

>好了整体难度是easy的
>


>userflag:
>
>rootflag:
>

