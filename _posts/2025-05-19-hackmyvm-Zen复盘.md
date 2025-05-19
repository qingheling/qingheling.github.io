---
title: hackmyvm Zen靶机复盘
author: LingMj
data: 2025-05-19
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.47	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.096 seconds (122.14 hosts/sec). 3 responded
```

## 端口扫描

```
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
```

## 获取webshell

![picture 0](../assets/images/0967df32909c8a3c8a95c3edfe0c9ba958613dc58ae35bfc00149842c2f360b6.png)  
![picture 1](../assets/images/10480fdb7639525685d6b289037d3eb61de3cc8f07dee79f2a60cde9674e6184.png)  
![picture 3](../assets/images/8d8c6827ba108fb8c13286f6f54b9567a1f39dc7298d0a2cddefff6a4cb31d0d.png)  

![picture 2](../assets/images/b0a66cd1ce0e0614c3fc286a21f9c3c3084796755a2a888cedfe88d17fe94379.png)  

![picture 4](../assets/images/c5c7048df6a39cd215f1a0196b6c29a376ed2b75653ecca2706ccf0de62f4ebd.png)  
![picture 5](../assets/images/9c79bd572b623780e1579d739285700c890c109e5c62610fff3bae71f60665a4.png)  
![picture 6](../assets/images/431eab7f243e559dc47b87601c9978ad634228cdd1167b927169b1652c1b8ed1.png)  
![picture 7](../assets/images/80c0981d1e30111a9dac151e83e180ee9f515f76b111ea9d0393e382158f4c77.png)  
![picture 8](../assets/images/edc89120aeab0ac5cc1444fbfafd2085c4c26ac61d83d5b085650e32d1fc6f59.png)  

>无sql看看怎么处理
>

![picture 9](../assets/images/39e91cff7a23a1af3c682e47d9c5eba7495af6b1b378992c0c775d137414527a.png)  
![picture 10](../assets/images/1e29402f74d6e8b043684fd93c19250de97fe55a7cbcf46630c5e890a99f5789.png)  

>感觉是xxe
>

![picture 11](../assets/images/7a4ce1d14d9358ce8f1f1a1e619efd8a9abf525c41d8580ee3259d2f75fe3ddf.png)  
![picture 12](../assets/images/b2eef19b532e882d7597b34376a1da28dd925e93eb630cf718368e27c77f6eb7.png)  
![picture 13](../assets/images/aed97e13fb3e4d0558ebd6ebef03fb22a117b72e2970e2620fdeecb95fd01271.png)  
![picture 14](../assets/images/f96d034d9c4f071b7d5860e5e989865ca7acadc93cdbcb20ebe3322b713c8a5e.png)  

>啥都没改，直接按照这个文档操作：https://github.com/F-Masood/ZenPhotoCMSv1.5.7-RCE
>

![picture 15](../assets/images/747273caa6041c963707fdfb57734639ebaa49f7bed839ff2ca0aafc633e9ff5.png)  
![picture 16](../assets/images/aaabc0305c32294bb0ed1ae53a929756c64f0c60a32688b6f69af1ac0362a8c1.png)  
![picture 17](../assets/images/4509955897ea907f1cd43d1fcf5cac23707efdfafcb22cbfb8405d3d6820e592.png)  

>拿shell还是很简单
>


## 提权

![picture 18](../assets/images/a1642f22c62f87de65e61cf5e6ff148af8bb8d9d2cd4d33561730ef7fe76ff61.png)  
![picture 19](../assets/images/b54dbfb3ed82d3b1626f3f36aa5f27e3d54c1055d6f8be368677c3323363bc4b.png)  

>没线索密码复用试试
>

![picture 20](../assets/images/c111077eb8aed11811b6de43a6d1dbd9abda6694638cf9c102b42ac354d91bb7.png)  

>不是
>

![picture 21](../assets/images/e9ab4b5fafb30ba0573c5e96b1d86b07750105af3cae4a8905cab090f68e280b.png)  
![picture 22](../assets/images/2fce9fc2be56e14b6092d0833b3b54e73dffa60c59c3ca9249501112626da28a.png)  
![picture 23](../assets/images/2ff500bfbf428da2751d3829a75ced05a621be480f651f1beb441f68196c688b.png)  
![picture 24](../assets/images/669dec38bb624e1fc983edb1579585f475c4c8351a109471f7d4c03775481a09.png)  
![picture 25](../assets/images/94d8e4108e32d0c72fe48311d6fecc7bddfe4aa483c1435b936652f135075f51.png)  

>mysql没啥用，没有密码
>

![picture 26](../assets/images/d90690d448eb971671b2f09429a0f24e821ff5265c61a9336a3c2f78f3c292af.png)  

>无定时任务那我咋利用/usr/local/bin
>

![picture 27](../assets/images/187ad38e2c58cee5c6efca482f149b8adb40ab8d8f29cfb83a5aac484ebfd7b9.png)  

>跑一下密码
>

![picture 28](../assets/images/fe05be9a1c67c491815691955af4cf62ee74ffcb79eab9da0f419a621a7d3b88.png)  
![picture 29](../assets/images/6b243504c351d79e46ec30dbd7b45c25f94e10f67a97211a4c72c495f0a139f0.png) 
![picture 31](../assets/images/2660116cf3d80199145d702510a0e4007e92d5f3b45d4d7d9bd9373f60a66f68.png)  

![picture 30](../assets/images/aef01aa2dc36f71e7c03d57ba1ee5c3530ebb7b430affc66339c97d4433e5362.png)  

![picture 32](../assets/images/36bc5db6c251a6ddf624149cd359fe76716ec5aec98e1d3c82c739430138510e.png)  

>不是怎么干哈哈哈
>

![picture 33](../assets/images/fee10e391392b2069b0bdd9d1c9cb1112dd983741860d283b85f45d2bfedf38f.png)  

>我推断环境劫持
>

![picture 34](../assets/images/30493468658bdb05e851612df4c45e9e270c622f7fba8659993053ea124004f2.png)  

>推断成功
>


>userflag:hmvzenit
>
>rootflag:hmvenlightenment
>
