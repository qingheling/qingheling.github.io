---
title: hackmyvm Smol靶机复盘
author: LingMj
data: 2025-02-21
categories: [hackmyvm]
tags: [wordpress,cve,vi]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.76	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.253	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.92	62:2f:e8:e4:77:5d	(Unknown: locally administered)

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.027 seconds (126.30 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.76
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-01 06:14 EST
Nmap scan report for smol.mshome.net (192.168.137.76)
Host is up (0.041s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 44:5f:26:67:4b:4a:91:9b:59:7a:95:59:c8:4c:2e:04 (RSA)
|   256 0a:4b:b9:b1:77:d2:48:79:fc:2f:8a:3d:64:3a:ad:94 (ECDSA)
|_  256 d3:3b:97:ea:54:bc:41:4d:03:39:f6:8f:ad:b6:a0:fb (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://www.smol.hmv
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.85 seconds
```

## 获取webshell

![picture 0](../assets/images/8600a494fafeeeda3f2c47ead5dcadde796f7e4545ea119b8b324fc292ddb2e6.png)  

>存在域名我需要添加一下域名
>

![picture 1](../assets/images/6daf880a70a85336227ef6a61d890700835d46951429f6b6b46c1e9f778ac546.png)  

>不知道什么问题出现无法域名访问重新一下我的设备
>

![picture 2](../assets/images/7fad719fcdf0a1ec9f071b1559a65690905bacf3edf72bee1810c80f9af328ef.png)  

>好了他是一个wordpress的界面，直接扫描吧
>

![picture 3](../assets/images/f532bf8021aeb363c9822cdbb86f060d47d29ab69fe2250bf726c78a763655f2.png)  

![picture 4](../assets/images/af62d1afc1ead4ad88537e0e96a04e80a3be38ca67b574b18bcd67e2d46813ec.png)  

>一个插件漏洞还有很多用户名
>

![picture 5](../assets/images/f69123ec979be7b6105ef6b4d88f17e0049d725ccd158173c79f12d698c4112a.png)  

>主要吧我不觉得是用户密码爆破，先看看插件
>

![picture 6](../assets/images/986b27782a9405e2c5365af6d4867835185e467bf8fe97dd898f4a0cdd7f2ff5.png)  

![picture 7](../assets/images/e23af684706fcd2dafed5c2af32a0a27ad902b76f662f33b233f4e63a0402f2f.png)  

>这才是正常的漏洞的地方，这里如果找不到情况可以加一下api就可以了
>

![picture 8](../assets/images/61c41ade4cfba3c899d13852348f5bbf5f0e48172c05421cdbe23c4daa852cd4.png)  

>地址：https://github.com/sullo/advisory-archives/blob/master/wordpress-jsmol2wp-CVE-2018-20463-CVE-2018-20462.txt，自行获取进行测试
>

![picture 9](../assets/images/4a9a72a8e4c53a34c847c187e1c7b80e517a9a9623bf083a62512206c4a6ec56.png)  

>用账户密码登录一下
>

![picture 10](../assets/images/1d626c30d375e614189e2b904eb0f09a5f33068e293db0315913cb35a2a613ca.png)  

![picture 11](../assets/images/e9a59d04ba1d9c00a91cfd7607ba0ed1d6126d583c9c031458b55846be3cb62c.png)  

>有一个后门的文件但是我们不知道叫什么可以扫描一下
>

![picture 12](../assets/images/f5c38394c1c5a337c803424bd8784dd0e58636a5b91aac70f9c9bce7c9e750cc.png)  
![picture 13](../assets/images/82b6f5e8fd207585238ccfdab889865bd80b12cda38a4081b2cfd60b5b145cec.png)  
![picture 14](../assets/images/bd55ca34b20d0eaf9b3e46c4f4f588adc389e3d265b10f42a4ef302b030243fc.png)  
![picture 15](../assets/images/b97979f8a995aa294b3c97d52df1c3f6867f22f0d450511ee35189e083eb83a2.png)  


>注入点是cmd在用户主目录注入
>

![picture 16](../assets/images/393a875b5878fd7d41a09fec0df20fa6b5603df504e9f1a5b7d86c23e916f8e4.png)  
![picture 17](../assets/images/acad5d6fb3a5f8d32a82490c522ccf3864970f8d112b4df119dde85693543de0.png)  

>存在busybox
>
![picture 18](../assets/images/eb4aa5eb427879de608e4e318cae8892f9e851488fb17869b90b57f18702d797.png)  


## 提权

```
mysql> select * from wp_users;
+----+------------+------------------------------------+---------------+--------------------+---------------------+---------------------+---------------------+-------------+------------------------+
| ID | user_login | user_pass                          | user_nicename | user_email         | user_url            | user_registered     | user_activation_key | user_status | display_name           |
+----+------------+------------------------------------+---------------+--------------------+---------------------+---------------------+---------------------+-------------+------------------------+
|  1 | admin      | $P$B5Te3OJvzvJ7NjDDeHZcOKqsQACvOJ0 | admin         | admin@smol.thm     | http://www.smol.hmv | 2023-08-16 06:58:30 |                     |           0 | admin                  |
|  2 | wpuser     | $P$BfZjtJpXL9gBwzNjLMTnTvBVh2Z1/E. | wp            | wp@smol.thm        | http://smol.thm     | 2023-08-16 11:04:07 |                     |           0 | wordpress user         |
|  3 | think      | $P$B0jO/cdGOCZhlAJfPSqV2gVi2pb7Vd/ | think         | josemlwdf@smol.thm | http://smol.thm     | 2023-08-16 15:01:02 |                     |           0 | Jose Mario Llado Marti |
|  4 | gege       | $P$BsIY1w5krnhP3WvURMts0/M4FwiG0m1 | gege          | gege@smol.thm      | http://smol.thm     | 2023-08-17 20:18:50 |                     |           0 | gege                   |
|  5 | diego      | $P$BWFBcbXdzGrsjnbc54Dr3Erff4JPwv1 | diego         | diego@smol.thm     | http://smol.thm     | 2023-08-17 20:19:15 |                     |           0 | diego                  |
|  6 | xavi       | $P$BvcalhsCfVILp2SgttADny40mqJZCN/ | xavi          | xavi@smol.thm      | http://smol.thm     | 2023-08-17 20:20:01 |                     |           0 | xavi                   |
+----+------------+------------------------------------+---------------+--------------------+---------------------+---------------------+---------------------+-------------+------------------------+
6 rows in set (0.00 sec)
```

>一共四个用户我建议一个一个爆破，按照home顺序爆破
>

![picture 19](../assets/images/2552f3caf43b1b2745ef35131e5133c86c43e6e8940db7f069f0a2033cbf41ad.png)  

![picture 20](../assets/images/69d382d1a5498fc2446943361354dbbdcde64f72409415b6b9fdb0db52f565b0.png)  

>它的id是group组现在我们可以随意去任何用户目录看看
>

![picture 21](../assets/images/afaa3cbab938b3ef9dc8e32be248af30f73ebc3388e8dae9bc49b8a2a01d741d.png)  
![picture 22](../assets/images/96f0b711584e68240e78abf90426025dfd740326525fb4a929ed0f26b6f5db33.png)  

>可以修改用户的ssh直接登录
>

![picture 23](../assets/images/223eab9b5aef8f0f18bbf62c98b6071bcffcd286c1e90c26b39def2ee74a3dc4.png)  

>不能改就读吧，反正都一样ssh登录
>

![picture 24](../assets/images/27655470e15fd907b2f7bf8c8be7fea151d03ee8f2bac19f80f1439721f98ade.png)  

>发现又多了一个用户组可以看看是什么配对的
>

![picture 25](../assets/images/6d8e7ea7ff84a90c9eb844f4fa7d59b9e7a0cfe50b808aaa10b6f458d855d1a8.png)  

>看看直接切换
>

![picture 26](../assets/images/20e62d559b89e9bbfb62c50991e560b0f94ed02ff617a8fc48821494cec46579.png)  

![picture 27](../assets/images/9e4ff7cd088c2e438fa592b82c7facae3fdfff06f98bc494c8617432a137c25a.png)  

>有密码爆破一下
>

![picture 28](../assets/images/f045aee996b22d5f6cfe2ec76af74bf419fe7f1c902d7f844c21c370dfe0573d.png)  

![picture 29](../assets/images/ded659dea329b8487490064b1fbec6a8443cf29a8d5023034ff269fd1b1a99be.png)  

>vi 一个文件就各显神通吧，我这里既可以改root密码也可以直接设计shell
>

![picture 30](../assets/images/a6dd494b09e29b177f99c7deea50cbf2b84f9ed7ba699b09576d8da828aaa4a0.png)  

![picture 31](../assets/images/4048713f0a088cc46611fc1fe202a173dcb033332ce969403bda5303f5d9357e.png)  

>另外一个方案的话就是加一个用户是root组的好了就不展示了
>



>userflag:45edaec653ff9ee06236b7ce72b86963
>
>rootflag:bf89ea3ea01992353aef1f576214d4e4
>