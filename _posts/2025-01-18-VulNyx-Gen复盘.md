---
title: VulNyx Gen靶机复盘
author: LingMj
data: 2025-01-18
categories: [VulNyx]
tags: [puttygen,tunnel,public-openssh]
description: 难度-Hard
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.172  00:0c:29:a7:dd:d0       (Unknown)
192.168.26.254  00:50:56:fa:a6:d1       (Unknown)
192.168.26.1    00:50:56:c0:00:08       (Unknown) (DUP: 2)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.925 seconds (132.99 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.172       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-18 06:17 EST
Nmap scan report for 192.168.26.172 (192.168.26.172)
Host is up (0.0025s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
MAC Address: 00:0C:29:A7:DD:D0 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.01 seconds
└─# nmap -sU --top-ports 20 192.168.26.172
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-18 06:18 EST
Nmap scan report for 192.168.26.172 (192.168.26.172)
Host is up (0.0017s latency).

PORT      STATE         SERVICE
53/udp    closed        domain
67/udp    closed        dhcps
68/udp    open|filtered dhcpc
69/udp    closed        tftp
123/udp   closed        ntp
135/udp   closed        msrpc
137/udp   closed        netbios-ns
138/udp   closed        netbios-dgm
139/udp   closed        netbios-ssn
161/udp   closed        snmp
162/udp   closed        snmptrap
445/udp   closed        microsoft-ds
500/udp   closed        isakmp
514/udp   closed        syslog
520/udp   closed        route
631/udp   closed        ipp
1434/udp  closed        ms-sql-m
1900/udp  closed        upnp
4500/udp  closed        nat-t-ike
49152/udp closed        unknown
MAC Address: 00:0C:29:A7:DD:D0 (VMware)

Nmap done: 1 IP address (1 host up) scanned in 19.24 seconds
```

## 获取webshell

>这里看到扫描的结果有点懵，猜测在ipv6
>
![图 0](../assets/images/d97cca75c346917f711ab77f81cf692e2d87420cf2b84b136ae402c7c04e77ae.png)  

>猜测正确但是需要思考网站访问web 80服务应该如何形式访问
>
![图 1](../assets/images/0dfe6dbe7b086cff7f79d11a1674716242c86b9a3e2e5e9b1db768f1c9ddaf13.png)  

>windows 访问不到需要找到Windows对应的ipv6地址
>
![图 2](../assets/images/47653f5c503ab20b0af177fe3bc6be3ae7c925a48eb5d41c6246a93cb76a7b7a.png)  

>陷入沉默
>
![图 3](../assets/images/81e4455196e8147d3db1816ae7a20d449e7d962a26e1be8e9dd95962b3525ae8.png)  
![图 4](../assets/images/497321ba9668f4d1bec90a957efef9f5f176b29f74e49fec5a6c4b4d11786f73.png)  


>但是我Windows不知道ipv6地址是啥，无法进行80端口操作，这里启动b方案去kali整了
>
![图 5](../assets/images/654aa7c3254fdffef13e4a87fd1a1f85d8f574180307a870124abefa45e427ea.png)  

>算了又卡，又访问不来，老老实实用curl
>
![图 6](../assets/images/0dd7b246f8cc12d9283777cfb75940e975be11299c1b7018c6731a0ed6aa1b73.png)  
>又卡住了
>
![图 7](../assets/images/dece8817b4109b8a8c7dc8eaaaf88b1b7022ead2a65ae1615578961c0c6a9e49.png)  
![图 8](../assets/images/a0b2fad6f7755bdd6f6e7efd2dacc540389105f3878ca7c97a35eaa61d4bbde9.png)  
>无想法，也没找到有用的点，查查ssh版本漏洞吧
>
![图 9](../assets/images/07386cb4ecf3309976e8ff6fd4f34296fcc7520155f32217c8590ebfd8df87b2.png)  
>找了一伙版本还是没有，尝试爆破user
>
![图 10](../assets/images/b8f8f8e7a32a1a3f4477cb140d237cdbdf577aa4c087a5260ed2b481775c45eb.png)  
![图 11](../assets/images/2caea1df31bdd9dcd65bdcd895fc0705827c6650a8ba50cff8bc9146cb95586f.png)  

>出现用户名看看怎么操作，没密码继续爆破
>
![图 12](../assets/images/c8a13d0afef277714de9d1ea4037c1948739531a0cb6e29c79b39c292fffb60d.png)  
![图 13](../assets/images/3843aec8716eda828985be3aab5ed7c5d5672e96754fad09ab25d199c452347c.png)  

>大概500这样子，等的时候无聊，hydra最近用起来老慢了，如果可以给他提速的方法留言给我
>
![图 14](../assets/images/c3190bfd4449426fcb05584659b6d67320ae4be63727c88c9c0b5ed4ae90625d.png)  

>新任务，但是貌似没看懂
>
![图 15](../assets/images/957b26a05f4a3678212b555c56cc9580ba357809fba582fd0cd36d1c04b9d8ad.png)  

>翻译一头雾水，
>
![图 16](../assets/images/655e9c994aa5702ad0707ff3b06ed3d9e503b04726e8fb8461b552de871ac6e6.png)  

>没用尝试缩小界面看看能否进去
>
![图 17](../assets/images/d47f484b28043eda001f81e51295e8b5e8053c87acd6c35c3b16e4b45792063f.png)  
>还是不行,查一下资料吧
>
![图 19](../assets/images/02318c2a2107cb0fe1cc0af3e6cfe5c1fb225b4e4066e1dbf7bb7ce112261ebc.png)  
![图 18](../assets/images/407f48736f64cb485bfa333a9a53377a880fc851ee3849e14dfdff7cb8d43cf1.png)  

>沉默了，这里打算看一下wp提供一下思路，去查看站长的wp
>
![图 20](../assets/images/e2659e9eb041212dec4144ed27e5d04b9f6c7c37e1a13904accb103b23e5a2ce.png)  
>可是我访问不了80端口，很麻烦，这里酌情考虑视频复盘了
>
![图 21](../assets/images/e99ae1b6d65bf5610cf2c4bd5a476880d18e949df8860275a420758f614cf155.png)  
![图 22](../assets/images/866459fbe96aea311664e3e7379debdbc15b3d90bd2994d8b088184317f69340.png)  

>太强了，我直接复盘不了，先搁置了。
>
![图 24](../assets/images/f289192e16efe68d411fea158a1a7c317d466d0efcd666a44aebaf90c50cdbf0.png)  

![图 23](../assets/images/37a34b61538906504d7a2380802a8cc3b3ced12fcd0d39ebfa9f8915c2cabfe0.png)  

>重启出现80端口
>
![图 25](../assets/images/f6812e2c631c3ea5ec6c11e2fd77ba1937b6d726097d18a9eddcc6386eda21d2.png)  
![图 26](../assets/images/de6d15a44af50843ac04a158f40bd9a868a54b254e69634bb0d60ab2e1dfcdd7.png)  
![图 27](../assets/images/2fd73ac374d76e42882a9d0e47056bbde16ef13a81587d4469fbbaf0e87fffe1.png)  
![图 28](../assets/images/b633bd5a0a397a85602215d45dc417654776b863b12a7e93fd87ba7c1bcfeeb4.png)  
![图 29](../assets/images/e456c43b5c59205582e5f10393d7c1c5845a822a45588cd2280a2e0205645430.png)  

>把他想错了，说明一下这个是什么意思，之前理解的是他直接利用这个9999进入内部登录，但是理解之后发现他并不是怎么设计，他的设计原理是这个mark登录进去，可以利用这个mark把里面的某一个端口通过隧道的形式转发到我们指定的ip服务，这里我直接转发到我的localhost:9999服务端口上，用ss -lnput 可以查看到我们这边的9999端口开放了
>

![图 30](../assets/images/c72cb08617a2910cdf632d161e65faf4ed0338d9d2f69af318f29df193fd83a3.png)  

![图 31](../assets/images/981a003e263651fee1a2655267a8c34708bf17aac6418a4d175e4bfef88dc4b1.png)  
>这里已经访问出来的9999服务，进行一下目录扫描
>
![图 32](../assets/images/c8988d5657ffb9d727f8495be317127c137ab297a25ff7f6c77af73cfb5a1e43.png)
![图 34](../assets/images/577951fd63734d4a92c126e497f52bd0352af6b1ee5d6820a65b91fb44e80640.png)  

![图 35](../assets/images/918d999c3293ff92bf1de3dca5facfb915a64cce1ac3b2c6159de95fcc966850.png)  
 
![图 36](../assets/images/c4ab87c74a7d764f3eb74d54ce3167b8ba5a84a9de26d096d31a48dd52b29726.png)  

>这个格式有点麻烦
>
![图 37](../assets/images/f1a59e64becb25995f58aa3c7f3f913ea14d0a46007b258eb80cef000bb5b881.png)  
![图 38](../assets/images/f2e38e960f2139d961eb6ed39831cd7ac420bb4be86af30cb9b4e9aff23d7ae1.png)  
![图 39](../assets/images/36856e1f2ca1b5548a5919f77eb139bb14506c92248ab2a7a27ddb5a603fa277.png)  
![图 40](../assets/images/dfa36cc2620807dbb3381d679c2ea32c77525ac2071e7c7154c9c1431ae1f1b1.png)  
![图 41](../assets/images/65b0e3bf27a5f9b7598cc01e8367ae7a344badebc32f2877ddf76b86176f881d.png)  
![图 42](../assets/images/f96d1d9b0e2b327718595c16a10909a66895fb72f65f22276e9bfea81af204c8.png)  

## 提权

![图 43](../assets/images/4ea61c7c1a6569705f2f5fc22b13112b7d27d00b11c149362cc6dfd3124b00a8.png)  
![图 44](../assets/images/e9c6993a3b82a764e2726259bf5e11e3b79c90726b8ea1b9a495e1ec0a914f33.png)  
![图 45](../assets/images/d443121a59533227fee8ebeaba54137db9f79d4683392c56b1750fa492b0147d.png)  
![图 46](../assets/images/1f7538b487afbbc97dc4cdde5eefcca4cea9411354a194c9025d8a88a4a93357.png)  
![图 47](../assets/images/c7e899568ff41f5ed9ab9f69c76513d95162755e9fa2aae2afebc605f8fc03df.png)  
>apache并没有解析php，很好奇
>
![图 48](../assets/images/6146a165a0c0f56f917348313476128ad096d9946e48aa9cbddc6e3737201aa8.png)  
>尝试在这里出现结果
>
![图 49](../assets/images/d8d18d9edfe88005330b169f9c1dec04feb90889073a9d11e905b4f8d6448ad9.png)  

>我就知道尝试怎么多不对就知道肯定不是怎么干的
>
![图 50](../assets/images/eaa03f2e2dcd05708e029d6455ceb6a08fc23a5ecc48d98c74a2fb92a6febd4a.png)  

>我想到一个想法就是把peter的私钥传递到/root/.ssh/
>
![图 51](../assets/images/6b9580312cfe7a58e63994d44490eef994bb17fad21856de6c11f9e532f7d697.png)  
![图 52](../assets/images/ea26e800718cc633f5d33609069d624852f6d0edac01259ac6eec5d033b0dba2.png)  
>没成功,继续憋
>
![图 53](../assets/images/b942cd1ce0ab3e1e2a4d00b0a8b8152afc7f4992bc19ba79e86178fa1e056b1f.png)  
![图 54](../assets/images/eb6363bbaed97cb678c88a1164f13e27707f43154d9a94cedb5a921fbab3b937.png)  
![图 55](../assets/images/c783dc5657180b867d3dffb25c9a8de569189febe09088cbc3c74d030a7e3c3a.png)  
![图 56](../assets/images/bc956cf8cdbf57c4f38a9b927179b6782be85cd2f2abe4b616371e0204bb97b5.png)  

>好像知道了，靶机应该被我搞坏了，重新开启一下这个靶机。
>
![图 57](../assets/images/e7ddab947321b20fe1469ea331829778651329aa3b187c0b09731a83e4e55960.png)  

![图 58](../assets/images/6194fc6172d9d4bcc6dfd6bb8e5e45fb8e122e37af180e1014e6ed653b92a014.png)  

![图 60](../assets/images/50a8e9e4d89e7c70f2ac4508fd7fa35ebc741edd3526d566afe6c6a702b8c3d2.png)  
![图 61](../assets/images/da4aaddc313e70679bd4cc7d814716835e5eb244153f9c3dc33610e8b3162251.png)  
![图 62](../assets/images/a07608ea55b989022cb52674367f51f8c06a50464c7f12055e311b04574bba89.png)  
![图 63](../assets/images/d1b80d86f083839e24aab63208c5388c3754accffad719bcb8c043104b6f386f.png)  

>明白了，真服了
> 

![图 64](../assets/images/31bbd40c351644d40c397d2eaae40e641f7dca7611084d514d4948b41dd1a6a2.png)  

>到这里靶场完成了，这个提权卡了一个点
>
>userflag:d045787a2743570b0dc1aea01fc952ce
>
>rootflag:f003a3bc3ff27072d6ac2c7a1ab63254
>