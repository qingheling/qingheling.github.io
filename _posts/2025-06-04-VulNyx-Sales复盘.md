---
title: VulNyx Sales靶机复盘
author: LingMj
data: 2025-06-04
categories: [VulNyx]
tags: [SuiteCRM]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/tools# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.243	3e:21:9c:12:bd:a3	(Unknown: locally administered)

11 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.103 seconds (121.73 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sC -sV 192.168.137.243
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-04 08:27 EDT
Nmap scan report for sales.nyx (192.168.137.243)
Host is up (0.029s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u6 (protocol 2.0)
| ssh-hostkey: 
|   256 dd:2c:11:05:8e:0a:ea:0b:df:52:60:ed:bf:b4:c2:92 (ECDSA)
|_  256 9d:5a:c5:8d:db:27:66:ca:35:30:05:1f:ad:25:40:3f (ED25519)
80/tcp open  http    Apache httpd 2.4.62
|_http-title: AksisDesign
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Host: localhost; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.75 seconds
```

## 获取webshell

![picture 0](../assets/images/490501e94c6efdc677d867c985c29a388d986f9b9e59e256baa5bb188ea579fc.png)  

>存在域名
>

![picture 1](../assets/images/a29e9d670cd402e0180c9a246b541044acb9c074b4085a42056fc49061760aa2.png)  
![picture 2](../assets/images/1153ff347decf160d25c29ad8a344729dac582f208d38bed244d5a13fb4cb597.png)  
![picture 3](../assets/images/bb240f264ff90e1b66a6cabca07183eaab22699e5993cb358e2c5c5c7e007c16.png)  
![picture 4](../assets/images/43ebceb6c5c5611effe0f42adb1df24462e1a7fb93095965732dabad2894b967.png)  
![picture 5](../assets/images/2717554cf96926f85482dd2d451018a1869d1f5d0768b612da011e89136ca9c1.png)  
![picture 6](../assets/images/7b7a99d161f7d376808e42fcef8cecea57cd6676b70bdb97c2a761e7b957963f.png)  
![picture 7](../assets/images/07153bbded8927f19dc16207b45a3780840045f72c78603fb0e6861ee9aede51.png)  
![picture 8](../assets/images/12ae05934dbd868c31a5b4ec9e209ceb4be3e1995d07a2eec43e3cf60b3955fb.png)  
![picture 9](../assets/images/b98ddd370568144b296ecbf27dcdcbddd03aa70b2bc1bd1ffe37dd7174587613.png)  

>方案地址：https://medium.com/@_crac/cve-2022-23940-rce-in-suitecrm-90df53980d8c
>

![picture 11](../assets/images/99433175218059700f140af197cad596251932e507e35b978371078f91f72b24.png)  

>直接利用
>

## 提权

![picture 10](../assets/images/84fc12ebfc9dc997682ac961cc559b0d18a6dd54b012ca82705ae9e09fd66026.png)  
![picture 12](../assets/images/77278e6f93e602670b2819d48135c147a245fcc790a74c8662091950c5b180b7.png)  
![picture 13](../assets/images/ca0697a968b582105e83130bb01b8e528bf513f62848717ee9d7a896bc450d77.png)  
![picture 14](../assets/images/77c202c70f9e818d36fd85a9417ec32a74f9eac4c6cdb05476d53b91827ef7ac.png)  
![picture 15](../assets/images/e375dbe1a7741e93b1fd8d3773a6a25ca17198abea835c4f142068b4ca53ff83.png)  
![picture 16](../assets/images/32c1fee719a6f2a96812e0e89beecb850387aa4dd2cea2efbc6d7b4c6d6e7036.png)  
![picture 17](../assets/images/0805864f128565bdb02f038624c613eacf6cdbbdba41bb54f0bcf1a6e5c26ff5.png)  

>这样就结束了前面可能难一点
>

![picture 18](../assets/images/3b534f6e69fae4d162a14a63d54ee50d5374ccead70d3506d83891f0725be49b.png)  

>和大佬同框记录一下
>

>userflag:
>
>rootflag:
>