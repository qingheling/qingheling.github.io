---
title: hackmyvm Diophante靶机复盘
author: LingMj
data: 2025-05-18
categories: [hackmyvm]
tags: [LFI,LD_PRELOAD,ping]
description: 难度-Hard
---

## 网段扫描
```
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.159	3e:21:9c:12:bd:a3	(Unknown: locally administered)
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sV -sC 192.168.137.159
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-17 21:39 EDT
Nmap scan report for diophante.mshome.net (192.168.137.159)
Host is up (0.037s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp open     ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   2048 34:55:b2:c3:59:4e:b1:e5:dc:47:bb:73:f6:df:de:43 (RSA)
|   256 5a:c3:b8:80:53:27:8f:b4:ef:27:89:c8:e5:a6:1f:81 (ECDSA)
|_  256 08:46:e6:ba:d3:64:31:88:e7:d3:66:94:ce:52:80:35 (ED25519)
25/tcp filtered smtp
80/tcp open     http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Apache2 Debian Default Page: It works
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.36 seconds
```

## 获取webshell

![picture 0](../assets/images/aafbfeab0c6128b6961c98001de16056568cf971312ae21842e91a581a88e40f.png)  
![picture 1](../assets/images/c7140a1a356cee874e13bcb139891c04a1e4748a4941a29551b59627b77ff470.png)  
![picture 2](../assets/images/6f7c65b6ef667fce89a07295cca655a27a4c91cbe6b230bb822086f3e3d3494f.png)  
![picture 3](../assets/images/5d291cf5849b274a1fa2c7a2b4f9edb021053b09e2fcd93c23249fd19361ec5a.png)  
![picture 4](../assets/images/120c9553cb63c7ea7e3cb3ff1a8af9d38207d3015e15eadc51896a0f8b106ada.png)  
![picture 5](../assets/images/3fa12ff4382d05934fa12335c5f22495e4c7c1575bd825742d21c2f39ae6e2a5.png)  
![picture 6](../assets/images/8c964f23c3577f391fad6189d7d3de34333cb2279ba367f4bbe6641bbf930648.png)  
![picture 7](../assets/images/c06a4bc51f6ad5dd8cdbe104e760c50fc641836376ab40ea8b634698d4a1fa21.png)  

>没用户么
>

![picture 8](../assets/images/fd4e586f92452c7696d08de5131a0ee313b32a6d70317f6ff9a057ca65fde5cd.png)  
![picture 9](../assets/images/3451302da830591514b36a58b29f76adafdb57e9738f3f9b288c0fbd936fcd2f.png)  
![picture 10](../assets/images/a1d2e5d0baf02faf073f28ca19bbd9ee3fbfc29e9fce2ce4e7a2f430bcf45a8a.png)  
![picture 11](../assets/images/f05c38028c9b7136b64fb351c26687d659f474961df4d9dbc53aab0f53a7e8da.png)  
![picture 12](../assets/images/52d4e88db7b37c4c3e65548f0c92c63e2efcc2c9bb02b761c66a04fe3a8de848.png)  
![picture 13](../assets/images/d0f1a99020920369b051caff0ee9fd768b8f9bea1c048b908f9ff577634b7996.png)  

>还没成功，大概率没有密码
>

![picture 14](../assets/images/304ba234e651110e3e6ac25779086d11e158da04fdd390fce03800496b9007ae.png)  
![picture 15](../assets/images/605d800396aa0d28ca12df98a0458f015b5f198c983cec2946918b320d336bf1.png)  
![picture 16](../assets/images/4b6b0a40c00c1cdc464d6b69a65a4c6d605a02fcf26c835f4fc313e239305aba.png)  
![picture 17](../assets/images/a602e4f4d1b51bc2a69b3c91447ef6784e418ff7aabc7d21a2f42bb9bd8f0fe8.png)  
![picture 18](../assets/images/aed225b905e87b048cb6fd84dd428ff9151d97bbb66fe28f557eb98ee1d243ac.png)  
![picture 19](../assets/images/d7dd807dd2a48f2abbe0afd09a598fa5d2220c482922c1811c72ff070dd83331.png)  
![picture 20](../assets/images/63941a09d603ded7e0f905429f91dacf2b79c78f6f891964fe544f4092af25d4.png)  
![picture 21](../assets/images/bcb041e6fa72ce41a5ccfa46747c0071e1c86d12ab448cf2a811f18dca9c935a.png)  
![picture 22](../assets/images/9ffb39136d8194af9a71ff98d15f82939431661a3b8cfe97f7e5a85c68840e1d.png)  

