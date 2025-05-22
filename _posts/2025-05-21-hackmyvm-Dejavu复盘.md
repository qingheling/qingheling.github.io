---
title: hackmyvm Dejavu靶机复盘
author: LingMj
data: 2025-05-21
categories: [hackmyvm]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.212	3e:21:9c:12:bd:a3	(Unknown: locally administered)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.033 seconds (125.92 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.212
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-21 14:45 EDT
Nmap scan report for dejavu.mshome.net (192.168.137.212)
Host is up (0.010s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 48:8f:5b:43:62:a1:5b:41:6d:7b:6e:55:27:bd:e1:67 (RSA)
|   256 10:17:d6:76:95:d0:9c:cc:ad:6f:20:7d:33:4a:27:4c (ECDSA)
|_  256 12:72:23:de:ef:28:28:9e:e0:12:ae:5f:37:2e:ee:25 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in -28774.22 seconds
```

## 获取webshell

![picture 0](../assets/images/423723e8e336bc09158812b3361a006f6e8bb840c24130c1d41936371dca0f69.png)  
![picture 1](../assets/images/3ac442b01ec344a76d059c72e8fbd5dbeaa9f1ed4170f0027b29af6f4c9a7e64.png)  
![picture 2](../assets/images/e9a4634a076935100507a000516c1790ed450ff8e34fe8bc913af9d22e56985e.png)  
![picture 3](../assets/images/fc386e3e4b463f29949bbaf3e4dcea238fb175ec0c882c0054f2a76fff76922b.png)  
![picture 4](../assets/images/d0e0acf655253577c0333c204fd3033f642feee75b2493d6ca53cd7bc54a3cf8.png)  
![picture 5](../assets/images/97dd04846551a371e518a0ea6c56f14f46dea419b60ad23605a99f5b565b79be.png)  

>大小不对
>

```
4,8d3
<   <!--
<     Modified from the Debian original for Ubuntu
<     Last updated: 2016-11-16
<     See: https://launchpad.net/bugs/1288690
<   -->
11c6
<     <title>Apache2 Ubuntu Default Page: It works</title>
---
>     <title>Apache2 Debian Default Page: It works</title>
196c191
<         <img src="/icons/ubuntu-logo.png" alt="Ubuntu Logo" class="floating_element"/>
---
>         <img src="/icons/openlogo-75.png" alt="Debian Logo" class="floating_element"/>
198c193
<           Apache2 Ubuntu Default Page
---
>           Apache2 Debian Default Page
229,231c224
<                 operation of the Apache2 server after installation on Ubuntu systems.
<                 It is based on the equivalent page on Debian, from which the Ubuntu Apache
<                 packaging is derived.
---
>                 operation of the Apache2 server after installation on Debian systems.
252c245
<                 Ubuntu's Apache2 default configuration is different from the
---
>                 Debian's Apache2 default configuration is different from the
254c247
<                 interaction with Ubuntu tools. The configuration system is
---
>                 interaction with Debian tools. The configuration system is
263c256
<                 The configuration layout for an Apache2 web server installation on Ubuntu systems is as follows:
---
>                 The configuration layout for an Apache2 web server installation on Debian systems is as follows:
334c327
<                 By default, Ubuntu does not allow access through the web browser to
---
>                 By default, Debian does not allow access through the web browser to
343c336
<                 The default Ubuntu document root is <tt>/var/www/html</tt>. You
---
>                 The default Debian document root is <tt>/var/www/html</tt>. You
355,357c348,350
<                 Please use the <tt>ubuntu-bug</tt> tool to report bugs in the
<                 Apache2 package with Ubuntu. However, check <a
<                 href="https://bugs.launchpad.net/ubuntu/+source/apache2"
---
>                 Please use the <tt>reportbug</tt> tool to report bugs in the
>                 Apache2 package with Debian. However, check <a
>                 href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=apache2;repeatmerged=0"
```

>那没事了，哈哈哈
>

>没扫描的东西我怀疑是php魔术块了
>

![picture 6](../assets/images/1d733364d3cfe226fb014bc3faf3631627ea7bf10a3fb181418d22f0a3153ae1.png)  

>算了等扫描了
>

![picture 7](../assets/images/ad224885f6115eaee5d15809081d2b59a079270b0fc493e3e19f410f6bbea129.png)  

>这个是什么东西没懂
>

![picture 8](../assets/images/a40672e632579994d87eac4de59eddce3133af3dcde6c076ec4cd752652cb665.png)  

>没有wfuzz目录也没有东西了，难道有是ssh
>

![picture 9](../assets/images/f1f57d5a152841535871b64507043f977baf908d928c1d9a06c6fd576d1e49f5.png)  
![picture 10](../assets/images/734d04d1f2d871feb5fe0778beb003b488e7987d93cb8b60ea69558795da0ce1.png)  
![picture 11](../assets/images/f72b0fc154db6d97601c280c9676ef4d0731e7e73bf0b4e93aa9ad6478b8bf85.png)  
![picture 12](../assets/images/adeb26ed43f2594c8821be49016520c1b72508c373f06098c386feffd78f9eec.png)  
![picture 13](../assets/images/2e7c5056f20b7602be1975b51d32cbc72367798e2bc844fe6b1a5e3a1559d57d.png)  
![picture 14](../assets/images/f0d1e85d09f7effe59fcfd7152622261a6f74f93b8fc8575d8e2d056a034487a.png)  
![picture 15](../assets/images/8a243c1fb370fd3711e113b812323b8a68a8032ca159cdff439aab2b3c17c17e.png)  
![picture 16](../assets/images/702eaf30f4706338493e8602ba7cfb1e0c9abcd768530fa36504790fd9ef589d.png)  

>都不行么，双写什么的了
>

![picture 17](../assets/images/c506fa5b6729795596a13ed2d09454bfea15d02d6662c9097456f44610fc49ff.png)  
![picture 18](../assets/images/c3253df9c7bf309203119438b68eec7c0abd373644210dfc14a30aaf78cac627.png)  
![picture 19](../assets/images/4bfa04fd2348b9b5d353dfcf815bbe2a56686fc2c6c9590c5323ca3e2d085b42.png)  

>差点忘了这个了
>

![picture 20](../assets/images/335d909d5c39ccc2f642bbb974f81e6389edf55e0248acc74dfe83b6e0668f03.png)  
![picture 21](../assets/images/3f737eccfc16667ce0d98a750bd5e2b96c4c52254143c173db5b63c36a40159f.png)  
![picture 22](../assets/images/f5d522bd40ed0869a950274327bb44982263040d5a5b9a8fa8f6573fa725e934.png)  
![picture 23](../assets/images/62bbe698424fd0439c9267e9efb3937282ca4a7693538af0dd436398d25abaa1.png)  

>没生成怎绕过
>

![picture 24](../assets/images/057d4e2f852759ec06744926d09ee24bc05240130c85a42d98c04433f49502ef.png)  

![picture 25](../assets/images/2c0d011b8b283b42070ded817d698e48d5605072051893dd598b04c3340227d7.png)  

>这个可以
>

>好了随便拿个shell
>

## 提权

![picture 26](../assets/images/e32d2f7d508845452ebb4a251ee30ba4ac1dbd58a5979b31d4d2f73fdb2a4566.png)  

>这样就简单了
>

![picture 27](../assets/images/bbce657b0d1013a9d7025f7c63ce372a145319acda66055c5541efa441f720bd.png)  
![picture 28](../assets/images/1bc0cb836a627cd2187ca3fad839a656781f5fa48a4a56feea9ae90e060e8d96.png)  
![picture 29](../assets/images/8ae97384790d142079e9ca9ddec5563b76c810a955d5b3e4b5dd1bcb803f351d.png)  
![picture 30](../assets/images/8840f03c1b0ceacf47263f849833aab5ec0d1ed38a6092ffdd19cc4379f90c8f.png)  

>等一下时间看看定时任务，没成功
>

![picture 31](../assets/images/af67ab86de3461a92ca8de15fd3358a391c244dfe7b71c07152df243b619db75.png)  
![picture 32](../assets/images/93175017e01e9019762b915fac5f404984e1fbe38acb19abbc12907698ebeb4c.png)  

![picture 33](../assets/images/5641d0da5772046cb691747bd69b13d103121c119ca0811dc3c7e219af4746cd.png)  
![picture 34](../assets/images/0e4a4f86c79371ac2d84cb9fdc2d1f85694b22b1144bad97352f818f8afef4e7.png)  
![picture 35](../assets/images/e9eb888332af307bb10bf8c516bfafe4ac1a4ba1c1ccbd8d42f96e58d3639097.png)  
![picture 36](../assets/images/0dc116e4fe53df903dd53ea028c49483446e7ab8f22e351e1b1f45af7b542507.png)  
![picture 37](../assets/images/7fea6502ea3ea462ca17a0a6b1d0a676e1a9a33c29106a09ce45ec69f72eca48.png)  

>完成然后，最近大佬复盘说有个新方案我复现玩玩
>

![picture 38](../assets/images/e21fc68777f977b09155fd5479fef06c26c8e2a45df57b9f62a7869536463d81.png) 
![picture 42](../assets/images/80d0ecaad0fdf60607367c6bc03c923bd927ec8f76a3159ef56d69bbb735e2aa.png)  

![picture 40](../assets/images/db7017c0419e792d4ee2006bb49ab915551dfc7c617180cffa2e35940acc26e7.png)  

>有报错的
>

![picture 39](../assets/images/4a13c076e5462ca64f10c5c5f751b773f30cc754217f35ee32729c8eb00534d3.png)  

![picture 41](../assets/images/b2fda9a897a31a53d4a4457d2c53431af232df04324cc5727b8009ecb439c5d6.png)  

![picture 43](../assets/images/f732c7e2b0f9ec0a79e7c8310218f5a6e1adbbf6756762168baee90a41648cf0.png)  
![picture 44](../assets/images/a224829ec1bcac5a62e6fbab52ef3f3233cec5622d7cbc1e9fc1c485eb4d85cc.png)  
![picture 45](../assets/images/47abf0b12e4d938450c418cea15df789dce36d770418152a16354300a05e9b9b.png)  

>我研究研究为啥没成功
>

![picture 46](../assets/images/9adc45c1eb5c78db7897d337be32c555a113cffe17fa024b368b665119a3ce0f.png)  

>还是有这个问题
>

![picture 48](../assets/images/b1f9e1c0936272ccb1233ec6982d9a249190332c295c8fd6cced318c2c462a84.png)  

![picture 47](../assets/images/c9a9ba033382b14aa3ed6f896025afc3783f348a43de8e53e0cee50a66ce5cdc.png)  

>好了，大佬的方案很有意思
>

>userflag:HMV{c8b75037150fbdc49f6c941b72db0d7c}
>
>rootflag:HMV{c62d75d636f66450980dca2c4a3457d8}
>

>这里感谢一下这台kali，劳苦功高，但是环境已经有很多地方坏掉我修不好了所以给他删了，给它记三等功
>