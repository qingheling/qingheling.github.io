---
title: VulNyx Friends靶机复盘
author: LingMj
data: 2025-01-18
categories: [VulNyx]
tags: [load_file,MariaDB]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.174  00:0c:29:99:71:a7       VMware, Inc.
192.168.26.254  00:50:56:fa:a6:d1       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.498 seconds (102.48 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.174       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-18 09:11 EST
Nmap scan report for 192.168.26.174 (192.168.26.174)
Host is up (0.00076s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
| ssh-hostkey: 
|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)
|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)
|_  256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)
80/tcp   open  http    Apache httpd 2.4.56 ((Debian))
|_http-server-header: Apache/2.4.56 (Debian)
|_http-title: Friends
3306/tcp open  mysql   MySQL 5.5.5-10.5.19-MariaDB-0+deb11u2
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.5.19-MariaDB-0+deb11u2
|   Thread ID: 7
|   Capabilities flags: 63486
|   Some Capabilities: Support41Auth, DontAllowDatabaseTableColumn, ConnectWithDatabase, IgnoreSpaceBeforeParenthesis, Speaks41ProtocolNew, Speaks41ProtocolOld, InteractiveClient, FoundRows, SupportsTransactions, ODBCClient, SupportsCompression, SupportsLoadDataLocal, LongColumnFlag, IgnoreSigpipes, SupportsMultipleStatments, SupportsMultipleResults, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: ks#OCEn|F&*=8@x\C{aV
|_  Auth Plugin Name: mysql_native_password
MAC Address: 00:0C:29:99:71:A7 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 77.45 seconds
```

## 获取Webshell
>查看web页面
>
![图 0](../assets/images/bf9db882eac063914497e05dea935e823df16b079345ec45c14a78b3cf3877e9.png)  
![图 1](../assets/images/3ac774e974c95e3e658babebc4e7e8c51e6b4d569145266cf0799697435c24c6.png)  
![图 2](../assets/images/d5fc6602ab4f6baba6adaea123bd96ec0ff07e23df84a1cd4572dbf0ebf93954.png)  

>这里需要扫描目录，或者看看mysql版本cve问题
>
![图 4](../assets/images/dd876525331b86ab21b8decac23789becf457e115417f6039bf263a8e30cbc11.png)  

![图 3](../assets/images/96f7cc196e839451ed0fcb56cf6d5c1f0d28f25d32ade3fdc2a3a7aacf8db9e8.png)  

![图 5](../assets/images/d76b48ec0beebab0b6f19dfe6e52e462a11b917bcf833eafab7bf5f1034513c9.png)  

>边扫描目录文件边看cve
>

![图 6](../assets/images/ecffded0d55c4cddd3e7506da8d37c2dabc13f7b8669058f2bdd65e11550342e.png)  
![图 7](../assets/images/cf4f5e0587c38632080ff23193b579f53485a6d2a0103ab5a60fe27272492dcd.png)  

>这里表达了好像可以进行代码注入，load_file
>
![图 8](../assets/images/bd6ff8fa0d679cafee841383123bc726f7cee1691e036d944f87ead4c3f89068.png)  
![图 9](../assets/images/f647d3e568ab32666d72af8e76cda8a5fa901eae169d691fc560e3ea956d66e2.png)  
![图 10](../assets/images/bdcb233940507d135d6c6dc8616e44001ebb1ef9796a6828c8a77d688685e559.png)  

>看了一下这个玩意好像是个动画片的东西，唯一的线索是名字Beavis and Butthead，刚好没有其他线索看看是否是用户名，扫描目录除了index.php没有了
>
![图 11](../assets/images/452b1b9b6fa1918b1dd8da0925f68187cea660534f5f7a7db4e2fdcf85400a3c.png)  
![图 13](../assets/images/0138aba72c61f911dc17fff3c7ddb11178ca1dc4e4f1062a0908a1b45794486d.png)  
>先爆破5000个不是再换吧
>
![图 14](../assets/images/83d256a21295aa91ca4501351654af4b98099087fbaa27b0c15e61131674282f.png)  
>挺快2000左右出
>
![图 15](../assets/images/1e4bbc9bb4f39245b8bc82879e7a3a8c1feb3c48aefa9074f21aca67a0481ae4.png)  
![图 16](../assets/images/034cd27cedf6834c59039d0f1f30fd89e726bfcde3f1e52867893daf00ce5a05.png)  

>这里尝试ssh
>

![图 17](../assets/images/6919f2448de12c5b1101098eb44a9686a5383a9ce30cc146bc3c5d4632cbf2fe.png)  

>需要私钥，那换个方式
>
![图 18](../assets/images/e7b3272221a521ef34ef870563bc6e7002e6ff1fd4e61547f3fcc9c6e1471780.png)  

>可以这个load_file没有白查
>

![图 19](../assets/images/3c22e43e7cd927f504e6f6d94b63da2849009d688a5c094f0a23ade92ffe3be5.png)  
![图 20](../assets/images/105b23ca11b615c6d762c56a56496a2d7c669a0a2c740adcfd5719f703bd041c.png)  
![图 21](../assets/images/edfd3d539799228822060fe42b35f8721f5fc9dc41d7a5ac128ec6588f2a62f5.png)  

>感觉是不可读
>
![图 22](../assets/images/f3fbe9ba2c81e59b2253d190cb000b397c07337b9bcd9dfc9adfa77f92eee37e.png)  

>出现新线索
>
![图 23](../assets/images/fe6b4084114185643df7f80dd594c82e86b698f89a9df0417c1dccaa25be5e29.png)  
![图 24](../assets/images/b84f8927cbe91b8a3f4cc7ff22aed2a373531f473c7f63d9ac4a97dc2606308e.png)  

>我记得cve可以注入，看看怎么写
>
![图 25](../assets/images/aea00f3bc8ea0a5e445e50a18efe6b2d52ddca437283155b08712a711d851f9e.png)  
![图 26](../assets/images/21216a952273947ed09a225f5d15e3ea91c87df80131ebef8ea64b0082297f5c.png)  
![图 27](../assets/images/1cd9026bbbb9022c7a9854916bb349e2719bf003062cb01b35afa803fc8cd13c.png)  
![图 28](../assets/images/1d7ad383f9a8d5e64c443126f8bdd0fdad70e22f5d3dadf22b3bf49ef96cde76.png)  

>非常好，又学习新技能
>

## 提权

![图 29](../assets/images/36c1097c24304aee7d54b76e7da7853740e34285bbc54486a14da1d0c48684c2.png)  
![图 30](../assets/images/8ad9368a63f2ce577db78c45520cae54d0588acf127de78fed8a4286fffce100.png)  
![图 31](../assets/images/e71f7c2df56f2c64a67792e643f44b281b3cf1069b029b2c8d44f6af1d1b2ff2.png)  

>试了mysql上的密码都不对,看看切换用户
>
![图 32](../assets/images/ca6f11b23c2ff662525179066d2949385e7af762267186a53b39de6b5cabfa9d.png)  
![图 33](../assets/images/cd02a365070fd5a285502888643e5b3b107c4fc18bf8bc519e24ba4dfd6feb67.png)  
>成功了，后面没意思了
>
![图 34](../assets/images/8a6647072e373f15cfa4beb508a33cd3f1f7d2bbb48be369cafcaa46586ba678.png)  

>好了这个靶场结束了。
>
>userflag:df81a6fd60ceeba1268f587366c1c693
>
>rootflag:59cefd06522a7e8f3725fe3655550c18






