---
title: thehackerslabs JaulaCon2025靶机复盘
author: LingMj
data: 2025-03-29
categories: [thehackerslabs]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.116	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.172	62:2f:e8:e4:77:5d	(Unknown: locally administered)

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.062 seconds (124.15 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.116
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-28 21:58 EDT
Nmap scan report for JaulaCon2025.mshome.net (192.168.137.116)
Host is up (0.0080s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 af:79:a1:39:80:45:fb:b7:cb:86:fd:8b:62:69:4a:64 (ECDSA)
|_  256 6d:d4:9d:ac:0b:f0:a1:88:66:b4:ff:f6:42:bb:f2:e5 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Bienvenido a Bludit | BLUDIT
|_http-generator: Bludit
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.10 seconds
```

## 获取webshell
![picture 0](../assets/images/44c4c24d495633df19db113db16af3a7891eca0a5b430a21ee12b3e875a047d3.png)  

>域名
>

![picture 1](../assets/images/da31041a6338fbb7f7c3ea96a136589433ccc31c0bf37b1ffc02dd50693ea405.png)  
![picture 2](../assets/images/e2fb6e90c80f55f94e5235f11ba631d701992aafde5b243b6564b3b09ec551ce.png)  
![picture 3](../assets/images/ef1dc4c74c52e1d5f980f956a6b582c631b55684db8c3bc2d5b4ba8d8dc4bae8.png)  
![picture 4](../assets/images/5be2ce214fda0b02d293f9ddf789063bbbb7fb34d37796934f641a46c7a71352.png)  
![picture 5](../assets/images/a187ac675e5049ef4ae0b0bde3f6ad5f2b92c74b7908aeed5dcfa5d3a4ac2f68.png)  
![picture 6](../assets/images/a5c000ec6c623d2aace5a11c6ab6a512d29e9a4e97c560b91dca4ef52f3aaa6b.png)  
![picture 7](../assets/images/482d5cd5057d817f812861c7d6107c4771fecafe7e9555be1f90f312a7d20a04.png)  
![picture 8](../assets/images/80371ddb66e30fa1d52ed2c935fed68fd9667fd5b48e4cd87cb3e51ca0b55bba.png)  
![picture 9](../assets/images/0b97f3bbffa0c88bcaf84fe39b585b6eacd6ed065f27ed47373d80d5487385c4.png)  
![picture 10](../assets/images/d115528ddd4288fe9f3d0960887ea7f81121e752d94ac5f250635c6b154e2a2f.png) 
![picture 15](../assets/images/761bc28033239ce4dbae4e910b02627cb9b7d4b89d8e610a61d77baac4604011.png)  

![picture 14](../assets/images/b692a5a6bef7b7e4719d5ff8f8b8663b04cb736c546200d373290ac8a123cda0.png)  
![picture 16](../assets/images/eb66b245c4031af033b83d1553bd246a203ef0dea11e5d0540f894b4809cbeac.png)  
![picture 17](../assets/images/8ac0d00c2cdb880c7ba5dc979c40038bad735a89d4eb62fea1903464973efa4c.png)  

>原本我是手动上传的，大佬来一句msf的使用，才往下继续
>

## 提权

>看了没sudo -l，就是找密码了
>

```
www-data@JaulaCon2025:/var/www/html/bl-content/databases$ cat users.php 
<?php defined('BLUDIT') or die('Bludit CMS.'); ?>
{
    "admin": {
        "nickname": "Admin",
        "firstName": "Administrador",
        "lastName": "",
        "role": "admin",
        "password": "67def80155faa894bfb132889e3825a2718db22f",
        "salt": "67e2f74795e73",
        "email": "",
        "registered": "2025-03-25 19:34:47",
        "tokenRemember": "",
        "tokenAuth": "70b08e65a3fa16d434ca40e603c99e22",
        "tokenAuthTTL": "2009-03-15 14:00",
        "twitter": "",
        "facebook": "",
        "instagram": "",
        "codepen": "",
        "linkedin": "",
        "github": "",
        "gitlab": ""
    },
    "Jaulacon2025": {
        "firstName": "",
        "lastName": "",
        "nickname": "",
        "description": "",
        "role": "author",
        "password": "a0fcd99fe4a21f30abd2053b1cf796da628e4e7e",
        "salt": "bo22u72!",
        "email": "",
        "registered": "2025-03-25 19:43:25",
        "tokenRemember": "",
        "tokenAuth": "d1ed37a30b769e2e48123c3efaa1e357",
        "tokenAuthTTL": "2009-03-15 14:00",
        "twitter": "",
        "facebook": "",
        "codepen": "",
        "instagram": "",
        "github": "",
        "gitlab": "",
        "linkedin": "",
        "mastodon": ""
    },
    "JaulaCon2025": {
        "firstName": "",
        "lastName": "",
        "nickname": "",
        "description": "",
        "role": "author",
        "password": "551211bcd6ef18e32742a73fcb85430b",
        "salt": "jejej",
        "email": "",
        "registered": "2025-03-25 19:43:25",
        "tokenRemember": "",
        "tokenAuth": "d1ed37a30b769e2e48123c3efaa1e357",
        "tokenAuthTTL": "2009-03-15 14:00",
        "twitter": "",
        "facebook": "",
        "codepen": "",
        "instagram": "",
        "github": "",
        "gitlab": "",
        "linkedin": "",
        "mastodon": ""
    }
```

![picture 13](../assets/images/f669e28e995baefb28b2d52ebe72c8c74354532f1a6677a7b2ab21695f9f7d1b.png)  

> 这个user密码对我来说赶上medium了哈哈哈，压根不懂咋做，拿了提示
>

![picture 11](../assets/images/93f185be2d4154c10dff11fa442fd6cf32f05491e952d959ceb0a61299f65e19.png)  
![picture 12](../assets/images/e47693945d2b80d39aff26da7feb463645aa62adc2f78f90b2fa7b0300d6a458.png)  




>userflag:368409a919088e8707d0617365156184
>
>rootflag:097fac9db83a1806f3355cf95227992a
>