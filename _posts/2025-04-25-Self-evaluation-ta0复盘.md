---
title: Self-evaluation ta0靶机复盘
author: LingMj
data: 2025-04-25
categories: [Self-evaluation]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.55	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.135	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.224	3e:21:9c:12:bd:a3	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.130 seconds (120.19 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.224 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-25 06:09 EDT
Nmap scan report for ta0.mshome.net (192.168.137.224)
Host is up (0.0096s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
5000/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.9.2)
|_http-title: \xE5\x9C\xA8\xE7\xBA\xBF\xE5\x9B\xBE\xE7\x89\x87\xE8\xBD\xAC Base64
|_http-server-header: Werkzeug/3.1.3 Python/3.9.2
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.41 seconds
```

## 获取webshell
![picture 0](../assets/images/2e3d3fecc5ca77e082edaf9f5dee53d258a35050b485838a668071aef31dac11.png)  

>文件上传转换么
>

![picture 1](../assets/images/68e78a0d28d3f8b323f916fa3b992a5067736ae6396f76c98dd32badf0e3350b.png)  
![picture 2](../assets/images/04b00e563fb3c2deaa70d1ba17aa4129cd7a627fb148736f4420353a510df86a.png)  
![picture 3](../assets/images/73e61156e730c109f3092a315450554541a3d23ef0aaf9c28482efb546acd13e.png)  

>哈？这样也不行要base64转么
>

![picture 4](../assets/images/b8dc30194fecf682f0fbdc0dd28e80bba35092137a689fb759034c3aeb2d980d.png)  
![picture 5](../assets/images/c5ce01d828d6924b6274e49df268f7a5f50a537daab05f61b4a39a079d7da8e4.png)  

>这个页面没东西
>

![picture 6](../assets/images/20430fc61ad44d7e980022f285e972a7f82367abec562cca8faabdc33f79c3f6.png)  

>单纯宣传么，哈哈哈
>

![picture 7](../assets/images/2070b1f542ce89685aef5544e2eeba3cb94c6b26099480302ca74376b74c3105.png)  
![picture 8](../assets/images/ba26b8a5d9795ae7241f200288208ad2a8eda1e2a287075667db18ac477b7f6c.png)  

>传啥都no hack啥玩意感觉要做不出啦
>



## 提权

>哈哈哈哈哈
>


>userflag:
>
>rootflag:
>