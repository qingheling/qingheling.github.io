---
title: VulNyx JarJar靶机复盘
author: LingMj
data: 2025-03-02
categories: [VulNyx]
tags: [ab]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.66	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.181	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.92	62:2f:e8:e4:77:5d	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.049 seconds (124.94 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.181
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-01 19:40 EST
Nmap scan report for jarjar.mshome.net (192.168.137.181)
Host is up (0.023s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 65:bb:ae:ef:71:d4:b5:c5:8f:e7:ee:dc:0b:27:46:c2 (ECDSA)
|_  256 ea:c8:da:c8:92:71:d8:8e:08:47:c0:66:e0:57:46:49 (ED25519)
80/tcp open  http    Apache httpd 2.4.61 ((Debian))
|_http-server-header: Apache/2.4.61 (Debian)
|_http-title: The Menace of Jar Jar
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.57 seconds
```

## 获取webshell

![picture 0](../assets/images/83d4c9abd74ab9fb82eadc73ef92808c29f96ac0e57aac0a8cb00c45537d816c.png)  
![picture 1](../assets/images/dfbfdef106cb184afc5a7475a38d47567f99df38f297982d8c9dd3c66280dad3.png)  
![picture 2](../assets/images/245fb5ea0e75cf33ba77d947520b303d827a0b6f005bbced5d7570ed731b9f99.png)  

>找找信息
>

![picture 3](../assets/images/ba5af63d1601683e18de7887e28a208f89a91540bbc4d45bbe307bf07c2b2ee1.png)  
![picture 4](../assets/images/5afa9b7e0c0da1f521c1c6d7de94a66951895e7aa779661b00f26a66cfb3abe4.png)  
![picture 5](../assets/images/ed437d8a10e45cbed66021337a621efcc56df1e8cc3311975b8d6e5093a0dae0.png)  

>说一下正常如果真是跳转admin.php不应该说有大小的所以我们可以试着伪造一下
>

![picture 6](../assets/images/834ee35c96f9431624d4dbadbecf201d6e38a31d9b2d864d3ebc92c1192d7ab6.png)  
![picture 7](../assets/images/92c4befe6f90d92feb23a2b9c33ec548fffee6386b787dc8cb704bcdc400ee4a.png)  
![picture 8](../assets/images/4c6b1ef8ee61053c699656714e17a34ca2be70703dfe1498651d7778934dc91a.png)  

>ok看来是LFI，不过我要找到成功条件
>

![picture 9](../assets/images/3316f0f62a73fcf6374769d29028b54c78a784ac38265f31a94915bd271d02b2.png)  
![picture 10](../assets/images/b8d0e08668f9e75056034bd13b27c35b7a324ffdd6b469dc1a42a01a7a49dfa6.png)  
![picture 11](../assets/images/5ce869cbaa27424c3dca63629864469723ee70af3570a35f6f737dbd791c68a1.png)  

>它得先找的当前的目录才能目录穿越
>

![picture 12](../assets/images/caf3c271bf9c39ad85e959f6ad7f9c18749f66df8c0746089e7baadfd32da084.png)  
![picture 13](../assets/images/8fcba2d5dc4e98e473f3cc91a699339acb5e56ca45f38e69353338edea51991d.png)  
![picture 14](../assets/images/76685ac0e8496152ee69dae7995b5be124941bbddb8ff2a7786e5dd9eafd30ed.png)  

>ok可以直接登录
>

## 提权
![picture 15](../assets/images/2438696d17ce1f5fec0000a46bac5f5348ae17b6e4ed121763c39f7f2e54a8ba.png)  
![picture 16](../assets/images/ec9abf443b4a1515f5a1391561d2ebbd0851b0da2c69121622ee2e5eb263c4d5.png)  
![picture 17](../assets/images/3cf74deaa8adcf012cccb008b11751c6aa3e65210251b0bef1d185d65325d7f6.png)  
![picture 18](../assets/images/419c392e6da8e6e49adfa49b232dd9b6bcc94efe56dbba0dc91f3d03acf637bf.png)  
![picture 19](../assets/images/8828cec936d113e1bb3622aba1e13b6cf06be75f3bf630363aa9c89da4400b2b.png)  
![picture 20](../assets/images/9524a3fb46a2109f2b665f1fd74b47ed8b1133f88f90f87fca71007ce3652428.png)  

>密码都不对，那看来得用tools找点线索了
>

![picture 21](../assets/images/7a15322e27a27934d993a7a0b2465cfec6188669ecc253775691d1455578c80c.png)  
![picture 22](../assets/images/21f8a651424ce467cc8eb9ea038122622e6d8e1ec2f54a9939c5e192b3b7631a.png)  
![picture 23](../assets/images/dc1301e373aab3c957708f53d3a4a2d39011fd070fd69333f7875e1ddda10f4b.png)  
![picture 24](../assets/images/4031624668f5d72cee4b593826529528e386bbff7fde06a07cf4832c5842fb30.png)  
![picture 25](../assets/images/c5b44a98ce5aaec564a86f504562b00214028d1b634bef4cfac25f8f338c8c36.png)  
![picture 26](../assets/images/9857cdafb90e0c5a21ddc0bb4d723583eb5c481855f5e5d348efa5d1e53d556e.png)  

>它是一个传文件的形式不应该是直接开放80端口应该nc接受跟/dev/tcp差不多
>
![picture 27](../assets/images/330f9b6e40e137b8f4ffe73659b224359dd66eced9258967aabde02bf93a4684.png)  
![picture 28](../assets/images/6a79281d776bf91178ccea14c3f3ad2dc1b605f1b0477ba1a9e030c1fb8f1019.png)  

>好了结束了
>

>userflag:cd3afd21332e0ea7c8a47ff6a26387e1
>
>rootflag:322f07d15f30d1c4e3009dc5f2decb0f
>