---
title: hackmyvm homelab靶机复盘
author: LingMj
data: 2025-04-25
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
192.168.137.135	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.232	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.027 seconds (126.30 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.232
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-24 23:42 EDT
Nmap scan report for homelab.hmv.mshome.net (192.168.137.232)
Host is up (0.035s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.62 ((Unix))
|_http-server-header: Apache/2.4.62 (Unix)
|_http-title: Mac OS X Server
|_http-favicon: Apache on Mac OS X
| http-methods: 
|_  Potentially risky methods: TRACE
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 68.65 seconds
```

## 获取webshell

>这个靶机对我来说挺难的，目测大概率会出现在hackmyvm，不过不重要因为我已经看过wp了首杀对我来说已经没机会了
>

>我呢按照正常思路走一遍
>

![picture 0](../assets/images/165ecd37242e327b6ec48ab8a5c31dfa904cca66ae8eb6abe4396837de5a6c60.png)  
![picture 1](../assets/images/1a643aa5e82da1469b8728053cc613246c0cdf03d6e59e4a7c15330ed9dfa3ea.png)  

>这里它是有东西控制的
>

![picture 2](../assets/images/b8cd10a065b4088050aa599be41f83896347775c0a35afced23c0ecd2098541c.png)  
![picture 3](../assets/images/51ee8c73a8b85dcca830b613c243a58f4c93a52024f2cca1f6078a68cceea072.png)  
![picture 4](../assets/images/99822e5fd4fcedb6ab687935d7afd1d131bdddbb423215e447838322a87a4f62.png)  
![picture 5](../assets/images/7b7c7e673cfce20aa3663b24f2c9b7efda53aba2a094df807584b6df33cdc20b.png)  
![picture 6](../assets/images/cbc18879550d3c18d11c06212fefe09a7313cb3ff5d5a0dcf7183628a5ba9b98.png)  

>还是有对应的trace
>

![picture 7](../assets/images/e842e94f8ab1a7645c1e0264d5508e8cb9865ac9f8bf8d32ab4f0c2a5d7a7071.png)  
![picture 8](../assets/images/3d2d3f807733eb21fc25be2a010d6dda3338a3d1ef7c0c4f37d312b9f4eb7f0e.png)  
![picture 9](../assets/images/36d88345e450648284581b0cef2084c861161495e299a091053d494524737d10.png)  

>没事变化
>

![picture 10](../assets/images/b5334524e01b51f0dcbb41e81085ecbb32620889500a9c849cefd4a291151573.png)  
![picture 11](../assets/images/ec5532d3f91fc432ffbab3b8ef4f9f7e9a707256a5fb84c05227393653cf89ff.png)  
![picture 12](../assets/images/623e19f18831b0c1c5186def2b88ab0fa48ec0782fcc08585f1591f852734522.png)  
![picture 13](../assets/images/2bf6ede3f2011ae6c733b4a450342f9491c3a6a958b692ace500d3c3106fbf4d.png)  
![picture 14](../assets/images/dab04aed378677d3af48605b285f8df6856efc470d2c8c0223c22deb0143dc48.png)  

>不是我全扫没有，单独扫就有
>

![picture 15](../assets/images/51ecdc785824bc74f4c29adf3fbd21a3ec805bc3185a35042e673386bff91232.png)  
![picture 16](../assets/images/7236a73e1bc57e9672719a5ef6101629fbbb0740742aa885d45cdf80d823ac83.png)  
![picture 17](../assets/images/cd7287c2e800dce84e06836fd063b1dcbcb499f9867f72949b48fe4ad5935132.png)  

>利用一下gtp组合一下
>

![picture 18](../assets/images/f3a3862d39e9df67b7bb6edc094285b347c234f0d09d014224cf0b05756ec4ee.png)  
![picture 19](../assets/images/135512ce3dafbdf5625ac42cd023e8c1a899b03a255f3f07f1210366f0ae5c3a.png)  
![picture 20](../assets/images/d2589a1746e516ef4bb6e73f062fd29f204f05ff30062504cdab1981f5e82ec3.png)  

>需要密码，尝试爆破一下
>

![picture 21](../assets/images/3df1b85f20548a1cab286978a203ec8a6524f22b3c469f47ac2c62a9ac57bedf.png)  
![picture 22](../assets/images/93a73ba75f5febcb1a836694d56d08c43468fc2c902fb16ddf660ac46c1e055b.png)  

>没成功为啥呢
>

![picture 23](../assets/images/3cee48bfb768f19a2bb6d3d050e0c7634333816a9e6c01c293e114cfc5160bfa.png)  

>它说有秘密我想看看for能不能做
>

![picture 24](../assets/images/d0ddce3731e659465471cafec4c2b618eb2a91d4974c0cc5135d864c4550c401.png)  
![picture 25](../assets/images/399eeb60bcab185ed19c38eb628972e4575408c0510d46ab431ad57376dfda15.png)  

>不会写这个逻辑的破解
>

![picture 26](../assets/images/41bae89c27012c4a2ea9990c7eacf4dc4af727ee563b1d70741f3045f174a7ce.png)  
![picture 27](../assets/images/4aa9c2dd2f3a7ee0b5f96abf62c16f4c91423b117e7dcb56878e81fd8e992d8d.png)  
![picture 28](../assets/images/7106588ee1246d94d51ae09cf0a91f4c86553bf86494c4bc1592653cb22f3896.png)  
![picture 29](../assets/images/7bb87c181369962250d5fa492d7be9d7912dfe413ed57124521fe7328a0a06a6.png)  

>目测应该失败了，看看大佬的爆破脚本
>

![picture 30](../assets/images/a50ce72dc6147f2783d685788b7b40e41b495b950db6051cf8b722de7d249fe8.png)  

>大佬的脚本大概就是那个意思我直接扔给gtp看看能不能有收获
>

![picture 31](../assets/images/917acc797a6b702652f489d796b8f07b479c6a376f5dfc5bb24644a290209983.png)  

>半天没成功的话直接换一个方式了
>

![picture 32](../assets/images/4beee2cb2913cea1587b3cc46e9ac359ce8d83db1236c676391ca708db265c8a.png)  

>如果你觉得眼熟不要怀疑就是哈哈哈，gtp给的代码一坨用不了
>

>13376个了应该快跑完了
>

![picture 33](../assets/images/73235fa7b141077da31025dbe1850caed4d4fca95cbc4964b68543e102088ef1.png)  
![picture 34](../assets/images/33fdf32306d5019f77e89d727bdc67c28e5f422aeea2ec31c155a8558ca596df.png)  
![picture 35](../assets/images/2dc2dc5e7ea4bc4c57d2b894cc8232ac4ab20bba5b5a904424ff43b9017d4796.png)  
![picture 36](../assets/images/a2815f3320d9072a585b729cd46fb57d55539e642547dd6762f1bfd3910fc9ba.png)  
![picture 37](../assets/images/df045259edde4f127bc8fdd36151c144672b8f0b7249a066f87ec7ea5309dbe1.png)  


## 提权

![picture 38](../assets/images/63e2ffe3388d69e37fe81fc98943b3cde21fd7d70b5e216832647be29cf5ea92.png)  

>王炸了，前面太难后面压根没难度
>

![picture 39](../assets/images/29640ec479216471b7cb0bb0439c41740796d68a562e014b1975ebb74afc46e7.png)  

>结束，flag这回就不留下啦
>



>userflag:
>
>rootflag:
>


