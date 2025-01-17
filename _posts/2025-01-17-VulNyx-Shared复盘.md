---
title: VulNyx Shared靶机复盘
author: LingMj
data: 2025-01-17
categories: [VulNyx]
tags: [nfs,wordpress,LFI,no_root_squash]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l 
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.168  00:0c:29:7a:e6:2f       VMware, Inc.
192.168.26.254  00:50:56:e2:a3:32       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.577 seconds (99.34 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.168       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-17 08:03 EST
Nmap scan report for 192.168.26.168 (192.168.26.168)
Host is up (0.0034s latency).
Not shown: 65526 closed tcp ports (reset)
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 9d:c2:e5:b9:bc:86:d4:81:5e:ad:aa:8d:87:a8:ad:5b (ECDSA)
|_  256 6a:d1:8a:c1:4d:f9:0c:4f:c5:f6:21:bb:c9:a6:24:53 (ED25519)
80/tcp    open  http     Apache httpd 2.4.57 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.57 (Debian)
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      39495/tcp   mountd
|   100005  1,2,3      45281/tcp6  mountd
|   100005  1,2,3      51436/udp6  mountd
|   100005  1,2,3      52559/udp   mountd
|   100021  1,3,4      34641/tcp6  nlockmgr
|   100021  1,3,4      43706/udp6  nlockmgr
|   100021  1,3,4      44517/tcp   nlockmgr
|   100021  1,3,4      51347/udp   nlockmgr
|   100024  1          38669/tcp   status
|   100024  1          40579/tcp6  status
|   100024  1          46810/udp   status
|   100024  1          51507/udp6  status
|   100227  3           2049/tcp   nfs_acl
|_  100227  3           2049/tcp6  nfs_acl
2049/tcp  open  nfs_acl  3 (RPC #100227)
38669/tcp open  status   1 (RPC #100024)
39495/tcp open  mountd   1-3 (RPC #100005)
41017/tcp open  mountd   1-3 (RPC #100005)
44517/tcp open  nlockmgr 1-4 (RPC #100021)
49081/tcp open  mountd   1-3 (RPC #100005)
MAC Address: 00:0C:29:7A:E6:2F (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 72.19 seconds
```

## 获取webshell

![图 0](../assets/images/e0a8d54d0ac6b9cc958b6bebb70073365de3bb79f8d00f3e70db9e413422367c.png)  
![图 1](../assets/images/6dde8f872d590f0174c99635be8af3f01fb3e29bd7181ce520d391027e3b2c0d.png)  

>这里需要进行的操作是获取挂载的信息，上面有路径可以将路径文件获取本地，注意点是用户可能需要更改
>
![图 2](../assets/images/703958347e31346cb281fe353a7bc4e5bff1ad6934e6a4688f4cdbe0c20052bb.png)  

>没成功可能是我的方式错了
>
![图 3](../assets/images/ad76aa84e9d54c7a67fbb802776087b03b4ffeb5e2b23a92095ba1c9bfc4f722.png)  
![图 4](../assets/images/7d0808ae39b098840463a1b46211dedac2d219048a3463cb688666fd64203862.png)  
![图 5](../assets/images/0b9d11da8c8d521d0d856dfde512eb65d765fbae10b6e7a51f44d96871a6e738.png)  
![图 6](../assets/images/12af27a2155aecc495b6a844b46610f6d603c41d059c0be610f0b3bd6ffa8b55.png)  

>这里思路断了，需要去扫描web端口看看有什么线索
>
![图 7](../assets/images/fa6e9de1fefb7ec71f11a4908e5cb94fbb36e4e148a7318ade369c9fd7c2737c.png)  

>新线索wordpress的漏洞
>
![图 8](../assets/images/336cd5cd69c315a273aeaa6053cba38e8d79b36abae712cbbae083b644602cda.png)  

>存在域名，查看一下插件漏洞
>
![图 9](../assets/images/3063650a205f4257ad2527cd3eb8ea780966ac836d1a66ad956c0128d2b7fd4f.png)  
![图 10](../assets/images/b077541b730bf610b0ebf2ddc44064dfd6bacc6d00dd45134bc60c61fafbe534.png)  
![图 11](../assets/images/e5a0735003f4fbe8baa1bd091b7190d060e5a58e3890d6900f79740b782a9bae.png)  

>这里很多插件漏洞需要一个一个试，不过可以尝试爆破密码再试插件漏洞
>
![图 12](../assets/images/29ae5e9c8985b5d07d651bd18f2ca86d7413ce2d2d71c6c5fbc52d94f47028e4.png)  

>存在LFI,觉得可以尝试php-filter，但是不存在，并且不能直接读取wp-config.php
>
![图 13](../assets/images/5a7d18b975719c8c04a29f88b3e8c47c55140fe1d8eae3cb6facd694200b4caf.png)  
![图 14](../assets/images/335ebd6db997a6c12e34ed39b65da6f8ef76fc96ccfaaaf7e340515f119d5da4.png)  

>目前想法爆破ssh，因为知道用户名，或者继续进行插件利用
>
>整了半天发现还有LFI没处理完

![图 15](../assets/images/823e000cac3e3ef1c5d79c801ba8d7d1981433f688d1034b40ab1fa3c8e61110.png)  
![图 16](../assets/images/0050e06908917bac3f3e289a08011a474a248a81566d6801fdb4c45fa94c3963.png)  

>存在日志读取，利用一下日志注入看看
>
![图 17](../assets/images/8b68df8141a424b17b873b9bd5538587fb7810cdaffbaa669da6705a1e013f06.png)  

>成功操作,可以获取webshell

![图 18](../assets/images/3488bbf4d50318ff1eb22c822f977f3d39f1b550c1671eb444248b417e02d3a5.png)  
![图 19](../assets/images/d79bb91b1a10d00bc897a5539e4caf18d177e6e500866b6b9c76b63ea0aa168e.png)  

## USER 提权
![图 20](../assets/images/37684d285462eaf8ab7e7b20b3ab96bc76b131f6ac64c477e41c539f423bcf99.png)  
![图 21](../assets/images/980cfe508ec0b29afb4b93becff32b6c38d7918ee0cbff93a45abee644342588.png)  
![图 22](../assets/images/ef9176fdca6f3df4fedf15ea89d6f352c64404cb68928d3a2abb927ce213b35e.png)  

>mysql 无果，可以尝试用上面密码登录用户
>
![图 23](../assets/images/8ec7ef1910dc6305bee99d9c47903cc555dd8deec418aa026694b92e14ca8d4a.png)  

>还是无果
>
![图 24](../assets/images/4b4206e505c767d470386dd4d10c1adbb9a26b2fa22c73149480264c5a0bec35.png)  

>没有想法选择利用工具查看信息
>
![图 25](../assets/images/315dcd60ca8877789a60b2e90d545416d338e666b57de2f424f23e78ab488704.png)  
>找到一个zip
>
![图 26](../assets/images/56354d33c3cfde6e879f6334ef3746367a3cf1986608c149dd86699cad80a035.png)  
>看来是找keepass的密码，这里需要利用一个工具是keepwn
>
![图 27](../assets/images/4d844772c3b80696eb265fb0dedf18d262adeefc19c374f2722b59864fbaeca0.png)  
![图 28](../assets/images/fffbf7bfef3bf06e53f1e6f34ed35a8275c69f4150567d5706491117affe1163.png)  

>到这里就需要等着了
>
![图 29](../assets/images/83d3878f67953123c622f6d2edda6245d50b6cffc907030e03972ed11dd2b6ad.png)  
![图 30](../assets/images/62f069641c9f9b0f771d61b77f1b7773e528dfcc71c49f774f2dbd4064ccc873.png)  

>这里有账号和密码
>
![图 31](../assets/images/d66f9c0dd34be41a97f069c43ec2ab99b1c09c855d7a843eb2aa36494a4ca046.png)  
>尝试一下发现只有一个用户可以登录账号密码
>
## ROOT 提权

![图 32](../assets/images/43b5a093c52b02ecb037b9f541495c0ccd78ccecb3a9b61c0b2e9563b37fae99.png)  
![图 33](../assets/images/f360baae4ade7c34b258dd93269214d5b911d2f45581be0cc6af99efa24ff436.png)  
![图 34](../assets/images/17513ba2027f4db6776bdeeb510ce8d2ba5c2741e633f2019434f95389962b76.png)  
![图 35](../assets/images/92f005bd9e2a61446bb74fdeba1c527131b6a1fe02351f4fd771c910096d7692.png)  

>得深刻理解一下这句话的含有
>
![图 36](../assets/images/f1e51b699308cd85f4d12dca401583bb5ef8b5a9129663e2a4f81481d92478e3.png)  

>有点想法了不过要尝试一下，首先必须回主机因为它为root，我现在需要做到的是把主机/tmp上的文件传过去，因为他是一个挂载的形式
>
![图 37](../assets/images/c96ac4a730f66a651563dffba7507397345f878698c281ed27756be03caf0a21.png)  
![图 38](../assets/images/5760c110c62d75469af398f2624e72e4e56808d9404b74be9595292d970fbe73.png)  
![图 39](../assets/images/b8fde891cf598c0149a472e682630dc6184d92b4c56bedb68e68b1270dbcd96a.png)  

![图 40](../assets/images/caee706359b9004d828ffed91d96dd9ed477105ee19f4051a8e47d606ea7a9fc.png)  

>奥，懂了现在是一起开着的状态同时同步
>
![图 41](../assets/images/d05e719717f382863d74860a6bfae68d2c2553d3e03399b70fda31bd735a553d.png)  
>有一个问题是他这个不行
>
![图 42](../assets/images/d0794d8fd9f07523049ac7daa6e157415dc6634de196135c74cb7e5114c23e7a.png)  
>看一下有python,可以利用一下
![图 43](../assets/images/39154f524970ffaaae4c16c3894cb3c6d9bf1531af66512f77d264dfb4482e57.png)  
>没有权限,继续想一下别的方案比如直接更改文件
>
![图 44](../assets/images/36f5a0b46c5f54d74868bed684096df8b310315c1fdc34b262830c0c07716eaf.png)  
![图 45](../assets/images/33ba731b2dcd4c13e119de46539b441677c71866de30c29f97a98fb84487e702.png)  
>方向是对的，但是没获取到root权限
>
![图 46](../assets/images/2af3200e4d07f74a2bd2f1ba9bf54b95d7c4de0e9e252cb676a9fbba91325d9d.png)  

![图 47](../assets/images/5896e15bc77307354df8fa9c8f1139775828915349ade0007cd754c72266a704.png)  
![图 48](../assets/images/bf60d11da319f66141175c5651a0af9cc5df2134cf32b0057fe6139074b6e962.png)  
![图 49](../assets/images/6c6645336806ac8c408d64cdc5081aaabc0adf76a9ea4107614856b1bc5edb63.png)  
![图 50](../assets/images/077593049dbfdb212ac36863d0cca979858c52085e06bdefd63e3c790fe81e89.png)  

>操作有点多余,但至少解决了主机bash和靶机bash版本匹配问题,这里如果有大佬有更加简单的方案可以留言在b站给我
>
>好了到这里靶场就结束了

>userflag:760ad04a056fb67653ffc01eda470e45
>
>
>rootflag:8e03a0a039069f840196498da1750f1e




































