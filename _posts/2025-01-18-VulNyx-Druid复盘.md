---
title: VulNyx Druid靶机复盘
author: LingMj
data: 2025-01-18
categories: [VulNyx]
tags: [domain,cve,super]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.170  00:0c:29:8f:47:15       VMware, Inc.
192.168.26.254  00:50:56:e0:6d:ff       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.679 seconds (95.56 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.170       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-17 20:24 EST
Nmap scan report for 192.168.26.170 (192.168.26.170)
Host is up (0.0011s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)
|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)
|_  256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)
80/tcp open  http    Apache httpd 2.4.56 ((Debian))
|_http-server-header: Apache/2.4.56 (Debian)
|_http-title: Hotel
MAC Address: 00:0C:29:8F:47:15 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.69 seconds

```

## 获取webshell
### 目录扫描
![图 0](../assets/images/4ebcbf640dffead220210cdb4d04ff3e021860118551498a1b301cd589553bfa.png)  
>可以去web页面查看一下内容
>
![图 2](../assets/images/3e5aece4af12c47d88b4a20613abce238aa192bd86728ea71a6be8fca9bf546d.png)  
>挨个页面查看应该有线索
>
![图 3](../assets/images/f0cb68d850cc4392bb22764fc42172b870ddf5a928388d17ae6a58d8358963f0.png)  
![图 4](../assets/images/8bdbfc7fb090f51b2372cc5539e6a3926a0afc67ff6439b6c262023b7f350dc8.png)  
>存在2个发送界面，测试一下没有什么线索，需要进一步测试
>
![图 5](../assets/images/f30782af91fdba35ab96b5f5f910e69df4996d9b93db7d37944d59e64ebed9e2.png)  
>存在域名部分,扫一下子域名
>
![图 6](../assets/images/6d198a2baba5690b6d5276d8523b8b624ed1f5bb0ea7c211d4691e498dcdfc7a.png)  
![图 7](../assets/images/384fb50c63802518c4e3288095f6a4bce4999580aeb02cd2565f7650ca0f0b47.png)  
>为啥不能访问,缺点东西
>
![图 8](../assets/images/f8b7a3308160b14ca216d4eb691f4b8d6fba40132db744b4dc2ff97a386d61e3.png)  
>用火狐是这个样子
>
![图 9](../assets/images/ec4e1e8e560cac28deaf3b5eed0d1092ac78018344f7ac4bdfb6bc7474e3140d.png)  
>发现是代理没关,整半天。
>
![图 10](../assets/images/0a4673082f69a023a85a7e64ec44f2b152fe790427bd0a253549f7690f18e2c6.png)  
>发现服务版本，可以查一手
>
![图 11](../assets/images/50dde68fbc741aeed689b67753125dc7d5c053f57d2747170106bcb88febe2f9.png)  
![图 12](../assets/images/5297580fe92f741c8440bd1cce8eb83a02496392df92e76f30901ebeb02ed73f.png)  
![图 13](../assets/images/61a4664da794b2bd7cf7e1de4d1c62c68351cfbcde141e6e2971d275c50b8f3f.png)  
![图 14](../assets/images/d479e617dacc1b0f0fe2bba28425175332647aa8778b08acb21adf0ada42fd3e.png)  

>一个python脚本可以直接利用
>
![图 16](../assets/images/a6ba1ed1f03f2347bd196d867d6b94580a6f9e77257c547e873f58d6d1a268da.png)  

![图 15](../assets/images/9995765a481ea36f9aaa81adde21b99acb7f497879049e159abdeb2db47ccf60.png)  
>发现有了命令回显，这里可以自己注入一下
>
![图 17](../assets/images/aa9bd915b19ba7a7fec3ffa842abd075da52c63fa2c1e331d5a9d9cd4d91e540.png)  
>有点问题
>
![图 18](../assets/images/d1632d5df2da0e79d6bf3a6f61d9cb5854e615606754c73f6aad8d176ce5f31d.png)  
>发现已经注入了，所以应该这样使用
>
![图 19](../assets/images/aa5feba3364d13098cf377315f633ded35cc465738c145d78022ddb4f26a7da6.png)  
![图 20](../assets/images/e9a721943f9fecde44c5cf87ec6a546c8f491f888f5c3ca6fcdef5d53d00e75d.png)  
## 提权
![图 21](../assets/images/5790243c5def837f9446f1714e4558132b002a5e395bfc6267611ed707920b4a.png)  
![图 22](../assets/images/0348cf112c91d61655300d13d2520b8be5601a33ae844d4c2ca015ff109ffde8.png)  
![图 23](../assets/images/121a2ac63c5db272be9be14117c40ee9b107b8aa866b38180da05362e8b22043.png)  
>但是没有密码不知道是否是这个提权root
>
![图 24](../assets/images/942e6feb5149e854975cab2d5b2b89ae151075dc5e1c00d3e202bbbeca23bfb0.png)  
>没有找到有用的部分
>
![图 25](../assets/images/9d78fa7834d017a9af3713b696b44bb369f0c840fc939216db972113984b78ce.png)  
>利用工具进行一下
>
![图 26](../assets/images/62997b099d6ad3293357a6ed8e37ebf149add50038cedeaa060ae9044c54975b.png)  
>多了个特殊的权限配置
>

![图 27](../assets/images/b12a25d3f5d98c75a36f2b11b489829223ca1513939c446f34cfcd6bddf97de5.png)  
![图 28](../assets/images/12acb1a089d91db3f5376307d2ac89d30dbc6845ca3de4eb43fefeced26a575c.png)  
![图 29](../assets/images/3cc047d44bdcfbc0e5a4302735b59a8880f43c5560485739db20220fef4f31ec.png)  
![图 30](../assets/images/e39f1c7009816f5a238067f7cb9c49e74e43b6db75e99791aab485735516beef.png)  
> 可以假装uid
>

![图 31](../assets/images/70866c773835a872826e046c9bed0361e3fcb0bc3ce9b0dd90840d60eec754ba.png)  
>可以读取文件,这玩意单纯试出来的，算是一个积累吧
>
![图 32](../assets/images/24b46f61bda8a62f4069403e88acfb807b6d26576075aeb8c5709c8539e54fe0.png)  
>这样是正确的读取示例，
>
![图 33](../assets/images/a346e8e058c592017763cbdea4647c7322a9fd3e6291602cd0a8062d7d1ab881.png)  
![图 34](../assets/images/cad0f9e31ae9a8c59fc4039fde56823d7a9dda79df1e52694fdbb071f09d7a93.png)  
![图 35](../assets/images/dd6aed3838d2003d2244f6187d8da3155b848fca29c3c936f65e4dff853fac4c.png)  
![图 36](../assets/images/aaecd021fb29ade25226a4c85000c0ddd33a1545da24ccb7bd53f4d0233d160e.png)  


> 到这里靶场复盘结束
>
>userflag：afa84b24191651454e5d2a80bc930618
>
>rootflag:1261b7a8c3b99b0daded8caca8b4023d
>