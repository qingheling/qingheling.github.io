---
title: VulNyx Bola靶机复盘
author: LingMj
data: 2025-02-08
categories: [VulNyx]
tags: [rsync,md5,wsdl]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l 
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.206  00:0c:29:ef:43:73       (Unknown)
192.168.26.254  00:50:56:f7:38:63       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.885 seconds (135.81 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.26.206
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-07 21:49 EST
Nmap scan report for bola.nyx (192.168.26.206)
Host is up (0.0012s latency).
Not shown: 65532 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
| ssh-hostkey: 
|   256 65:bb:ae:ef:71:d4:b5:c5:8f:e7:ee:dc:0b:27:46:c2 (ECDSA)
|_  256 ea:c8:da:c8:92:71:d8:8e:08:47:c0:66:e0:57:46:49 (ED25519)
80/tcp  open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: VulNyx | Offensive Security Playground
873/tcp open  rsync   (protocol version 32)
MAC Address: 00:0C:29:EF:43:73 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 88.34 seconds
```

## 获取webshell
![图 0](../assets/images/59201d4899afd80f6be5c056e0b83e8463ec9a21f14aae8a749f9ae0c533c390.png)  
![图 1](../assets/images/879f1dab977548a17c9cb15bc197281c2c60e835988e5c71dec174e693a89bdb.png)  
![图 2](../assets/images/012a6a95e50d4356b9461b850aa7a4ec1c7fb2c382ff560e69ba0e85ca266b09.png)  
![图 3](../assets/images/989f5f426d37b9a80b96a2295e8e3ece07177bd10eff4eace3048cbd78e72875.png)  
![图 4](../assets/images/c3713e70532ac92332e14e17ff07e574fd058f386e141bfb58126db4cda2bd52.png)  
![图 5](../assets/images/62f65aa13d5a41aa81b61f034f6b46cdcee88290e0a7b2211bd0561a1a0e9ced.png)  

>登录没用
>
![图 8](../assets/images/f40c24d318a40a1f27724e8fa70d469e5496ad9a10e78373dac919a131b85e96.png)  

![图 6](../assets/images/c5a44114761eb5812b79cba01fa537936ca8ab23edba3a92545082b262e9bbbd.png)  
![图 7](../assets/images/f1353a8f5b75c7afa50c26cf67c22c721b5cb11e37917cba61b3919f58578d4e.png)  
![图 9](../assets/images/073ec14d48e161b532e5d961ce9c2f407f679c5aed1134638ba54057ad608687.png)  

>到这里web目前的信息就没了，看看873
>
![图 10](../assets/images/120be043f4480ea6375a0cb0934168cd6f7c0e0d937dc440068da8bf7f911848.png)  
![图 11](../assets/images/351fce4b868ce8c612a66375f6980f6775a822e5540e582625ee3c3a42017472.png)  

>没有东西爆破一手
>

![图 12](../assets/images/d2bd2faece6110fd57ae3f724f2364c675a1f7a5a258f24405cbc45902ca8914.png)  
![图 13](../assets/images/d9693138683625a153149c4afced00e6a65b2438397c051044c7d83e77dd63ae.png)  
![图 14](../assets/images/e2a16a1328735a0d59cfd7def6e3529170a6d55d3a7109a00051e1d6e9cae323.png)  
![图 15](../assets/images/c20ed8dff47e899d1c96fa26359d15ef2006668ba232ca24ca16e600a9333189.png)  

>思考一下怎么拿下来
>
![图 17](../assets/images/9c09b49380caa81a4443bb9c425bc081b5dc868f4ad40c218f516dfa81942b4d.png)  

![图 16](../assets/images/ce0d533b2ac6bc20a43f5e43c473887b9f333b385e7904f58df3801c7b37b976.png)  
![图 18](../assets/images/9ed4c1228055d38493e2a4df601c990060a898e0284e35168b43b16363e2bd8a.png)  

>有用户密码了
>
![图 19](../assets/images/e345691aa107ecfb57b6e315a786a55f54c85588145e1a047c167d180f44fe19.png)  
![图 20](../assets/images/0ddffc54c313082b51e8fdae20263ed0f5188183c19258a001e2aaa3913a7fda.png)  
![图 21](../assets/images/fabd555670c4b5d42c3170775e867d7248246746caaa000ce03bf7b693ce2259.png)  
![图 22](../assets/images/57f53b04dd70da160c2f5f149d4536fb53498ad10476a9866a0a02c8e4e16984.png)  
![图 23](../assets/images/4fef3e51b28a6b93566b50ec3deb5af914f8c213bdc69ce9f7e029f31bd131e3.png)  
![图 24](../assets/images/749f6f3ceec8821aa5353a3e940a31c7b22cd449b6e6f73bfaf58eadfea1cbac.png)  
![图 25](../assets/images/fa8e2b63d444ac3b0aedfa68279b65e908a260e9dbf2b6e88523fb5184728a7a.png)  

>一个pdf
>

![图 26](../assets/images/a366f5405627937d9556c2986417b4e21ecaaa6a299e657c2dba7d4b62e0760a.png) 
![图 28](../assets/images/ca4aa1de51ff3bcb77c843a1841026ce341a031c5ebeb538868a62097a9ee420.png)  

![图 27](../assets/images/f04d39506d1c9b7a7717c5c132de250df796b28d27dd3b40038b2fbdb9dbfb8e.png)  
![图 29](../assets/images/ebd6b6671e54175c75bf75101c07f3592c745279942a131717861a35b0a2de77.png)  

>好了拿到密码了，不过用户名是：d4t4s3c：VulNyxtestinglogin123
>

## 提权
![图 30](../assets/images/b8ec8df2bc90cbdd50ac70f83491c7af23341c6b4d5b7c707045ecb4e39305a6.png)  
![图 31](../assets/images/6536bd507cd0325c59d3d70edc565bfd6924cacd84d145f7ac6bac2f564a9e58.png)  

>一开始我以为是mysql，但是不是什么都没有里面
>
![图 32](../assets/images/acd8516df2a349b5f462b75ec37a8f52a8c11f9358b7cb49077981ba296aa158.png)  
![图 33](../assets/images/cf0cfd1f50a5ad7c9d2e843d50e8d1cb2e1d772947ecdfe37ec83cf04b6cee2c.png)  
![图 34](../assets/images/9d4e7035b88700b8b0569bf4177a312d7e424dab868c5c24741dbd5c88640ca2.png)  
![图 35](../assets/images/dc559ff4c29a41912d8b8d20ad29c12980c7f68d98613e70ec7b32c77eb2a06a.png)  
![图 36](../assets/images/737394f994f5934526e78b0bc634d2292d517335c8dce150a30962c1fc5b2412.png)  
![图 37](../assets/images/48ea9f4e1eeb115abe9c4b14211b75397245f91b48f18349c77cb1492e350795.png)  
![图 38](../assets/images/4ab86128357faa34525835702022d2af9aa1650a52599bb39a344f7339a48910.png)  

>结束
>

>userflag:4e62a268197ebd869b7bafe859e35d00
>
>rootflag:8930fba2c5f4da4e76ceb626f8f5454a
>