>不见密码
>

![picture 23](../assets/images/4e59629e1784383fe855d87970027ba26fc0132dd9cd452caecd16eb21de67c8.png)  

>无法爆破搞错了用ftp了，哈哈哈
>

![picture 24](../assets/images/4b7126822109f16ef9893fa5bcb1f02187bb441f482568990ce7d21348ce2b0a.png)  

>不能目录穿越么
>

![picture 25](../assets/images/a5840fe795232dfa73b42c9e2a2bc3e9dd8aba90142d0fe053b5be45db24cba4.png)  

>利用25端口了
>

![picture 26](../assets/images/678ff4122146aa38dd25ee1098b40f2a86fe70feab4904c1988601b23c7205a6.png)  
![picture 27](../assets/images/14785a10a5675c852589e026e375d4755d15c491b012ce225f8f03b66b5f842f.png)  
![picture 28](../assets/images/3332578a2b6dd836794530234f1aa2f36631d0738fba34816aeea14fe6e674f6.png)  

>严格遵守原则不然不知道能否成功
>

![picture 29](../assets/images/43cd370e801f74432ea33843fa6b9a07aa9630fd625fc43a4e51cb0be0a90928.png)  

>邮件位置的话之前打过所以有印象
>

![picture 30](../assets/images/a848e4183f61970d4af457e1c618b836ede99a20be1731d108a0432dcfc29691.png)  

>这里停止
>

## 提权

![picture 31](../assets/images/c9b3b1e75faf96e2e350c79e27ce87a0ef050e30a14aaee14ad80337c1184eaf.png)  
![picture 32](../assets/images/924b7339c88888a3576ba85ba85cabcc01ab891cb1058877fe038fe72b4ab03c.png)  
![picture 33](../assets/images/d9fc88081e01980fa1521c44d65422e49324e704cc4078d35ba19a4f8f382233.png)  
![picture 34](../assets/images/51bac1684eadde9f4b0771dd006f91511fb9a0fa971b3641bcb89e64def0d19d.png)  

>没啥思路这个里没线索
>

![picture 35](../assets/images/c0a0ee76cb9f5d40977100ef8a405e9bc9c142023e9677ee6bf3eebf9f3b8387.png)  

>找到了doas内容
>

```
permit nopass www-data as sabine cmd /usr/bin/setsid
permit nopass sabine as leonard cmd /usr/bin/mutt
```

![picture 37](../assets/images/573167c2b4d10ee0fda6458e7860c8dd3772895179ba8832d1ac01c8d598b203.png)  

![picture 36](../assets/images/1bbb0c927d61cc7307e12ed07b8d7dea56b432fc8d079ffbf06bb8002d401a65.png)  

![picture 38](../assets/images/3c5ccc464303054cb957d206b72ebfa988bb3f6f707bec1c2e2b234800af69fc.png)  
![picture 39](../assets/images/41e2aa181cf751f91e3b5efd2b1ef07e23e2f3a3e80de592340584a770121e82.png)  

>-a选择一个文件一直按!/bin/bash进入nano直接操作
>

![picture 40](../assets/images/80762db342bfc2fb989f588088773c0217d98bc062e74e710ed96bc487fdb6d4.png)  
![picture 41](../assets/images/7a9c3a7f1e2e28d52377ec0ab9d9b12a1798b8ef278d4679639d888c3c5a5b3b.png)  
![picture 42](../assets/images/ff253156235dcf28e591a8fa32d40a39f74796c91fe170da95d3ac755ac0fdec.png)  

>全没成功但是我记得我打过这个方案
>

![picture 43](../assets/images/10af91493a63c0ea94ebadc83178e5edc7d2813589050ad90eddb569f62bee0f.png)  
![picture 44](../assets/images/6f27e2154727cefe7f51477f58e6002785e28624e9602aba44645ee22c3b4152.png)  
![picture 45](../assets/images/f35026fb250dd9c3633e4e16e484fe067ce5e2379e81651a42968c29f2f94d27.png)  

>好了结束了
>


>userflag:Thonirburarnlog
>
>rootflag:Culcelborlus
>
