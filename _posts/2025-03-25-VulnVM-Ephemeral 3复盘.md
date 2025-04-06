---
title: VulnVM Ephemeral 3靶机复盘
author: LingMj
data: 2025-03-25
categories: [VulnVM]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.71	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.197	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.249	2e:5c:af:d4:ea:c8	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.126 seconds (120.41 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.197
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 19:47 EDT
Nmap scan report for ephemeral.mshome.net (192.168.137.197)
Host is up (0.029s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f0:f2:b8:e0:da:41:9b:96:3b:b6:2b:98:95:4c:67:60 (RSA)
|   256 a8:cd:e7:a7:0e:ce:62:86:35:96:02:43:9e:3e:9a:80 (ECDSA)
|_  256 14:a7:57:a9:09:1a:7e:7e:ce:1e:91:f3:b1:1d:1b:fd (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.85 seconds
```

## 获取webshell
![picture 1](../assets/images/d413ccc31c9d44b62782db8a1826d7126c7d2a811b0af30e9a66e3107d7fb056.png)  

![picture 0](../assets/images/b3c8f91bdafc5d540f7373aa7acc205bdb6aaec635d97e510c0928cde81b52b2.png)  
![picture 2](../assets/images/a56e529003964b643172daa8385f39bddeff5d9e3e5ba841818396d9f36a2cbc.png)  
![picture 3](../assets/images/37c6017dff85492d842ee0eb7be8b89a91866adcaf0b3207439bcb8718d68943.png)  
![picture 4](../assets/images/adba86b5f078e6eb4995067c43a483fb11cfd62aeb27b9a045b1ac4b4e36784b.png)  


>这个在hackmyvm就出现过的题目，重新打一遍，是个easy，就是等的时间有点久
>

![picture 5](../assets/images/5a676d3d4dc9d5183649a8adc28f4be95e848d14e3509d0766db0e8b2862e099.png)  
![picture 6](../assets/images/e94f6fdae5acf4aafc48473a99fe1c068df34206ddacadb17a0b348655407e29.png)  
![picture 7](../assets/images/bc68a4c7b1e044a98cc6a022589fbd447e912697d7f885d057b47cf28084e515.png)  
![picture 8](../assets/images/24d4a5c60f8c8ab7591d30fa57297106acef78de46aab52429e908884dd095e0.png)  

>它是一个纯私要爆破的东西，也是纯等没啥技术，所以正常可以跳过等待环节，直接看wp拿私钥位置就好了
>

![picture 9](../assets/images/d2d70630fa67936eeaa6b32b2070c17b5a72ad80216695a39c9649c3039d970c.png)  

>还没出来，感觉应该是bug了这个东西，上次都跑出来了，跑了一个点了，不跑了，直接下一步了
>

![picture 10](../assets/images/eb80c82f89e45ca0ec022591b252985638f2f8257565bec11c91e81c5f504f10.png)  

>看进程有个程序，不知道对不对,验证一下不对奥，应该是我这里的问题，我直接看记录了
>

![picture 11](../assets/images/6d21c9322353d32c0a22783f92d32cbefe4d3b246ebcc9a7c1d61aa3f004e50f.png)  

>按理来说前100行内就能登录了
>

>写个无聊的for吧验证我说的不然没啥意思
>

![picture 12](../assets/images/a8c6b23b2b58e6051d065e0cc61e5a8deec693cb9db962b9228ef08e8507e87e.png)  

>这是主要的ssh方式，我们现在有所有
>

>我先把所有的pub删了，接下来直接去前100行进行ssh私钥登录爆破
>

![picture 13](../assets/images/963602bda5afa3c80ee66b927307525a2dea0f8a117d9ae408277cbf7f69a87e.png)  
![picture 14](../assets/images/9306023523faad417ea00326d288b23e926ecabe7f0bf993ae52c454db788b5d.png)  

>直接ssh这样好像要密码不对的
>

![picture 15](../assets/images/e4b035d40d8b0293820fcf163d7f537ca6694de180eccdbf67ae3b1a45ec459e.png)  
![picture 16](../assets/images/e9a5434f7d2fb4e22c6f0b6c0ac2062b54ea93098813dd8bf7524a9977e7bfbb.png)  

>可以的一直按空格，比脚步好用，就是一个问题，不过优雅哈哈哈哈
>

## 提权

![picture 17](../assets/images/46310f25fb0091a4c15e285db69f99113370b5d09f4b9747459576c726cb8d0b.png)  
![picture 18](../assets/images/fffa419d2cefde4e4ca1008b1515d01b1c8315c8cce3c493057a6c3b264f3fe3.png)  
![picture 19](../assets/images/d43cab8ed61e0c7d029fab73543b20927c0ddfff5e7a0e59d9b868fb0796b97f.png)  

>没有过多的条件奥
>
![picture 20](../assets/images/a87faf28302b08b29884550ccfeaeba11e6f34a661275eff9f729c25c28b525d.png)  
![picture 21](../assets/images/e39738507b2c8a520a6bd5744c6c83da8ce877ee9f43f9c87c1c8144c8ea37b4.png)  

>没密码无法sudo -l
>

![picture 22](../assets/images/a4275a3cec0e9dcf131cefc5bbe2e278d669e46cd42709724b57a2cfea2331c8.png)  

>不想手查了工具找了
>

![picture 23](../assets/images/10e7feccc77404fb520a09576ab63624e731b317200076f51ad8805ba19ec2a4.png)  

>好了目测靶机结束了
>

![picture 24](../assets/images/e310b38786f3cbb0e24e0c16623dd2a2b12df6b2536c06806228c7e12ac3957f.png)  



>userflag:9c8e36b0cb30f09300592cb56bca0c3a
>
>rootflag:b0a3dec84d09f03615f768c8062cec4d
>









