---
title: hackmyvm Soul靶机复盘
author: LingMj
data: 2025-03-04
categories: [hackmyvm]
tags: [stegseek,nginx,domain]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.66	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.92	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.213	3e:21:9c:12:bd:a3	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.056 seconds (124.51 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.213
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-04 02:57 EST
Nmap scan report for soul.mshome.net (192.168.137.213)
Host is up (0.0081s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 8a:e9:c1:c2:a3:44:40:26:6f:22:37:c3:fe:a1:19:f2 (RSA)
|   256 4f:4a:d6:47:1a:87:7e:69:86:7f:5e:11:5c:4f:f1:48 (ECDSA)
|_  256 46:f4:2c:28:53:ef:4c:2b:70:f8:99:7e:39:64:ec:07 (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Site doesn't have a title (text/html).
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.65 seconds
```

## 获取webshell

![picture 0](../assets/images/142ae938319cbf1f242ac9b93ce5a18289dc11aeb32843bb9843cbbdabda785a.png)  

>一个图片猜隐写了
>

![picture 1](../assets/images/2ebd4e47314c2bb0516d23c3beab350584fe61acca79c162828bb353f6685db9.png)  
![picture 2](../assets/images/0f9d91a63744182bd8ef32bfad5a638df679446e26063b7defeaefb6c59feccd.png)  
![picture 3](../assets/images/65068c83575a83bab7e6d16e853443d62bd0fc9020d3b5c3ec4f06ad57a6cee3.png)  
![picture 4](../assets/images/051c897423b67a7f9a45995f9e744cfb9851cb138dad493771515631f3dc8bf1.png)  


>到这里信息没有了奇怪爆破用户名？
>

![picture 5](../assets/images/db07a8dd7e6e67bbf4781a663cf59d5b424316265687ddf3b974a4d4a7b06a9c.png)  

>可以爆破用户名
>

![picture 6](../assets/images/3f731763e8f7992880a6f52f35684b6e52ed5550cf768e2d8bf9b749fd1a5c9d.png)  

>不是怎么简单？
>

## 提权
![picture 7](../assets/images/7cfdf56befc1c65eec17f74b858f7116fdefdeffb4c477dab612a4459eba4033.png)  
![picture 8](../assets/images/8737e636e1d38f04c2acf3de1a066feb1b76d60a5feea147102554477c45907c.png)  

>没有cd，我想想咋目录穿越
>

![picture 9](../assets/images/b48d350324b3aa2ce700a35e21e0326298ee6036a0f2410c595abdbc75edc6fc.png)  
![picture 10](../assets/images/b1efb852cde7c224df8a02aa7e3cafd5be6f0b4970c71432418d04a3d20c4aa6.png)  

>奥我说呢原来是shell搞得鬼
>

![picture 11](../assets/images/8df525e3728f7d1256c19b7abde6a34e90a3371aa5c9456512ab79bad4e3f61b.png)  

>不能直接转换奥
>

![picture 12](../assets/images/7c38278d749493b036f6705e5ebc13e8cd4eabc72514c178214127770588b77e.png)  

>我看看web吧，没信息看真难只有ls 和 cat
>

![picture 13](../assets/images/42785775a0e0d078ecc34912c3fc3993857941b6d14b99b9ed955c43b26f5572.png)  
![picture 14](../assets/images/8ecf60fa9473c74330d5aac166fc6bde50e0e6d3b97b9963c111998bb3154126.png)  

>可以写就简单的查看一下能用php不
>

![picture 15](../assets/images/42891610d5f96a913d44bc2076e8e53102f89c0d423ec728818fcec522476e91.png)  
![picture 16](../assets/images/c99950c11e9954a0880812513216be9d19114878de944bc8f0f986bf565e05df.png)  

>不能直接>用wget吧
>

![picture 17](../assets/images/14c1f310e1ad7b1b22890f3aebc969dd0ab2963b17ad2c2f0a6606aa3a83d5b7.png)  
![picture 18](../assets/images/f7edffbc8453c64c950ed17e37a269d86c001c170fb0e57be1d3bd5b21367e17.png)  
![picture 19](../assets/images/9f05678abd1c528f223ef70e6e8e9274665a9d0760b24abc1fcf4ca9f4fb385a.png)  

>不解析
>

![picture 20](../assets/images/ed12064cac46b102b6c0bcbe42ab9c7527dc5689fde7cd59485f3314f484aca6.png)  

![picture 21](../assets/images/b0d619ce5cce9bbd17b619049d3685a9a01bd00bb11738bce1c9345f1383f71f.png)  


>有动静了不白费我试
>

![picture 22](../assets/images/6245efb08469ca461a28d6d9d3ac2674d09461050329434bfd2b27178bd48e8d.png)  

>但好像不算解析
>

![picture 23](../assets/images/b61d2ca1a714db0185dbd3d1d29af53ff84b1b30b8ab7bf089ba8989595cc80d.png)  

>不行不是这个方向的解我要使用工具了
>

![picture 24](../assets/images/17d980c3767f10043e3be689482c487152d690521a822d57ca724a64d457186a.png)  
![picture 25](../assets/images/f2c7a57dda4e2d77b1e312ee492ddf0756b3acf968c90d2d39a06aa708959055.png)  
![picture 26](../assets/images/1c57f02999045ee2f6a7f5716b2deeafebc0290489f7f6899d228192a54d3a5d.png)  

![picture 27](../assets/images/c9fa1cdbe7031a47cb14e0c1cebbba2c570a050a24f3a18c743502cf161e089a.png)  

>目前知道拿到peter用户就结束了，但是没找到其余点，难道是爆破?
>

![picture 28](../assets/images/c50c487b0fa3cc2a59d9f6c9aef5b3472d21abc473bfbf8931b4f75c38e0ed38.png)  

>目前我没方向了，跑完密码没有看看跟-x有关不，想想还是不对不应该给/var/www/html/ 777不是这个方向的东西
>

![picture 30](../assets/images/989e9e33bf6f51a6d34309a7179b224c07b036cca1926c5ec0951984f00adb39.png)  
![picture 31](../assets/images/b7fa49de6abd0b995716f88de613b23722544814136771c742bcb3c8a3a7491f.png)  
![picture 32](../assets/images/3841ea194b760604c5b059cffd18cc2c6e84d0242ae16cd87fcdb79f2139ae69.png)  


![picture 29](../assets/images/b893c3baa49dfedd74146e923507d94ff80b2ab3254e74e5c3c2478479003f2e.png)  


>80服务要域名
>

![picture 33](../assets/images/d48b035e431a96c7c2512b0b36626a38b2b8a0e6f470970918e3d57d7552c0ce.png)  

>真果然是这个位置的提权真麻烦找起来
>

![picture 34](../assets/images/d1e1ac5b266273052b84eafaf7b840bce261d9a6a8550e84dc0153c12c3344e4.png)  
![picture 35](../assets/images/781cfacf595fc6cbe09d4286929042405fdc18161ed052b47ec90dd8a3e7528a.png)  
![picture 36](../assets/images/704bff081c91da06a676885a1885558c13fa0c55d6a8b06302bb78e3ecac5790.png)  
![picture 37](../assets/images/e5dfb8715326e3c3fc062e527ae1d739ec9a7cacb8b10577ee119641d63f3120.png)  
![picture 38](../assets/images/00012769300ea62dd6df5051bee99b20fec15be944434f4abf453ccb619578c4.png)  

![picture 39](../assets/images/71442a7bcad18ad84dbeab4e3d73806ba389be58a78b5f05a85a5f696205b117.png)  
![picture 40](../assets/images/3bdd18df2fbe5f8daec45d85677d2d9f3f8206882e2289909f452e8a881fa0bb.png)  

>这是hard？，直接suid拿root了
>

![picture 41](../assets/images/220303ef0c48b89f47ae1bdb6be713278643851adb8ab7823ed1c1016f2adbec.png)  

![picture 42](../assets/images/3c6c60fa63e42bc36856b6a0c7733c6c21e1ccbbdc60a16979a04d4acee48749.png)  

>结束了整体如果找nginx算难度的话应该为Medium
>


>userflag:HMViwazhere
>
>rootflag:HMVohmygod
>