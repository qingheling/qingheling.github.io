---
title: Self-VM Exchange复盘
author: LingMj
data: 2025-07-12
categories: [Self-VM]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.50	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.118	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.053 seconds (124.70 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- -sC -sV 192.168.137.118
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-12 06:13 EDT
Nmap scan report for moban.mshome.net (192.168.137.118)
Host is up (0.0089s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    nginx 1.18.0
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: nginx/1.18.0
| http-title: MazeSec\xE9\x9D\xB6\xE6\x9C\xBA\xE6\xB5\x8B\xE8\xAF\x95
|_Requested resource was /index/login/login/token/eb97293ab07da8385711111076fe91b9.html
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.44 seconds
```

## 获取webshell

![picture 0](../assets/images/2b665303819e742140c47ad09aa0627ee39f7aee511d1f36b87d6c0cc3790a62.png)  

>存在注册，注册什么都行，不过好像这不是重点
>

![picture 1](../assets/images/2dfcc3f4bf3b2f9a00388e8a4af86abf3acc203f9b848f7ce36b1429b0c4aa98.png)  

>作者给的小游戏提示
>

![picture 2](../assets/images/7d412bfb86c84d8298066762f179c3263f7ec27d666f2c28952ba140a2e28959.png)  

>这里可以看到前面的路线是thinkphp
>

![picture 3](../assets/images/245bd0b5810a809e95588f707601412fcf428fbf57c4116d3b64fa0e35d83f84.png)  

>知道版本重点就在这里了，如果不知道怎做的我给个参考方案地址：https://blog.csdn.net/weixin_40643324/article/details/143920994
>
>诶我好像复现有点问题看看什么原因
>

![picture 4](../assets/images/a8890556f1943aea3eb8940bf3cc2903b1d7fffab97f7db2966f744a1be485c3.png)  
![picture 5](../assets/images/2cfc6c08a9d7eb09260143e55df825049a7cd785c40b7dc58afafeaa116dd395.png)  

>逻辑就是创建一个文件
>

## 提权

![picture 6](../assets/images/36f01c9511ce1c75838e542a5f76194d9ab8dd8a9b7a1f0f715487052619eb14.png)  

>这样多开几个进行操作接下来就是最最重要的部分
>

![picture 7](../assets/images/161d946e1e6e91daf0e14ad984f032e69116463371434be73ade8f9a07804d54.png)  

>可以看到很多ip，我们需要查找其他ip有什么，这里是不能root提权的,因为靶机什么也没有我传一个busybox方便我操作
>

![picture 8](../assets/images/de1937310c539eb7665f42a436847ae8f6c905256e809af8d7bc266141c60356.png)  
![picture 9](../assets/images/6fad0b1249177b0ab2006cefeb990cf3f3d257aeee210e4d2314571e0dce83ac.png)  
![picture 10](../assets/images/14fe79c3d839f1afdb88bb5701c2882471c13aca3eec67f945f9d2bbfe0778b2.png)  
![picture 11](../assets/images/1d12e64ab257ec339ae8dec48e47a600c107cfa896d9e8b3d31e0bef1ad630cb.png)  

>主要就是redis为授权主从复制漏洞
>

![picture 12](../assets/images/9917a48cd0a4d41893ed5d77796e9118d52429add45ddba3c5af182a6ae54f54.png)  

>工具自行下载：https://github.com/n0b0dyCN/redis-rogue-server
>

![picture 13](../assets/images/8fdec826487d14b05e6bb883ae21a218a00d4d69e52ca71cc538c14e2d2e26b2.png)  
![picture 14](../assets/images/8e0ea83bcfc769371ffd19d59ca0044d326f9124ca2943b024a8a7ecbae7017b.png)  

>当然这里有一个小坑，不过对我没有影响
>

![picture 15](../assets/images/88e48d937ae6197bbefd6226575aab6a2837eeef63d4d041aba820714fb5a626.png)  

>好了这个地方是有密码的，这个是第一个机器的，不过我顺序有点反了因为一拿到shell就应该看这个
>

![picture 16](../assets/images/d51df1415ebf07e4cf60e7e25ac2917e32f038379b1cbbc7a59dbb4ff63f129d.png)  

>最后一个考点比较考验人
>

![picture 19](../assets/images/b52ac08e3c593787a335781052baa775c1041bbd41a7d100976ef3e7acbf3fa4.png)  
![picture 20](../assets/images/90bd3af551958b2f6d67acd64c32ea7657aadb72028e3d75539c28878c051d65.png)  
![picture 21](../assets/images/3f65d51ff5ab1f5452b1cd98cdebc38c19aad6d4e3fc692e239ad9802e180222.png)  
![picture 17](../assets/images/d3abd73769c04ef87dcfe525bf267a394b72d042a52b1f65a0c8fcedbb75c03c.png)  
![picture 18](../assets/images/f830f0c7f6df3ceb48d0e66133de36101c0f4083e1d0131f2fc50aa787c438e4.png)  

>复盘结束，感谢bamuwe大佬出的有意思题目，非常有意思每一个知识点我都尽力去想和测试，虽然我是测试员但是还是在路线下学习每一个考点，是一个非常不错的靶机，推荐各位大佬去玩玩！！！
>


>userflag: flag{user-4f6311d4cf5776f0316c2f1b6526a653}
>
>rootflag: flag{root-6dbfaf239023f6da6ed2ffc59d3bcea5}
>