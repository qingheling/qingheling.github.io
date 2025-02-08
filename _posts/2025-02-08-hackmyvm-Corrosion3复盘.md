---
title: hackmyvm Corrosion3靶机复盘
author: LingMj
data: 2025-02-08
categories: [hackmyvm]
tags: [runc,LfI,phpfilter,python3]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:11:42:18       (Unknown)
192.168.56.145  08:00:27:47:de:e7       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.861 seconds (137.56 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.145
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-08 05:32 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.145
Host is up (0.0061s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
80/tcp open     http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
MAC Address: 08:00:27:47:DE:E7 (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.14 seconds
```

## 获取webshell
![图 0](../assets/images/eda8de410477a94cc64c6f8493f7cbec86020d98d9d3225dafe93512b8d8c9fa.png)  
![图 1](../assets/images/9b80b79baf201d93a68e7d27ebea775f4725ad33342b78041eba8780aa353d10.png)  
![图 2](../assets/images/5747b734ef8432d845e18e584c02e780815b3532fbf40cec22b87ec794d51a03.png)  
![图 3](../assets/images/bd8dbebede14f369b7419da940966e4fd63fa09d26786bcd1fc700fe3baf8da3.png)  

>检查了一下不见啥玩意，看看扫描的目录吧
>
![图 4](../assets/images/ee768a01c0f45722427052b6dffd36641d74514a72502103fea91dc853d4fe78.png)  
![图 5](../assets/images/3c02ac1413d271dce6ebff9ca520e6b62a6f8880a50842126564fad18687367c.png)  
![图 6](../assets/images/50558e3ad728a43e35e16b445682179b843f60f61407d1b355ef5ef3d4caa0fb.png)  
![图 7](../assets/images/edde762fdb4cc5bed7e1f17a15328cce2106d86e1c22ca5bc899c449c8c8899a.png)  
![图 8](../assets/images/c4e379e697d0e6e985db801beb4d49fd4a2c70df3be3d95f3134e4ca910b2ef9.png)  

```
POST /login/ HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://localhost/login/
Content-Type: application/x-www-form-urlencoded
Content-Length: 29
Connection: close
Upgrade-Insecure-Requests: 1

user=randy&pass=RaNDY$SuPer!Secr3etPa$$word
user=test&pass=test
```

>不见登录页面目前
>

![图 10](../assets/images/b9af8a5884c9f22511c404ebfb00edb7dbe6bb924b39d0dbb262e7a7515280c1.png)  

![图 9](../assets/images/b4db62e5acfbb9c84be00df451730c473f4e72d19b45c0140befe813c156bf2e.png)  
![图 11](../assets/images/de3fadd34de953f99f7346bb9f0f71b50700c27be9baf43a1e4884c1e772158b.png)  

>是一个模板，也给了版本
>
![图 12](../assets/images/69ecef1248fa4c8d36ab50068d53052ef953d451e4e4db47a9b92e50542acac7.png)  
![图 13](../assets/images/1dff8d164cfa2564c8fa4a12f305354b9f543effbcec2c95c9e07fb805f3cf8e.png)  
![图 14](../assets/images/df3924fe9e713913983e1ca793f97a4d4dd5682f327c8ca9581a80264bd7e610.png)  

>确实有怎么一个东西，不过我还是偏向于找一下登录页面，不然密码都没有尝试位置
>
![图 15](../assets/images/4c11bf7c3474704a8403046a4823ce59dddff417e546eb085a1dafc8b94681ed.png)  
![图 16](../assets/images/68abb134dc96053099980d66ba9037ee981bfbd0227006d0580d8b8e868c8832.png)  

>看一下是否有LFI
>

![图 17](../assets/images/c3971e927663e693520f86b8714f951d0ceef12f281c9d462b7c525bd7c6240e.png)  
![图 18](../assets/images/9b7ab5b0cb5f351646fbf4a1d1b154f2bbee07ef2cc02d72d6c20064fe98b836.png)  
![图 19](../assets/images/5e1e4556037ec258e0e221d8602c4e117fbe977614b665280face3578e2ccb9a.png)  
![图 20](../assets/images/5aca0c77cff0837652760c4f2663bd381c790fad19d19e74918ab224324e2531.png)  

>user flag,还挺简单
>
![图 21](../assets/images/589fcc17371f43c2b58f11f6f9f8c0517ffe321c6ec9b7958a1bec0b54fa78fa.png)  

![图 22](../assets/images/9125e67f3d9eeca3896aeea123d18e93e66de9837a4f75d79257545d593e15cf.png)  
![图 23](../assets/images/71ed475270737d9dc94a7564853fb863a13b08a94041925712fe5c947dad73df.png)  

>一切正常
>

![图 24](../assets/images/b3438cd5c20dfe6a52129c95183c3bb55892888b673c68dca0a1d520ee251506.png)  
![图 25](../assets/images/02f80d965c9127244d57eb92ef41f788d0b1c98ea1d6291b4fbe21fda048979c.png)  
![图 26](../assets/images/4e9ad82385364e3a0cc3b443c2fb42413562668877ea9d8be7ff0f5e66b3c273.png)  

>用busybox
>

## 提权
![图 27](../assets/images/41f53f7c442c5c99e8b8060cc233fc8e014cec59077fd6b9f00e52892ad0399b.png)  
![图 28](../assets/images/77ff2876103bfcf34977f21626dda61839ffe32bc44f24a31f979bed053511aa.png)  

>密码之前log的
>

![图 29](../assets/images/12059cbac0f96620c3c1d24db87377ec14387738aedb603ea8307a3b553a25ba.png)  
![图 30](../assets/images/fcb78e2b0644420066d742ee7dd4c1783408702d890c62b840437fea01e5fec8.png)  

>跑一下脚本吧，目前的信息基本拿了
>
![图 31](../assets/images/c0791dbdb5f555e8cbde292e672913f02f3cd000855c3bdf11d9f919a38bd90c.png)  
![图 32](../assets/images/6052edcf46bd5a490ade2d378ec343312ec02631dd0bebfc45b1374a5a3cd7f2.png)  

>看看定时任务吧
>
![图 33](../assets/images/0e98d41f5237cbd2c8ce57daecba807e7800c738517af0d98c1f39c78802cf5b.png)  

>有一个用户在跑,我可以写一个脚本在这里进行操作
>
![图 34](../assets/images/623dd4453c3514f656dbf428c9a9a63b45dd3ee13dc79aa747c8ae0e93bb2c91.png)  

>明显没有弹回来
>

![图 35](../assets/images/3e9189fbace7c88cb9962a60f11cab211841d349e90134126df9495a5cdf00e4.png)  
![图 36](../assets/images/34d1c162bdc91aa186736eb001318ab0da2c143836570aec147780a0b03c3f17.png)  
![图 37](../assets/images/aa41506a0bab9cb40f8588f3640ba9caf32d6a598f9f683533da436750456f79.png)  
![图 38](../assets/images/59a8dd64663df3e8e00a05572c77841f68643428eb7d97f881e006fdc6082f03.png)  
![图 39](../assets/images/47b81211dadc36e0f7997d05ce412b0afa996cbcb210ee887a141c9c54c7ef45.png)  

>之前做过，不过我一般不记，又是研究时刻
>
![图 40](../assets/images/e785db700418abec6b660d600965b66346166229ecf3ffaa57b94c08f2c819e0.png)  

>无容器需要自己创建
>

![图 41](../assets/images/74eb701d568f3b45f610a6c78aff9bbd4969d3192890cb0fa86d3a453172e03c.png)  

![图 42](../assets/images/8e5d5ef10a5df37ca0ba132fb02786ac7b3ff55b360edd67fff4e7eced7e6509.png)  
![图 43](../assets/images/5a6bd9a478825141cd493562b98c67f1a14bdd0393a068a39177893fa3ddfa96.png)  
![图 44](../assets/images/d865ff15344f07e610553dddd01400e9369e61f08a1b3aecfb84c66bcab21341.png)  
![图 45](../assets/images/fb9dc19ca9cf35c16f1f8b4df0d441de437bfd0d8edfc6ba25c2c6d046390874.png)  





>userflag:d3a6cef5b73fa1fb233ed6a0e3b9de01
>
>rootflag:18e8141ab1333a87c35e1fad5b394d66
>