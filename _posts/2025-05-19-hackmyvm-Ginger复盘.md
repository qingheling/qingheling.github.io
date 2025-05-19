---
title: hackmyvm Ginger靶机复盘
author: LingMj
data: 2025-05-19
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l       
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.142	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.114	62:2f:e8:e4:77:5d	(Unknown: locally administered)
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.142
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-19 02:58 EDT
Nmap scan report for ginger.mshome.net (192.168.137.142)
Host is up (0.069s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 0c:3f:13:54:6e:6e:e6:56:d2:91:eb:ad:95:36:c6:8d (RSA)
|   256 9b:e6:8e:14:39:7a:17:a3:80:88:cd:77:2e:c3:3b:1a (ECDSA)
|_  256 85:5a:05:2a:4b:c0:b2:36:ea:8a:e2:8a:b2:ef:bc:df (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.43 seconds
```

## 获取webshell

![picture 0](../assets/images/4c495329567af4929e2a4669de572bf91c07a45099ad2b367c3685cf4e485ad7.png)  
![picture 1](../assets/images/218434387c1e724e057c6d9f8450f778849e34be9f03dcd85e58eba83bd22313.png)  
![picture 2](../assets/images/1b2d37af76f7e86a7e052f6f5b24b253058013302244c4ffa8b5fc780677022f.png)  
![picture 3](../assets/images/30bb875fe47acf95c60003900f4b74f8a95bcb1012378dbfcf21e3366326b2a5.png)  
![picture 4](../assets/images/2df3e217d0d68962bd3c10c9490b975c5407ce1612b43d2b369412f37d1ce3d2.png)  
![picture 5](../assets/images/e84ece640112813d12051cc9c6cbc3d0912c5d0698d06fe426f883080e9ff137.png)  
![picture 6](../assets/images/4469d855b830b77219a23f449d5d5e78a23fc8f399a515d708f9f709c64502ee.png)  

>登录和插件其中一个
>

![picture 7](../assets/images/45b93ca6a2ba476e19da57250cf678bdba455b1c64944caf8be3f3d481279419.png)  

>看来是登录 webmaster / sanitarium
>

![picture 8](../assets/images/51ccba718fae3790aa0772293819db9958a297a0270ff239a0bb08c72551b728.png)  
![picture 9](../assets/images/edcbc1d0eb24a2f64782dd480fd1cdd7e1511cb67918bbedbe0c24488a900fe9.png)  
![picture 10](../assets/images/f6d966411bd6c35879603257c2dd796b7a4a66b22f54beb4e8fb3ad5cc46b7fb.png)  
![picture 11](../assets/images/9b5f5aee39ef1455bc86eb3418dd26604406326883d4a609f7d8842b9ed08f4e.png)  
![picture 12](../assets/images/f414265870bd9ed2f0bd7e0989a670d7f89ca8ac8d1b05dcb6a92f6e2449d31e.png)  
![picture 13](../assets/images/82dc8c5764dfeef0a311324a1147521947d93a6df39e4e78f85905a7e693affc.png)  

>好了成功弹shell
>


## 提权

![picture 14](../assets/images/4d65958aa90a9ff73e3f1721cbca62b1e4d080fa50e521b573ff60a50f97a4f3.png)  
![picture 15](../assets/images/e257217a3d907eb5b93bd1aedb1901f968fdfe8efa9a6311af47212560c148ec.png)  
![picture 16](../assets/images/3072625559830cb873dcff5e8663716c65cc7ab2d7f90fd1325602062b7f9d92.png)  
![picture 17](../assets/images/d5064e2e3a817f2d74b736375a1650664e2d10c6da34ddb7246fc411067fc8e1.png)  
![picture 18](../assets/images/0cb198d61684396dc6af8a0122a65eef5fe0927039b10a9bceb9a1df090e81e3.png)  

>看来不是这个方向
>

![picture 19](../assets/images/543bf5739a29144c983a669c8655dd6f632fbdc853e379d6e273588684a784c8.png)  

>看不懂
>

![picture 20](../assets/images/deb395f09c75c10d55b832e2feec15b0980b88ef892871541f283933576f6150.png)  
![picture 21](../assets/images/2653b0dbf63d257341a6ae3e8652dc91e9dfbbea09bce8e7b9cd4bc74c378faa.png)  
![picture 23](../assets/images/d91ac53dc44b893affa8ea26d71018e3ce8979576b417f078f6eaf3f4c22b190.png)  

![picture 22](../assets/images/0209868423feda0f745116ad0ca8686cd448907fa01125054665558bfd4af2c7.png)  
![picture 24](../assets/images/3f195cbf728d180e03ab3e2e4eb78aa49528232b9e38e777d83f9dc3204f4d1e.png)  
![picture 25](../assets/images/39c219c0bfbb22a619e1fc66c2844bed1cadd4b7c3912f7e0c8e196550309a9d.png)  
![picture 26](../assets/images/1640783e78b80baf7a6a245bb3e5eb5b4e49255442a9ac21e969ed21089fb38c.png)  

>不行开始迷茫了,看一下wp告诉我dmesg 这个东西
>

![picture 27](../assets/images/670394bbf2d8c86c9daaa7bb44f0486d71967ee34634fc0cef7e2d1ace345f67.png)  
![picture 28](../assets/images/228b29dc96400badea6fa7b818a018308b4d71e7c27945a40455ccc12684f335.png)  
![picture 29](../assets/images/66faa67c69ca8fb8878c22295529f071bbfbb9f796302c2d28579045d95ccf03.png)  
![picture 30](../assets/images/95d930f21aea952cbf462aa0cd4cc19c5b17136ec6bb83d4e11f4d4d29f4c074.png)  
![picture 31](../assets/images/19c1fbc53467690aab15664a6805e8b77456d53ac1997b77c313d74f0186f104.png)  

>给我个*能帮助我穿越么
>

![picture 32](../assets/images/f8adc62648f850523c11ea563f806451409360970a32425865d9019bb9d0623a.png)  
![picture 33](../assets/images/564f833832b7bfaf4a26ed28babf363e0b42b094e3f80fb41df339e83b60bd94.png)  
![picture 34](../assets/images/7c4daffcf8080d07516d8e4cabe399ce81eda784d88cc553cd3655f80d186cf9.png)  

>sabrina:dontforgetyourpasswordbitch
>

![picture 35](../assets/images/caeb4ba2ad47936952fe8ff1832f5253a7264e255ae011a5603ca34887a4f3d6.png)  
![picture 36](../assets/images/2cd0f7ce255d88fb02109d065d1900979249c91130645ca42978023494894345.png)  
![picture 37](../assets/images/e1de4cb79d9be2e388e5d13e2b1f8e0616934a2d7deb0537d6024e0a1e72501b.png)  

>王炸一下
>

![picture 38](../assets/images/ddcaf9b89bc20e0486d28e9098a1e57a2374c908b3e3a824ac1168c9047a0097.png)  

>调来调去太麻烦直接搞公钥得了
>

![picture 39](../assets/images/c164c1da003dac670245790a391fc61dbfc59cbef93648b578515dd4df57dd57.png)  
![picture 40](../assets/images/8811a9b804598ff4edf703e1ba9f017a294f9f95126c891bae512e94b7fe093d.png)  

>5秒啊那得看手速了
>

![picture 41](../assets/images/2d0301063b88755f9b3c6f79e515044041320d5f1a9b1918f1a0e2ddfddca63f.png)  

>做一下准备
>

![picture 42](../assets/images/cbaada506166debc716d2c6b71337c8279cb646dbdb5db90247760bb35a9a6e7.png)  
![picture 43](../assets/images/2bcccf27186c88b95196ccb69a21a0aa1c4f075b4beb2aea066ff7cfd32aba30.png)  

>结束，前面那个dmegs不知道其他都没啥难度
>

>userflag:f65aaadaeeb04adaccba45d7babf5f8c
>
>rootflag:ae426c9d237d676044e5cd8e8af9ef7f
>
