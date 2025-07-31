---
title: Windows Always靶机复盘
author: LingMj
data: 2025-07-30
categories: [Windows]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:fb:0f:16, IPv4: 192.168.137.194
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.97	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.167	62:2f:e8:e4:77:5d	(Unknown: locally administered)

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.036 seconds (125.74 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- 192.168.137.97 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-30 09:24 EDT
Nmap scan report for Always-PC.mshome.net (192.168.137.97)
Host is up (0.0062s latency).
Not shown: 65522 closed tcp ports (reset)
PORT      STATE SERVICE
21/tcp    open  ftp
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5357/tcp  open  wsdapi
8080/tcp  open  http-proxy
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49155/tcp open  unknown
49156/tcp open  unknown
49158/tcp open  unknown
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 24.54 seconds
```

## 获取webshell

>目测没有80，有8080需要找点用户爆破
>

![picture 0](../assets/images/0ab8582981f06f132fa4c81b51e90c48e278fe7ddc4655a4b2a726adcc1e07f1.png)  
![picture 1](../assets/images/10565bd65decd256e0261c6f0d2048a3c7d6ba72297389e90a03794c730ca153.png)  
![picture 2](../assets/images/b90b41136728179371e8d2d77c92f21f3465b76ea8c1629388692ae465096f2d.png)  
![picture 3](../assets/images/7b493fdc64b09ba219f11727d22319fd1ceda706de4ea181e07e57c394dd5d31.png)  
![picture 4](../assets/images/2473c31da161921d830c49bfbc90197bff49287f1745d29ce36eae7e5daea26b.png)  
![picture 5](../assets/images/44d9c332397ca918b9f8c98fbd68cbfd870c08c144d32f30dee101e025060c94.png)  
![picture 6](../assets/images/43412cb0fd8010897d202ee31b37b0c4fafd7ba5fdfdd595e4fd7aabbe44c317.png)  

>构建反弹shell
>

![picture 7](../assets/images/ed01377699d0f0f8ce3f56e0d0485b477b78bf89825618b5a0a82650edd32324.png)  

>试了密码得用ftpuser登录
>

![picture 8](../assets/images/98f3b540e7cc44f9a4494c4f4ae873045ff287b85da47e6ae34ffd76cd270ba4.png)  
![picture 9](../assets/images/fee6287071e292e1f3c5be6f407068f0d6481b494275e38981252be8111aa3cc.png)  

>没弹回来我很好奇
>

![picture 10](../assets/images/3c3b7b5e8726e1a4415bc64fc1628ec36651b0c6423c08c21d76b4a817687ff4.png)  
![picture 11](../assets/images/969e5497a75b12add1b31a46cfbbb64ea2067cfc7c2c3975e65ffcf22d8bdee3.png)  

>重新构造也弹不回来
>

![picture 12](../assets/images/8b991e636acf326db7d9eca3e5b1f2968c83da92af95b2eb3d62089cb592a3e7.png)  

>原来是我点错了点击左边右边是取消
>

## 提权

![picture 13](../assets/images/628e9c10292ed84f604a332428327175da8d2a745c3d75d8dde6a21309f473b2.png)  
![picture 14](../assets/images/b1fe96327eded7cf689df9ac2d229c9e5558ae5d3b3e57391226ae7904ce4bf0.png)  

>可以了利用msf进行漏洞库扫描
>

![picture 15](../assets/images/98387e3e6c8763c59355fa2f432ec44db22a9d620253cd341db4a0f6ca326484.png)  
![picture 16](../assets/images/e0c9065a7e83eff277e340e78b6f36016f0eb38d0f7322892ada2b784e34b06d.png)  
![picture 17](../assets/images/1c903f6f526139080c5ba2b277e5f9864e67054b73bbbc111d7d309d6a1ebbe8.png)  

>是root权限可以去找flag了，结束了感觉还是有点难度刚开始windows
>

>userflag:HMV{You_Found_Me!}
>
>rootflag:HMV{White_Flag_Raised}
>