---
title: hackmyvm Hacked靶机复盘
author: LingMj
data: 2025-03-04
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.66	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.149	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.143 seconds (119.46 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.149
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-04 07:42 EST
Nmap scan report for hacked.mshome.net (192.168.137.149)
Host is up (0.0094s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 8d:75:44:05:5f:f8:4f:ac:a1:33:fa:84:03:db:6f:94 (RSA)
|   256 5a:b6:c6:9d:a9:15:42:74:4c:7a:f9:dd:67:ae:75:0e (ECDSA)
|_  256 05:97:3c:74:bd:cf:8d:80:87:05:26:64:7f:d9:3d:c3 (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.14.2
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.01 seconds
```

## 获取webshell
![picture 0](../assets/images/33678cc6851a5271d162a54ef2d5545bf2193c5314431c5ab616ce9df81f0fca.png)  
![picture 1](../assets/images/b38796bb52e41116637224558f09523f0b408a83a6c263e6e6263f18ac97b366.png)
![picture 3](../assets/images/126501597d9feb9b7a742d599d4e84563f50d74ec85d9579a7faab42f43817ca.png)  

![picture 2](../assets/images/750d59454ea26b5c56d46493c0f5263a6c6b7baefede52eec797b07a645eff25.png)  

>看起来这个像是一个用户名
>

![picture 4](../assets/images/e9735a76519d49fb9611ef358c9f1d0604b7cd2457425431490573e4145347f8.png)  
![picture 5](../assets/images/9eda346352446c7906a27f70d97ddbf27159501efe98810bc126702bffed0eaf.png)  
![picture 6](../assets/images/8b91d9f3b3432b233c2364d6ca4cb3d95fd371a9ba712f27f39e576470848559.png)  
![picture 7](../assets/images/22aaa570bea8bbcf80fca19c8554191b834f36c2461423e1617ef0a5bdac9e74.png)  

>看名字是执行rce的
>

![picture 8](../assets/images/90c14b7f20e9f25f4d0a4a9d85820818277722709ba48623449a337bc970801b.png)  
![picture 9](../assets/images/2702d0b5a69263b83166bbc6be50bd6e1e7936029c2f8758fe0d1bb6b8a265f2.png)  
![picture 10](../assets/images/49e15ee583edb567b3fd3bf769c42ece9c83d779c88a1612ab20bf7aec079bd1.png)  
![picture 11](../assets/images/e8376ea146e73f3135f3e236fdb0e85099edae42d0a64278b7c8e37b1a22b450.png)  

>好了shell还是很好拿的
>

## 提权
![picture 12](../assets/images/1c1731e7291076e213d51d1668595f23f5234b3e2f7ab616f0c3d416b1aa3d17.png)  

>环境劫持么？但好像劫持user会比劫持root简单看看其他地方不然不是劫持就说爆破了，看完opt和var没有有用线索
>

![picture 13](../assets/images/1fc3628f2c18c5c8ebe0db2593ca221e529d6164f92a524afe75aa8b8d7bb454.png)  
![picture 14](../assets/images/9f7ae0b061cc6b56cc1d865c69f55a9c288b4d425f4352042d57158959b1da2d.png)  
![picture 15](../assets/images/b8d2afcd2039f650626743261d1dab80b887c9782b6eceb1bb2e937bdc973743.png)  

>无有用信息，尝试环境劫持
>

![picture 16](../assets/images/3f7231428105a447f8406a7e85425c783e06b3ab53c5cd90dab8a882b4b338a2.png)  
![picture 17](../assets/images/e62d91ba2b1d959bfc11b91f16b4266bf301217d6775f8129dd4ef95cb075507.png)  


>没有用跟这个无关爆破用户名了，得到了些线索导致我觉得很难熬，所以搁置了，哈哈哈哈
>

>补一下后面找到了wp看还是内核我对这个研究一向不通
>地址：https://github.com/m0nad/Diamorphine
>


![picture 20](../assets/images/da590db2bd868274a8e46e30581206c14349e7238e66b6a2286374956397524a.png)  
![picture 21](../assets/images/e4d84ec75bcdb7ef312d85ccd4168c02b67725a7a2b31cdedd6f4a501af7f4b3.png)  

![picture 18](../assets/images/65e7ccb15642e9a15e2f00f237b547c55900a7d709c88d8899a58530695f2a02.png)  
![picture 19](../assets/images/43817753d400908c664219458133659f5150253bc75ea55227002bd374f123c6.png)  

>这样就结束了
>


>userflag:HMVimthabesthacker
>
>rootflag:HMVhackingthehacker
>
