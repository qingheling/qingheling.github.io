---
title: hackmyvm Bunny靶机复盘
author: LingMj
data: 2025-05-21
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/tools# arp-scan -l                     
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.41	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.060 seconds (124.27 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-21 04:38 EDT
Nmap scan report for bunny.mshome.net (192.168.137.41)
Host is up (0.0071s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 98:7a:07:5b:ed:f7:76:e3:f5:2e:10:16:ba:61:dd:77 (RSA)
|   256 bc:f8:11:12:e7:cb:20:c5:6c:87:00:b5:57:43:22:d3 (ECDSA)
|_  256 9a:61:00:d8:47:fb:7c:b1:a3:4d:4c:f6:8d:5e:40:59 (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.38 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.14 seconds
```

## 获取webshell

![picture 0](../assets/images/73dd4ca392ad5c6100cbae10550185ca8729787d69155623abcf86290d08323a.png)  
![picture 1](../assets/images/d1a114fd4d8b7709cc7854b6102871c17389aa77283e197732b690d7673a34f6.png)  
![picture 2](../assets/images/af8a7b778e4d548781105a70c2c0866b12a895526f4c9362bf787af9751e58b8.png)  
![picture 3](../assets/images/9406d86fa366e68c4112ed4b767537697f8e0714ebb3ca01a23f415ea4dba7ae.png)  

>普通图片
>

![picture 4](../assets/images/a5be88be4ed9567f20b0cbab4506c645a9068ad275d595f135ff79dfdacf3ba1.png)  
![picture 5](../assets/images/3e1c579c8efe838abb70c378b5c6f5267e35f870352538e74a40bc7082a9e80e.png)  
![picture 6](../assets/images/c97b752ad0cb22f0221c32e66e2c1832878c6bb9117a7614930c098d59e52e24.png)  
![picture 7](../assets/images/c64ff50787cb10ed1b5063cf4ccb0edcead06a402d7950397880e2f1e69da776.png)  

>??? VK
>

![picture 8](../assets/images/2f77d1f76746a0fc1d35c9e4f614728481eed47453a16bbf708093ae49c146da.png)  
![picture 9](../assets/images/5849cd93fffc416eb231f641651e9e83d31bad15e2f710c130f9d9e07b1e7d3c.png)  

>upload.php和config.php都是01他俩拼起来？
>

![picture 10](../assets/images/13f8cad6a318f7235fc9c9a95f6dbe72911d9ebc0d01c46caa2aac4f4c2166e1.png)  

>fuzz了反正都要的
>

![picture 11](../assets/images/855b251959ddaad4a1815041a9ea33041a48d1cd61f82a0a03784a58220a43c9.png)  

>不知道有没有这部分
>

![picture 12](../assets/images/a344c84602e4925693fe227127b7e4504bed08d933beb4da4a9a917c711263eb.png)  

>这个php没有
>

![picture 13](../assets/images/3f0b2dd4a8c946a857cd3b9dcfc92ae3ee65f5aa86d529ea1b49e9c2bd3fca3b.png)  

>有了
>

![picture 14](../assets/images/d639cf12396b878f65aa13de0998f1e9ccc6db214073850b3e99e6282a473b51.png)  
![picture 15](../assets/images/99a62d95ebcbfe84a1816b4acbe1ecd002562fd1b92a03e05984f63a2c9144de.png)  

>有php filter
>

![picture 16](../assets/images/847985c278eb0d307cf85a7b95f43b7dd4546f18e1f387f970f75bc461bb4bb0.png)  

>好了老方案拿shell
>

## 提权

![picture 17](../assets/images/f83cb0a0c572fc57dbb883abb53ef3cae388ac665d7a105c66087313b9ad7b52.png)  
![picture 18](../assets/images/f8db9ef8b7a2c5efbf5dfa626b5bf7b1311e8f010f5c9893f9e16752859c59b9.png)  
![picture 19](../assets/images/7704babb98f71e48f03c3c2268f4f9ac3acf18e2925377d3a03f4c5383d775b6.png)  
![picture 20](../assets/images/e548f421e9c5a1d1aa17e08f00b48975f4ec0bdc403af239b8e879a44e621dc6.png)  

>算了
>

![picture 21](../assets/images/b879bba38dbeab839254ac53b2ee3a0e0b5fac6f5ee863b952d1a93e17142d28.png) 
![picture 23](../assets/images/391682a2ed3205b072c61135013b1dcaf8a304173d0729d74add45d086cac1a6.png)  

![picture 22](../assets/images/8b76e270ce4b1ddaf65bfb1991c63d40ec33c753bb6c65a4fd858888509a9a66.png)  


>这个代码很好看直接输入位置即可
>

![picture 24](../assets/images/a5de4decc238094140c77d3e1610cdf3742199e38e9c6f0305bc734852959dd8.png)  

```
www-data@bunny:/home/chris/lab$ cat magic 
#/bin/bash
$1 $2 $3 -T -TT 'sh #'
```

>直接注入奥
>

![picture 25](../assets/images/44b453e23c64808bf0f25e2debdde0ea56ad9b2107d91641537134a092a61ae2.png)  

>发现内容看看是不是定时任务
>

![picture 26](../assets/images/f22066c773d69dd0181410d4375a8bfb2b133e79cea168258ed3adf03299bfb1.png)  
![picture 27](../assets/images/beea8a88fd31a9b8f707ed03877e0169635b924d33dbe6a673889bc6d29fe2b0.png)  

>好了，直接搞触发就好了
>

![picture 28](../assets/images/84313c875845da7ad5d3fe32310a65c05b207b352481f66bfdc9b9b5f82f202a.png)  
![picture 29](../assets/images/bf7a909fffb39df5886b1d8287747306240483c6ee356b8937772f33f37e7982.png)  


>结束了
>

>userflag:b9c1575e8d8f934a4101fdbec2f711fe
>
>rootflag:536313923133fb4a628f8ddd5e0ed3e5
>
