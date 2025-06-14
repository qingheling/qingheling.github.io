---
title: Self-VM EVA复盘
author: LingMj
data: 2025-06-07
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.139	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.071 seconds (123.61 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- -sC -sV 192.168.137.139
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-07 04:40 EDT
Nmap scan report for EVA.mshome.net (192.168.137.139)
Host is up (0.0065s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: \xE2\x9D\x80 \xE9\xBE\x8D \xC2\xB7 \xE8\xA6\xBA\xE9\x86\x92 \xE2\x9D\x80
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.33 seconds
```

## 获取webshell

![picture 0](../assets/images/74d78b733e374cdf85f422a1f081866e600fc7fefce0c0b3ed6bff14049403ed.png)  
![picture 1](../assets/images/7910e0d4f7213d048b1d9126c73f448c863ee90e39011834c94cd54a0cb97999.png)  
![picture 2](../assets/images/c36fd18b055a783ba39ec0f2fd49eb079ffefab984ecb00b1e94cb8964da202e.png)  
![picture 3](../assets/images/5ed5c01984cfc1afe459d825ad52e8c83a24102a7301bb852dce7968b82d0c53.png)  
![picture 4](../assets/images/5895253b85b86bb80d8ec39ddff346e7423792bae0d347842453bd97dfaaa944.png)  

>这里有一个特殊的提示
>

![picture 5](../assets/images/a3844843d4d39baa57950be3dfb259b82691974ee235b3fa8c135c37939ae19b.png)  
![picture 6](../assets/images/ac7ca2bdda81df3a6ee15f5a35f5b580b4127c93ecd00f68ffd7e844ed6e70cb.png)  
![picture 7](../assets/images/01a9599481cc29c974e8485dd98580213cd2ee54b612c8781f22ad03518e5a10.png)  
![picture 8](../assets/images/a584a059bd5ea7a14c8a8960d21306e7d3903f8241ee17d29b20552eaaabf241.png)  

>被禁用了不给登
>

![picture 9](../assets/images/a654bdb8fa817eaec98a13f91f8b2d26800dce47191618c8b584f929204397e7.png)  
![picture 10](../assets/images/222d18f4fb1709d7d20a2ecbe1d3c7cd5f9ad9d830d96d50c2fffaefd809ccda.png)  
![picture 11](../assets/images/63f6999290c73ba8ce5660c16953f0b7fec4fa453b2dec9f9f39ff7a80352a20.png)  

>爆破用户
>

![picture 12](../assets/images/0c84bd337730c0b3bdf7b87d064ac036e1c8ecd288ee5e5185d3da1b2503a860.png)  

>这个可以登录
>

## 提权

![picture 13](../assets/images/676c41fbe7e01cbba3eef1ef45595ed295f794e7a5c4a1e7ce65508ef30d28f4.png)  

>只有ping和pong的话大概率是二进制
>

![picture 14](../assets/images/7268f8e25ac4a7eebc302f079cac8c2b84462b284e92b6928590d07045bcc0f6.png)  
![picture 15](../assets/images/0210ac43ef2ed103c65263dec4550ad2e6f1e43c9722d8d2a716864ec1382505.png)  
![picture 16](../assets/images/7c16efaadde37a59bdfaf7ab9c9ea2fef938a5b1109e2ab3fd38056e5c4d433b.png)  

>他不给登录然后我记得有一个方式可以操作但是失败了
>

![picture 17](../assets/images/29ee3d297cd5d854d4b2936f88ce4226a63836e495a0dd6e012e86000bd56ce7.png)  
![picture 18](../assets/images/d6e1e73229270d5c5d972fbaee37440d196b03084ce8c965d6f13643e7025da0.png)  

>这里有一句话，猜谜用的看过龙族四的话应该懂，但是我看到三不懂，不过我猜是说的人名把我知道全输入不对
>

![picture 19](../assets/images/0b1d0e328690ce4a3795cffa6883c5b4765b9c593677b1a8b26f7f0ce86744bf.png)  
![picture 20](../assets/images/5c51216b09f3fbdff7f8f4c0b91d5c80b67db27659cd0741ba907119c4b56f54.png)  

>然后我要了提示是清除这三个规则对应书中表达
>

![picture 21](../assets/images/d8d252c7024336e7060773eddf339a4cef2e6e76fa14627e4bb43f42f34c9cc5.png)  

>这样就登录进去了还是挺有意思猜谜，到了root部分我卡住了很久我在思考为啥写md文件不被执行或者读取，当然解释是不完整的问题，所以我要了懦夫模式答案主要连用-r和-m
>

![picture 22](../assets/images/faccdcd5490d72e99dcc37cefa42a5c3176ab43ebfc1f06afa5d56c0f9241d5c.png)  
![picture 23](../assets/images/8316b5ae8fcd01f758279587f28da47d96f413c12274fce0677ef64bdb6e524d.png)  
![picture 24](../assets/images/6446c90c376ce467320fa40a36061a7a2e951faee6d2f8650bea33ef782d10a6.png)  
![picture 25](../assets/images/87f634d959091a338a2dd415aae6fccc113c709803e1a74380c5e94bc1157c8b.png)  

>好了结束了
>

>userflag:
>
>rootflag:
>