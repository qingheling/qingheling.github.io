---
title: VulNyx Cache靶机复盘
author: LingMj
data: 2025-01-17
categories: [VulNyx]
tags: [proxy,brush,ssh,write]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.167  00:0c:29:dc:fe:67       VMware, Inc.
192.168.26.254  00:50:56:e2:a3:32       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.552 seconds (100.31 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.167
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-17 06:06 EST
Nmap scan report for 192.168.26.167 (192.168.26.167)
Host is up (0.0038s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp   open  http       Apache httpd 2.4.57 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.57 (Debian)
3128/tcp open  http-proxy Squid http proxy 5.7
|_http-title: ERROR: The requested URL could not be retrieved
|_http-server-header: squid/5.7
|_http-open-proxy: Proxy might be redirecting requests
MAC Address: 00:0C:29:DC:FE:67 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 71.43 seconds
```

## 获取Webshell
![图 0](../assets/images/df70e0bf097969b9c54de03ebe333ab26b5e73b342f1c406b12edf172af63461.png)  
![图 1](../assets/images/e108c1af2fc104cbeda8f5c9fb6a61ba4a2c0a85c6dba85351a09e4f424b01f2.png)  
>从所获信息看是一个代理认证身份的操作
>
![图 2](../assets/images/7689c904fab5b16c9c6705d6758647ac099b8dc67f3bf939aa5104777f774530.png)  
![图 3](../assets/images/f5ab3e81274c15dad8402a606416d67834a5cae90a552f8c9ae22f14310612fa.png)  
>这里先试这利用一下工具，再去查点有用信息
>
![图 4](../assets/images/31e5b7eda4e184796ac25f8c29b75aef956de5ffea0a40cc5a739c7acba58a52.png)  

>没看出什么线索，去web上看一下，并且扫一下目录
>
![图 5](../assets/images/7628c6b603556498bf743c453bfea7630a6f2d6e7ca8e0ce171dffcafd0a2272.png)  
![图 6](../assets/images/d8109ad72ca3f08df0789b1d06d81ff63f1831a434d6397f5ca72ed74db98784.png)  
![图 7](../assets/images/32295a35e83e6e7caf568f4c11bb1cf919b77302e44af4f5d6fdb2ae73dae04a.png)  
>搜索exploit上可以看出一下对于这个版本的利用方案
>
>发现无可利用的，去gpt查询一手解决方案
![图 8](../assets/images/1f66fd2a96e19ea774ca5f7cfb930fb4b30a9b40f50e197152339e9702c25023.png)
>貌似这个线索表示的是需要内部的服务器地址访问，可以试试127.0.0.1
>  
![图 9](../assets/images/b2c153096f924fd135474a27a87a90fb2bcf8a68b62478d1d4f96994cdf8fbec.png)  
>代理上了但是80目前没有有用信息，可以试试其他端口
>
![图 10](../assets/images/395893e16c3e9e2ccc2ad034f9e886b95313dc907fdbdd4113438059f2a23026.png)

>利用一下wfuzz进行操作
>
![图 11](../assets/images/908e766d85033956ec66087a235e89cf37210cb51b08af41a559b35c64a7e140.png)  
![图 12](../assets/images/b3ffdf68e527c474d187581318022510ab13ecbcff1ac570b6fb591ac661d702.png)  

>有线索了，继续打
>
![图 13](../assets/images/f69ba7846bcd9d3c34f77f8b229e38cad5559f5abc13c71c354605e6b6ada351.png)  

>这里查一下目录，看看有隐藏信息么
>
![图 14](../assets/images/fe7e73341d9ec57b87f92229422e411056f2d5aa012b8cbab99a12d063645250.png)  
![图 15](../assets/images/ffbf213ce917961adc28c919b41014cae0c5497f507e77afb52983ebe1de1b8b.png)  

![图 16](../assets/images/17183a0fed91734557acff54c42ebc5f415cf157ffa3c4cf823a7caf48e3ddb3.png)  
![图 17](../assets/images/c094f426846c4503906781869b4689f8da44f3f6586b5ac1445696e8f2600e5f.png)  

>到这里就能拿shell了，不过应该需要爆破一下密码
>
![图 18](../assets/images/e8652124f129a4e4a2d2a04b5f0ac366faa604a74c65511913254619b980440f.png) 

>发现竟然不用爆破密码，但是这里没有发现用户名，需要查一下
>
>找了一圈没发现用户名猜测需要爆破，测试root没成功

![图 19](../assets/images/fe1878a33791919b4ad67dfe2c14c827996fd070d3d91da09a9dcfff4757cc70.png)  
### 方案一
![图 20](../assets/images/478e66d264086a42eb25200a60a5c69cb275b50835a848e3ac86aa22461e64c8.png)
![图 25](../assets/images/64f53e3420f0c3e2d1acdab81a4b032fe3f186865269dbb0c3e6443c6128580c.png)  
>这里出现了这个用户脚本直接挂掉证明登录进去
>

### 方案二
![图 21](../assets/images/e60c625189aaebf525c3b773d0f09d840eb0020c6f5cf2adde98a901c76d7f55.png)  

>方案二来自ll104567大佬提供
>

## 提权
![图 22](../assets/images/b31684ff88d364983b495456395ae453679f90450c758398a263e055596975eb.png)  
![图 23](../assets/images/3eb7302db18bffe01e6b86302039285f540ce74990ceefeec21bbf171269b931.png)  
![图 24](../assets/images/f2024ba493cc8090d8549648c9fbb43d225c90379ba47e1c890372cb2cca4a85.png)  

>这里没有出现对应的sudo -l 提权我们需要去寻找可以利用的点
>
![图 26](../assets/images/d4e4f67d22e94ffe10d17f6dd9ca9efa276d20fa0f2470d8d6f3ec5ff5779271.png)  

>这里利用工具查询一下服务
>
![图 27](../assets/images/53ae6fbe8587d0735d2f8c85d45503026a90c3b330df59297a8c644e701501d4.png)  
![图 28](../assets/images/a8f7c28ff16f54cf52e10bec735ba245672fdd885db187550c8aa7d0545cb54d.png)  
![图 29](../assets/images/4462916a0b3baf0ac4b1328594c7d1b89a6ce6d4e20acd2c7ddd78d2757c627d.png)  

>好了到这里整个靶场就完结了
>
>userflag:a9d46582a96fabcaa2736d6bed398144
>
>rootflag:4034da58b08629f91fdbbb89bdd869a4
>