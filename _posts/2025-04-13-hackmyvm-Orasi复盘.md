---
title: hackmyvm Orasi靶机复盘
author: LingMj
data: 2025-04-19
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.53	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.97	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.137 seconds (119.79 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.97         
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-12 22:53 EDT
Nmap scan report for orasi.mshome.net (192.168.137.97)
Host is up (0.021s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
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
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 ftp      ftp          4096 Feb 11  2021 pub
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 8a:07:93:8e:8a:d6:67:fe:d0:10:88:14:61:49:5a:66 (RSA)
|   256 5a:cd:25:31:ec:f2:02:a8:a8:ec:32:c9:63:89:b2:e3 (ECDSA)
|_  256 39:70:57:cc:bb:9b:65:50:36:8d:71:00:a2:ac:24:36 (ED25519)
80/tcp   open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Site doesn't have a title (text/html).
5000/tcp open  http    Werkzeug httpd 1.0.1 (Python 3.7.3)
|_http-server-header: Werkzeug/1.0.1 Python/3.7.3
|_http-title: 404 Not Found
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.84 seconds
```

## 获取webshell

![picture 0](../assets/images/4994b1c931233514d8c9ba9913b76fff9b18ea46280c7be2f793d4d15b09f2b9.png)  
![picture 1](../assets/images/f5cc431f0ad310b834c3c5f72a2fbc5f7dee30f99a4643564d92559bda0fa50d.png)  
![picture 2](../assets/images/3c8b10637149ef617b702f694a9c0ebb4c9d0c8e55298a4d480acc77b8e52786.png)  

>这是什么呢
>

![picture 3](../assets/images/a16ace3334cb9ebad6abf4a9a0b85c6c39e5aaebd63fd24c871174dc0ebc003b.png)  
![picture 4](../assets/images/c4ace5395f191c6946c7b454f7d465a936754700810d92ab4e03844fa3ec9dfc.png)  
![picture 5](../assets/images/84a279cc04660a694bd5feaaf82dbe358d1b576f035707bc8add2fc85a109b25.png)  
![picture 6](../assets/images/b6a3bdc4f3855929d8e416035b22f0379acf93499ae7aeb989c523b776d4ca5d.png)  

>像一个目录
>

![picture 7](../assets/images/85b1c55eaf4744e0b8c5735735bdaed29927eebb67ea9789149a1faa6ae460ba.png)  

>没有成功
>

![picture 8](../assets/images/906a3f9f037afe3d7c93d96a59e801797b284bf00ce2c48041279e6254ead8b8.png)  

>重新检查一下没有*而是/sh4dow$s
>

![picture 9](../assets/images/ca2ac37628805efc8234627f6ad37b845ac7a86c464a88b10b51df942f8ddada.png)  
![picture 10](../assets/images/297db0e648283231864bd9d05f8774ddfb86e55fe6eec39c43a818ae1346eb88.png)  
![picture 11](../assets/images/fad2b36df25c70dec02412bc879cf56f9243eb3d4d0be266c754a2839b48aa73.png)  

>奇怪什么地方出问题了,打错了应该是0不是o
>

![picture 12](../assets/images/cfe1ad4a95345bb186644afb915d984b5325b3ce25ff7efc6514cb05c081ccdc.png)  

>现在利用这个方式过滤出需要的部分直接复制就好了
>

![picture 13](../assets/images/fdb8c41f9a522c4d2e9c29fad3b08df795ef4879a029d34a20adb7ceed50f6a9.png)  

>看一下wp，我之前是不知道crunch这个工具所以6 6 1337leet的提示对我很难想，而且查不到
>

![picture 14](../assets/images/ec1bb6a7ff382baa0d55d5820c91d40675e65679e579caf662b1ec4fa1886dd4.png)  

>这样看就清晰了
>

![picture 15](../assets/images/093ee86e1171e8a19fa1af1081eb80773c0a566fc8c973ca32f7d205f64cdb79.png)  
![picture 16](../assets/images/7049a4d47fc6daa6a1f1c296565e106696b6f23be5e76f9908b0df50dfbf2477.png)  
![picture 17](../assets/images/e72a24947a53e250108f880a71b141c56649f8665233aa3f72e4aad3648dd319.png)  
![picture 18](../assets/images/3a9b49885da183807f624081f35581c8bb597b479a0af01d5023d139bf160209.png)  
![picture 19](../assets/images/49c312ba69824feb803e35595e1df1117c97b3ce47737a2b038aeb400045f59e.png)  
![picture 20](../assets/images/71b4a1b011f7914fd0cb4cf7d931e58d546d106fb516f7045f4e8f898a3ce791.png)  


>试了很多唯有ssti出现需要响应，而且不能使用+测试
>

![picture 21](../assets/images/6a7499f82188e171c2437e4476ff3fa1f198c323b313ba70bd1825bb9c26619c.png)  
![picture 22](../assets/images/aad5e927a156231e2f9dd0ed301062a784628a663173732e167d94cb74f18399.png)  
![picture 23](../assets/images/d2f00f3a164769ff56a33fafa11754940a1596d6909079dd6af4c992dcc4b3c7.png)  


## 提权

![picture 24](../assets/images/20920d249472d6946e0ae011a42eefebca57b3036a7bfc470200002afb10d4b2.png)  
![picture 25](../assets/images/2c059ef97e6713445fbf42506c47d205d03e243a46cea9bfa1c48f3c2bb03f42.png)  

>单纯一个命令执行，但是需要走点过滤
>

![picture 26](../assets/images/19cffa101fe01f42287e20da46f4ac266fbdffdb99271efb27b482207b202ba3.png)  
![picture 27](../assets/images/01b3cbced609797939284ddb58c04e37d8a1c5c17f3d3c92a71e090d59d30460.png)  

>想想有啥可以直接命令执行，除bash其实可以使用echo或者wget
>

![picture 28](../assets/images/01f7a9c7272f2c0b1d30be2bd62aac1791d0c45aa21fbb6e63b8d95ecb86fe48.png)  


>不能使用 /这样的话就无法直接扔进去
>

![picture 29](../assets/images/1efe4ecece0e501208f639ca3fdf0bba912a53f520647002d792471db5a6bf52.png)  

>为啥呢
>

![picture 30](../assets/images/9ed6d33a58480ae1d28c46600ba57e180c0270781132fdcb48b6d10f7be0502c.png)  

>王炸吧，哈哈哈
>

![picture 31](../assets/images/7123d14d6a36b5aab06f2cd8f742e49c25fcd0ec6daef4b28ac62d0392abb8f7.png)  

>可以但是这个终端不能用了
>

![picture 32](../assets/images/864a75365aa9f9f4dae224ce51f3acef2b348ed2570d46899a5a054ffd1061dc.png)  

![picture 33](../assets/images/e99d9484423f9be4e74cab17298c03473c16f754ea9a841a5cb1119d94ff42b5.png)  
![picture 34](../assets/images/645cf10fbdae370aad1447ebaf300eb2351ca4941871a8e9582ca7c6117635cd.png)  

>没权限是什么东西
>

![picture 35](../assets/images/132390d3a86c218446e2fac2e49024b01e88513c0ea65bebc7f20a855e922b90.png)  

>不会apk逆向哈哈哈，完了看看还有没有其他方法提权
>


>userflag:
>
>rootflag:
>
