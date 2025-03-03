---
title: VulNyx Spooisong靶机复盘
author: LingMj
data: 2025-03-03
categories: [VulNyx]
tags: [LFI,log,user,arp,dns]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.66	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.106	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.92	62:2f:e8:e4:77:5d	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.039 seconds (125.55 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap  -p- -sV -sC 192.168.137.106    
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-02 19:12 EST
Nmap scan report for spooisong.mshome.net (192.168.137.106)
Host is up (0.014s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/kavin
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.40 seconds
```

## 获取webshell

![picture 0](../assets/images/826e101656e8e4baafec1f2dcb7a6310c9102954ea2f0ba85c40e80cb33d7876.png)  
![picture 1](../assets/images/6079cb15ebc80906baf5bcfe5d56df29e26f83bb0a426121fec79e98bcfcf6cd.png)  
![picture 2](../assets/images/3b7238a39c351f515083bbc61458393a6584c53bd2d3384bd877f409a8badeef.png)  

![picture 3](../assets/images/de587d49c6c9184ecaa1281a14a3488dcb481e635c67439f78bce6dff8f35637.png)  

>没啥信息可言了，不过不影响继续扫目录
>

![picture 4](../assets/images/37775c4d863b51571fb2f4cd185fca74b1c4fe9c0e127ae8e67e5c7dcfdd7b17.png)  
![picture 5](../assets/images/6ed96ecc9e06c031036a547a2dce1f31afca7fedb4e669ee76e5d28d6b131a01.png)  
![picture 6](../assets/images/01d2ec628f38dccd57b7f619de51d4f9fd8c8fed5bb605313b8675d9f54def5f.png)  
![picture 7](../assets/images/6fa9c49373bb778b264533f19712e1f0366b8539c1136d379fe5edc85a275150.png)  
![picture 8](../assets/images/3214f344a866424cdad052ae32efbb009a473cefda0da07fb6488de2a6534fa5.png)  
![picture 9](../assets/images/1039893e0423f18f6ff27c14dbf87eececc319d724173eaeb5052a66ed022563.png)  
![picture 10](../assets/images/ad13bd8595fea4c17491bc40aff3054088315ec633298d825365f3b4d2ae5e54.png)  
![picture 11](../assets/images/24db8ad7b907b64de5ffafa6851c4fa88d5d7ceb916f147f7d0bcc80b69a76de.png)  
![picture 12](../assets/images/f9428eb76d2c01c073f0582a8d802da992f60c7d35516c4f3a4f7dd834cc60ed.png)  

>按理来说如果是log注入应该能看到log的活动，目前啥也没有，感觉靶机bug了，我检查一下
>

![picture 13](../assets/images/79bc13e5088f6c5e1adb100cf0a8a8c8bef7370dd7f459c9d7d6c6d120725c7d.png)  
![picture 14](../assets/images/ccf2c0df336a73fe311eb410cbdf32060f20a4494ee222b999f178babc7fc97a.png)  
![picture 15](../assets/images/5c2936e5dcdac2279ead6d6d5f6bea8d419d20aca2b608b4be526cf9b2b110f7.png)  

>奇怪了没注入成功现在日志也不动了
>

![picture 16](../assets/images/5ada6a8e1da3f64506b9162e9823c1afee5f144efe0ffdc210e4e39c8d207970.png)  

>重新安装就好了
>

![picture 17](../assets/images/de0ccd00db73fe5102815f52f9b186e06a14745cd02214cc9ccd84f3b445e016.png)  

![picture 18](../assets/images/b9144f8dd0a456d321a37ce4d8418db7a6215a3526b147bfa08cbd9f86487426.png)  

>没有nc，看看busybox了
>

![picture 19](../assets/images/ab6ce643727bb2bc0d679834012f164936b337e87a7849207cbe794abf9b0184.png)  

>好了拿到权限了
>

## 提权

![picture 20](../assets/images/5013872adc458baa405dab50d9654e813b2e6d9201df3cdd7ced7ff049c59e9c.png)  
![picture 21](../assets/images/ec3ae97ca32f7b0a0f46ea9409652d6bb6953c50f60b59c85f3ad03d12646304.png)  

>密码就是它本身找了半小时了，啥信息都没有
>

![picture 22](../assets/images/561fb282212f53b81d2f820ac415bcdc4f868f8d141311d4864fff4a6204af59.png)  
![picture 23](../assets/images/b14ef46587a4105b086dfc96ecb7f1472be8e36f2cca8a0b29d07ff7781a4b14.png)  

>只能arp欺骗了不能直接/etc/hosts更改
>

```
suraxddq@spooisong:~$ cat /var/backups/dns 
#!/bin/bash

/usr/bin/wget -O- "http://sp00is0ng.nyx/configure" | /usr/bin/sh
suraxddq@spooisong:~$ 
```

>做了一个命令的调用，这样的话欺骗arp完成命令注入到configure然后被sh调用
>

![picture 24](../assets/images/e8c0236790f47aadf1535b413e92d9126109ee00806a11db32584eaa2b23c6a3.png)  
![picture 25](../assets/images/15f88150f5233693467a62915ad80c9e033b3535dfc864f8479bf2bd86eb42e8.png)  



>这里arp欺骗要用到bettercap
>

![picture 26](../assets/images/32a175032c4965ecf61dbe3f30b5c1085695d2c1b274261e87faa87b2ed6b160.png)  

![picture 27](../assets/images/98f2e710f1642bc5ae52af740e79bf79ca69fe63f01704c1675e44252b13ae24.png)  
![picture 28](../assets/images/a886be91334a71166fabbefbc74043027a53a653f1badfcc8cde5ccdae7ba8f7.png)  
![picture 29](../assets/images/40c53f29c620621686a1538bfca083f9f698157c913be9f42b45a20bba56d583.png)  
![picture 30](../assets/images/747a3bd27b7943120d314a9fb65eadf7ddc8430e14fc03bd7ba1906e55691a31.png)  


>好了靶机就结束了，主要考察还是这个arp欺骗
>



>userflag:bca7e2be452776803ff6ff7aed76416b
>
>rootflag:3d7c0671c87e41cb601d60417992d817
>