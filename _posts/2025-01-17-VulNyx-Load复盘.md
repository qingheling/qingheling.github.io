---
title: VulNyx Load靶机复盘
author: LingMj
data: 2025-01-17
categories: [VulNyx]
tags: [Upload,Xauth]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.166  00:0c:29:36:5f:76       (Unknown)
192.168.26.254  00:50:56:e2:a3:32       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.889 seconds (135.52 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.166       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-17 03:29 EST
Nmap scan report for 192.168.26.166 (192.168.26.166)
Host is up (0.0032s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/ritedev/
|_http-server-header: Apache/2.4.57 (Debian)
|_http-title: Apache2 Debian Default Page: It works
MAC Address: 00:0C:29:36:5F:76 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 62.11 seconds
```

## 获取Webshell

![图 0](../assets/images/d84de3b442d2dffc7aaa371053df664bd51700b20f9caf722e6483182b678923.png)  

>这里有一个robots.txt文件，去web上找信息
>
![图 2](../assets/images/d8ccdaee32acdd7adbc0f0424ccf6b101debf3955cae1d0f4713d79426e724e3.png)  
![图 3](../assets/images/ab3239c3ab2aebed84edfd5945656d3aeb9cff0eec049bd2772be7f3cc74c674.png)  
![图 4](../assets/images/57c4481d3f6b340f003e0d73e974affd372a5429fec53a390f7afb596a6abc83.png)  
![图 5](../assets/images/87241c599d915c562dfe7a02354bb2e0a93dcc0f7b2a6315b3d0c4c10c7dde02.png)  

>这里很多的目录，但是访问都是null，只有admin.php是有用的他是登录账号密码的，尝试密码爆破，sql注入等操作
>

![图 6](../assets/images/8cd347a12fd180962363f30f1281cad602b52404a6ff587654e88d2a2b782520.png)
![图 7](../assets/images/e3da686d8432a3130f2f4095dc569cdc0513f315832a8122821354380b17ee6f.png)  

>验证了一下不存在sqlmap注入，爆破无果，我猜测万能密码登录或者默认账号密码还有继续大字典爆破
>

>大概我sql不太行，没成功，继续尝试默认账号密码
>
![图 8](../assets/images/13a75301ded5abaa2fa96191fc4027eaa093761d028dfbbff5060fb30b341002.png)  

>默认账号密码登录成功，username：admin，passwd：admin
>
![图 9](../assets/images/5a4d6be18110a994d32e439acb76e654a95629a1956a47f5f1618e79e4753ddc.png)  

>貌似可以进行文件上传操作
>
![图 10](../assets/images/8980dcf88d2c357a912a99425b9946161fcdd1e85b7f73672b11408f57be3378.png)  

>尝试上传无果，需要改一下东西
>
![图 11](../assets/images/9bb158da171f5ed1c11d20a8e60d04a5bb041cb12a075633740b0fc06dc0eec6.png)  
>这里有存在漏洞配置的，不过是3.1.0版本这个是3.0.0版本不知道能不能使用
>
![图 13](../assets/images/357a849e13e920d8aa08fb163b8d34f0747edae7a1ee9a3507280ed4e432a5d0.png)  
![图 12](../assets/images/600d9b5167f398ab0bd5b64c541a9b3372ef1090740fd181fa3384812cc01d2d.png)  

>有点蒙圈，需要理清思路。
>

![图 14](../assets/images/350fef88ac02be9b3e6ed75970391d9d555b819e9a59208a33af7d56c6eca7ad.png)
![图 15](../assets/images/4a2c18f32eb9ddf49ebcdb967d4cebfe54ef5a886e7ba728cd72230ae9f0deb0.png)  

>上传的使用给他加filename就行了
>
![图 16](../assets/images/6489f3980024ee4fe93c8401747da67af74b7829a782095d2df0785176b72499.png)  
![图 17](../assets/images/dc27bef9c09c37cc481a46ed411733b06f3c5b12921dbd9e9907997104fba527.png)  

>发现busybox拿webshell是好用
>

## 提权

![图 18](../assets/images/023f540cace1632ec8f1bcd5d18661f0db7e4b1a6bffb8e3c4113d4624ee89ca.png)  
![图 19](../assets/images/85c539c5f475d5ef9f63a6d970a714de238146054a02bfc04a88697468b4ff38.png)  
![图 20](../assets/images/33500e3962cd44d0bcc9a91636f8b230d0931184caa47f2bbbe11c9499ff3473.png)  
![图 21](../assets/images/8e06fa95981ddad102e13ceef9a4e75a8c7dacac2970b850204467504147a23e.png)  

>没有直接利用的ctfobins需要查看方法了
>

![图 22](../assets/images/a8bf11d0984eebf5ba7f3597211bfeaa71afa2c9f53e65209b3f99e6fd8c2df5.png)  

>存在文件读取，就好办了，看看是否有私钥可以获取
>
![图 23](../assets/images/9932d944a898db5e4a45f0ba56fed30069cac1135f472361f0f318aea175206f.png)  
![图 24](../assets/images/23180868be00fcfb24d070430bfabc40f0e79d490229acbcbf60cad540243078.png)  
![图 25](../assets/images/54ca2abb5a8e35d92f1fdfe749b78a20f10b154a335550ceb28d180d8a1b2d30.png)  

>不过可以发现确实一部分，可以去其他地方查找
>
![图 26](../assets/images/0922136f9d2d20a230f2f5e18108759da6d8e7602e71bc0400769846326aec96.png)
![图 28](../assets/images/0f0a7d75484343ed0f9b14a2f78f49d387742d7d5208b9bbf40b6b45b6814ea8.png)  

![图 27](../assets/images/434d1a9a3af834d74f181aabf2ca38de7444e97447e318d0d4fbfec96b6094bd.png)  

>到这里靶场复盘结束
>
>userflag：c08d9e59eb1252c60bf2ec2fd73c87f1
>
>rootflag：85ed9306438d8302cbb4dcbc7c5491b3