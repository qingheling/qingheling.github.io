---
title: hackmyvm Eighty靶机复盘
author: LingMj
data: 2025-05-16
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.18	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.066 seconds (123.91 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.18 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-16 04:46 EDT
Nmap scan report for eighty.mshome.net (192.168.137.18)
Host is up (0.037s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp open     ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 c9:ce:d7:2a:f9:48:25:65:a9:33:4b:d5:01:e1:2c:52 (RSA)
|   256 7e:3d:4d:b4:82:0b:13:eb:db:50:e3:60:70:f0:4a:ad (ECDSA)
|_  256 7f:9d:13:c8:7b:d9:37:1d:cb:ff:e9:ce:f5:90:c3:32 (ED25519)
70/tcp open     http    pygopherd web-gopher gateway
| gopher-ls: 
|_[txt] /howtoconnect.txt "Connection"
|_http-title: Gopher
80/tcp filtered http
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.12 seconds
```

## 获取webshell
![picture 0](../assets/images/cdba576e18ed69dc94cc4b8e2e23951664ebd4746d8608964ef14f3684681304.png)
![picture 2](../assets/images/0fe6543804838117ae916525e136419e43b83d1c05135ed8e04edb15bd0908aa.png)  

![picture 1](../assets/images/844d0ad708395d8915ff5f9e8160bcf6409f98404f9a804a326b544914152ce7.png)  

>存在域名
>

![picture 3](../assets/images/aa45b97129250151356b3594af4d21c582166a9ae03ceac744bf6840a82eff95.png)  
![picture 4](../assets/images/3a2e66e5d95fc30546cf9ae0b2a8e42e014ced1083a578aaa25b00d651d53ea0.png)  
![picture 5](../assets/images/133f5412ca929bc225492513c3be1cc8b7e65e8b531d54a80d42ae8425273512.png)  
![picture 6](../assets/images/2647e13856e091c72ce2233304dead4caca9a4aaf705ff6c780f2bb12b2f5f1f.png)  

>像是源代码继续找
>

![picture 7](../assets/images/49e1ffe3deb0c042879d3d0d9dc9c6ac8c830517e23c518dbb44460c37747d91.png)  

>xxe么找个注入点
>

![picture 8](../assets/images/0c20f2b9d916b323cb0ca06a1755db846c81e58eef4c3ec1e0d3cf0955126f3e.png)  
![picture 9](../assets/images/ab57f3cc7af787d8ae2fbe0a598bf68132499e752492985b56913bf83adaab2e.png)  

>没啥有用的
>

![picture 10](../assets/images/97c7ec71905bacab5921b9469683b0052127d5dd80a0886c9a30f36e62328f07.png)  
![picture 11](../assets/images/2d4b3175a2ebc6a039989b2dc38e3c7677817d656ba687b2b9424c4ec0e0353a.png)  
![picture 12](../assets/images/c60641455d223aed54f1dbacc32227940d22c6f00ad59e4a5334954b3625a642.png)  

>好了出现了
>


![picture 13](../assets/images/42733ce8e661e155cf4b24107e52e9a0e3f204bddf8a1155214fa9c9f3708248.png)  

>为啥浏览器访问不了
>

![picture 14](../assets/images/a57cd3ac431798e7bd06956c463b1a3f140e0f37d7f064b49452bb8ebfd816f2.png)  
![picture 15](../assets/images/1616bd113c79ef4b8697878593729f8c1edd709ca79fff672bd95c3288907b2e.png)  
![picture 16](../assets/images/2625852f9b442e6b13e159a14a3ac548ec52f5ff8a2df35eabe2eb2fcfd048dc.png)  

>目前没啥有用部分
>

![picture 17](../assets/images/28a0c83d6f2cdcb7bb8e09651e9340bc53321c6e365e09573e464c0143f84cf3.png)  
![picture 18](../assets/images/fb6c9024b3b9f6a431589b80e2d52217c95d3b33797622e8458cc8b9100c3bdd.png)  
![picture 19](../assets/images/d38bddf0483c66bb8eb8aa870cc60cec69827f911aea8ad3cfe59a3d3066ebf6.png)  
![picture 20](../assets/images/cb189a59ff17a43e9e3d9600c3a2a34b0549a992f6ef8418e52d0ab5c385cc58.png)  
![picture 21](../assets/images/0739032471a596e1fec093f81d1a1250df90d643730ffaeb5633e4f45e09c164.png)  
![picture 22](../assets/images/0173693070197975ff0d9911cd578caac6cb3189c094974314aa4f714caf31fa.png)  

>没找到密码
>

![picture 23](../assets/images/6846ff635c232abd238a85acb29113cb37d7f5e7e15939d706874b582c070b2d.png)  
![picture 24](../assets/images/9b90b657690243439efedd60940f223eebf175f040d6f2eb3c4abb2b8d25cb2d.png)  
![picture 25](../assets/images/78c53fabbbdeb882025d075aa3255020ead29233d6a50c911e42ce3b5a042da1.png)  
![picture 26](../assets/images/24a69a76f2628d84bab14b51cba6748ed05d180bcddc5503062183dc0d2a364b.png)  

>不懂但是应该是什么认证密码
>

![picture 27](../assets/images/0b54a879738fde1f900d8cbc024e467d3d697e7d6668b2443b6552a691288a40.png)  

>看了一下wp原来那个才是密码下面这个是认证值
>

## 提权

>认证密钥挨个试就行
>

![picture 28](../assets/images/12a6776628e1277fdc13661b7969608ae1c4783527a8b7afcfeadb16c2a62890.png)  
![picture 29](../assets/images/58135f8b43500a1daa89136f4e9d03a344ea4fe5acc1f2c361d3ca22a265a91c.png)  
![picture 30](../assets/images/9fe230fa9863cd26346ec0149f4935cbd6e378800330ebb0e4d1f85ee94357aa.png)  
![picture 31](../assets/images/0bd9036b173bb3c1f39320ac02e7d5ed31ac29a687400b0b2f0cbf43d1266b3b.png)  
![picture 32](../assets/images/98230245f65a80959b2f242a5f2ac360ddcb50184cc3caf85cde3397b0f972ab.png)  
![picture 33](../assets/images/83e53ef39900a0d7587ed909aa92bbb5676fcf9c66ec8e757e28c1c0e655eed1.png)  
![picture 34](../assets/images/99d5724293447508806f20ada1dafb8ea936252898258f6f20a4547cf484d48c.png)  
![picture 35](../assets/images/39344860c38c10896077464616a2f6abd03ed3ce3bd373ea9fcf003a438d8f0f.png)  
![picture 36](../assets/images/27c520cdcc813a51459229d90534b92859c122697ad2f1e9ddffe6ece7c9834a.png)  
![picture 37](../assets/images/5b297d0bd02f6bcd8026efe762d6d47377e6968835e21755e490ad03dec41a49.png)  

>无线索没有头绪
>
![picture 38](../assets/images/25713d6d53356aa0c645f5e6c93234d70123aa5b5d6943a1e19eb76f36e567c9.png)  
![picture 39](../assets/images/1d1c4882b64937060f3cb0ce6f61233d14fd4748636a49f7a2030ff72945ca0c.png)  
![picture 40](../assets/images/0ab4c2e986f3a2032c8b4be3f89c9ae83e5f629002d36d570cade70db1699a60.png)  
![picture 42](../assets/images/92466bce298490be5b1a780040d8e1aff25fa4522d2a5a574713232878abffb9.png)  

>需要输入密码，密码在之前位置
>

![picture 41](../assets/images/6dd939e45fdfe67e3763ac200bf4b7c7c1453f8637ecdf4c56b61ff68b6cbfd4.png)  



>userflag:hmv8use0red
>
>rootflag:rooted80shmv
>
