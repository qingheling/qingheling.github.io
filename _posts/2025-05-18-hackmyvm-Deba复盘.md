---
title: hackmyvm Deba靶机复盘
author: LingMj
data: 2025-05-18
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.42	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.049 seconds (124.94 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sV -sC 192.168.137.42
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-18 07:14 EDT
Nmap scan report for debian.mshome.net (192.168.137.42)
Host is up (0.043s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 22:e4:1e:f3:f6:82:7b:26:da:13:2f:01:f9:d5:0d:5b (RSA)
|   256 7b:09:3e:d4:a7:2d:92:01:9d:7d:7f:32:c1:fd:93:5b (ECDSA)
|_  256 56:fd:3d:c2:19:fe:22:24:ca:2c:f8:07:90:1d:76:87 (ED25519)
80/tcp   open  http    Apache httpd 2.4.38 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
3000/tcp open  http    Node.js Express framework
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.47 seconds
```

## 获取webshell

![picture 0](../assets/images/b161d350ef4c2dddcf4a01e539c4fdb437c4b7d2322b9a17dafc8c95b09a6a67.png)  
![picture 1](../assets/images/ca30382486793832357fd0f5bf3234e19d47f88cba522512f476492ac7a8e077.png)  

>没收获
>

![picture 2](../assets/images/845ca7a707b87ad66169e6861a3dee14999493679f361f6d75804ffeb271d5e6.png)  
![picture 3](../assets/images/0a9a054b43bc9c5e0ac62f012607ddb10886e91e8bd254ec9e817fff1c8a9f97.png)
![picture 5](../assets/images/b38b9267d60c280417e5a12b0b36b2e25d4e19dd4f9fad9e1e3d8d8a2e8034bc.png)  

![picture 4](../assets/images/496438f7cf7e205bb617d97c503d2a6a38dc1e115f057fdbe3f9c8d11bd9ca7a.png)  
![picture 6](../assets/images/d5b285c38c42ab393fab16a104e534deb68ec9398e5810ba1d02b8747b1e34b3.png)  
![picture 7](../assets/images/84a711120ad2d0dbe1ae9cfaf1428f9dafa09ca9674b7fc689f9906921d579d4.png)  

>随便试，没有线索我查查这个nodejs
>

![picture 8](../assets/images/a99e92395942dde9ab2bd0c7caa96090c9fd69ecfc8baf22ab0aa9669d3cfc44.png)  

>不是这个
>

![picture 9](../assets/images/f54e1b8a018edc58f5cffc8464f996989ef0f6502248ba30bbcdc18544ab65fa.png)  
![picture 10](../assets/images/2a25cb6b782b1b81e3c372cf8041a7fc1510a777e44854dd91934f521ff2ac44.png)  
![picture 11](../assets/images/c759b591d61000d913bbed6210692187514af99dd9a59b44ce3837e73aa388d3.png)  
![picture 12](../assets/images/622fe447dc264f95181be1aa85df402fed60bec15c40742adb684472a04795d0.png)  
![picture 13](../assets/images/a8cf95ce00c646df18d3c463f1b09c982e6988304844dda82ca970117a11c10e.png)  
![picture 14](../assets/images/e8f404d3f1ee0e13e031e4a39efa6f8215f27382c8fe94b99f4e3a23f381cf49.png)  

>没成功但是卡住
>

![picture 15](../assets/images/28efb834a95cd346c00e1eeb172b673fbe7855ed9333aca268623bf2f39028ea.png)  

>方向不对没弹回来
>

![picture 16](../assets/images/1b8c8df61a850c9df761a734f17f040860001dfe598b58812f70a7a4d2faa5d2.png)  
![picture 17](../assets/images/c66f63120c324e137f9115ab9d2a9a9ffd95c2f7f9fb156a3f6f48929d7438c0.png)  
![picture 18](../assets/images/11b7464372648a7a6912586fb1fd228accd8258c184b9023a61eacb6d849433a.png)  

>一摸一样确实是这个，地址：https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/
>

![picture 19](../assets/images/3a57d56ded9f24c9fbf1cbd50dbb9e54f454eb59e57cf6ab93c0468cdc4959a8.png)  
![picture 20](../assets/images/00f17c12b6dace3bff6c4d65a1fc2f59b5f3db0ffbe10afd5dfa9464b95fa5f0.png)  
![picture 21](../assets/images/322637db5f7251dd55d4a3e709b6471c753b2c051ad89a4ac1be37c253d5f6be.png)  

>还是没成
>

![picture 22](../assets/images/30197cb220b35ff24a46b442a361796ae26d8e0f8e226ca71f07edbae0fe0a61.png)  

>好像靶机bug了
>

![picture 23](../assets/images/39b5044bdec2344b836841bee39e23c4c57e64e6bb8af512bbcef5a1f9b1a8d8.png)  
![picture 24](../assets/images/cf0ef19c015731239748a65c2d99b52216b9874c321c1ef65b4c89d40c8aea8a.png)  

>好了出现了
>

## 提权

![picture 25](../assets/images/0f77adcf415aad19493d808da8fa026ad3983f9157cee818deaf26b51d0cb0ca.png)  
![picture 26](../assets/images/f976c9766f9a7757f5d92ab3abc3e9905e73a1eaaad50d56a29bd657993e6978.png)  

>可以改main直接bin/bash
>

![picture 27](../assets/images/34a05951f57a6a3ae830327a5c09d2a5605f43554bf5c7f172ceab44964fb4df.png)  
![picture 28](../assets/images/076b9b4c96aa14f7f128b4bb3fb2f0ca435ce76e108be5301792dd5efb7205cc.png)  
![picture 29](../assets/images/27ec374654d9959e06c22226f45b61f2dee0b8617c14a6b7ea0018c72b3b1631.png)  
![picture 30](../assets/images/a352898ea07ea7043f8267b04db6f168c53c72d8217ebe43a55b5c43825effad.png)  

>我刚想一下它是all啊我直接root得了，哈哈哈哈
>


>userflag:justdeserialize
>
>rootflag:BoFsavetheworld
>
