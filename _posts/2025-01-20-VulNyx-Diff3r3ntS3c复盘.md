---
title: VulNyx Diff3r3ntS3c靶机复盘
author: LingMj
data: 2025-01-20
categories: [VulNyx]
tags: [upload]
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
192.168.26.181  00:0c:29:7e:50:28       (Unknown)
192.168.26.254  00:50:56:e8:96:d1       (Unknown)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.935 seconds (132.30 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.181                
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 20:40 EST
Nmap scan report for 192.168.26.181 (192.168.26.181)
Host is up (0.0012s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-server-header: Apache/2.4.57 (Debian)
|_http-title: Diff3r3ntS3c
MAC Address: 00:0C:29:7E:50:28 (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 61.08 seconds
```

## 获取webshell

![图 0](../assets/images/1c10f781818c9ca23b7a4e6d3b49395270fbba2e6f585ea4de779122e3f2c049.png)  

>存在上传点，毫无疑问这个是个上传的靶机
>

![图 1](../assets/images/4706847e5c33fa62d910606221ad294e287afa746f47b14f6b04e67b2b9e0485.png)  
![图 2](../assets/images/02cd0b9399571a9ee0069c5f7bd832ec28fb55e304f0742b95bd50eb8b4b395d.png)  
![图 3](../assets/images/731e75b867deedda25df7193fc749c6b03b2145b7107e536059a9f695acea736.png)  
![图 4](../assets/images/d731d7bcac0fdb1cf79f365796de4d0149f665df356c88a2ed0e094ef86ac5c1.png)  
![图 5](../assets/images/54b69634b801763fbf9c9c2249d14aa568f690cb7debd50440fe9019dacd03f0.png)  
![图 6](../assets/images/589ca20e5c055cc80f2451f326e54f259ed125bd847d95784664b86155667952.png)  

## 提权
![图 7](../assets/images/53693f9843b58fc13b3252a88cfd15334c9b0817d81e275aefb0e11203b377d7.png)  

>不存在sudo -l
>

```
candidate@Diff3r3ntS3c:/var/www$ ls -al
total 12
drwxr-xr-x  3 candidate candidate 4096 Mar 28  2024 .
drwxr-xr-x 12 root      root      4096 Mar 28  2024 ..
drwxr-xr-x  5 candidate candidate 4096 Mar 28  2024 html
candidate@Diff3r3ntS3c:/var/www$ cd /opt/
candidate@Diff3r3ntS3c:/opt$ ls -al
total 8
drwxr-xr-x  2 root root 4096 Nov 15  2023 .
drwxr-xr-x 18 root root 4096 Mar 28  2024 ..
candidate@Diff3r3ntS3c:/opt$ cd /var/www/
candidate@Diff3r3ntS3c:/var/www$ ls -al
total 12
drwxr-xr-x  3 candidate candidate 4096 Mar 28  2024 .
drwxr-xr-x 12 root      root      4096 Mar 28  2024 ..
drwxr-xr-x  5 candidate candidate 4096 Mar 28  2024 html
candidate@Diff3r3ntS3c:/var/www$ cd /var/backups/
candidate@Diff3r3ntS3c:/var/backups$ ls -al
total 16
drwxr-xr-x  2 root root 4096 Mar 28  2024 .
drwxr-xr-x 12 root root 4096 Mar 28  2024 ..
-rw-r--r--  1 root root 6765 Mar 28  2024 apt.extended_states.0
candidate@Diff3r3ntS3c:/var/backups$ cd 
bash: cd: HOME not set
candidate@Diff3r3ntS3c:/var/backups$ cd /home/
candidate@Diff3r3ntS3c:/home$ ls
candidate
candidate@Diff3r3ntS3c:/home$ cd candidate/
candidate@Diff3r3ntS3c:/home/candidate$ ls -al
total 36
drwx------ 5 candidate candidate 4096 Mar 28  2024 .
drwxr-xr-x 3 root      root      4096 Mar 28  2024 ..
drwxr-xr-x 2 candidate candidate 4096 Mar 28  2024 .backups
lrwxrwxrwx 1 root      root         9 Nov 15  2023 .bash_history -> /dev/null
-rw-r--r-- 1 candidate candidate  220 Nov 15  2023 .bash_logout
-rw-r--r-- 1 candidate candidate 3526 Nov 15  2023 .bashrc
drwxr-xr-x 3 candidate candidate 4096 Mar 28  2024 .local
-rw-r--r-- 1 candidate candidate  807 Nov 15  2023 .profile
drwxr-xr-x 2 candidate candidate 4096 Mar 28  2024 .scripts
-r-------- 1 candidate candidate   33 Mar 28  2024 user.txt
candidate@Diff3r3ntS3c:/home/candidate$ 
```
>看看这个backup
>

![图 8](../assets/images/e1749ee19c77bcb07ab244d145df1cef4f05c70896a1c0e63a918c167e6922dd.png)  
![图 9](../assets/images/e6ab28f3dd61fa50aceffe089c726944382f3cd84dc133f42c9d08b603efb413.png)  
![图 10](../assets/images/75416a0e8648a079e8b5f92175b36b22d6ea40689f13e720e711d988e150fd6d.png)  
![图 11](../assets/images/c7a89f5ea89288b5535f46d15e9994059ee58cc3592c88b318a6450a1d27e6ed.png)  
![图 12](../assets/images/90e823e4990e7fe1de67d7037a9a0787e08df9744222bf37e2517877fe34fd7a.png)  

>这个单纯是上传的打包地址
>
![图 13](../assets/images/233fee549d547a687db7112b0eb986efb17aba769f4b71d8928dbcd24fb41fcc.png)  

>用工具跑一下没啥想法
>
![图 14](../assets/images/af859b2f5b56e6dc6d814fae510cf8678874c00cf5e88a0c568c39b83033beaf.png)  

>等一下好像存在定时任务。这个打包的
>

![图 15](../assets/images/26bfa599d899642ce68bd1001850fe9727638f410d01f4d2b746dc9cd63e112c.png)  

![图 16](../assets/images/ea6b425ca8f4383af0d1172bf26d671171ef670a337a75536cb6700515a4c1b5.png)  

>好了王炸方案即可
>

![图 17](../assets/images/606284b67793f3b063b2078a040a6969652b45c8875ba655e3d6c13e740115d2.png)  

![图 18](../assets/images/3c7b6c6346214c0e7ae6a85679a957fb06ffccaf995824a8c1e00142e35ab73c.png)  

![图 19](../assets/images/4b416fd1f14de9995cdb79dd0619691bd821ce1260174b2a9b976c08a288262a.png)  

>好了到这里就结束了
>

>userflag:9b71bc22041491a690f7c7b5fe0f4e8d
>
>rootflag:24886c4b2777d4359cd3dbd118741dda
>









