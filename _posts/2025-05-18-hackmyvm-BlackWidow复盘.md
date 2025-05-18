---
title: hackmyvm BlackWidow靶机复盘
author: LingMj
data: 2025-05-18
categories: [hackmyvm]
tags: [access.log]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/tools# arp-scan -l           
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.24	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.076 seconds (123.31 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sV -sC 192.168.137.24 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-18 04:34 EDT
Nmap scan report for blackwidow.mshome.net (192.168.137.24)
Host is up (0.0078s latency).
Not shown: 65526 closed tcp ports (reset)
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 f8:3b:7c:ca:c2:f6:5a:a6:0e:3f:f9:cf:1b:a9:dd:1e (RSA)
|   256 04:31:5a:34:d4:9b:14:71:a0:0f:22:78:2d:f3:b6:f6 (ECDSA)
|_  256 4e:42:8e:69:b7:90:e8:27:68:df:68:8a:83:a7:87:9c (ED25519)
80/tcp    open  http       Apache httpd 2.4.38 ((Debian))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.38 (Debian)
111/tcp   open  rpcbind    2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      50051/tcp6  mountd
|   100005  1,2,3      50587/udp   mountd
|   100005  1,2,3      51380/udp6  mountd
|   100005  1,2,3      57111/tcp   mountd
|   100021  1,3,4      38588/udp   nlockmgr
|   100021  1,3,4      43487/tcp   nlockmgr
|   100021  1,3,4      43577/tcp6  nlockmgr
|   100021  1,3,4      52683/udp6  nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs        3-4 (RPC #100003)
3128/tcp  open  http-proxy Squid http proxy 4.6
|_http-server-header: squid/4.6
|_http-title: ERROR: The requested URL could not be retrieved
38975/tcp open  mountd     1-3 (RPC #100005)
43487/tcp open  nlockmgr   1-4 (RPC #100021)
52601/tcp open  mountd     1-3 (RPC #100005)
57111/tcp open  mountd     1-3 (RPC #100005)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.47 seconds
```

## 获取webshell

![picture 0](../assets/images/d4883c4977e7bc08321ecb76df924e702f37893ca2b7e8d05dcafe0329759195.png)  
![picture 1](../assets/images/cb906d2282f4b1ae1537f78667d5d919ed9ec19e8a456ea9e81b284acfa3e6cc.png)  
![picture 2](../assets/images/4ec8d2001c21655f419e98b74ca47ac8a50445a8fac1c2ad472b51b140204968.png)  
![picture 3](../assets/images/1a51335d0809837e2af66fbbe3ff05ab99944009fca987e38b672f4283d64d33.png)  
![picture 4](../assets/images/ffdd7662fce4a5c28e28aea3928e1f7ddf4024f907cca8b6d3b6840e592a871a.png)  
![picture 5](../assets/images/bbd78884bf2f8b32bf669cdd5752a42d1fadf9fa1ffd767b26f603a8f531fc3a.png)  
![picture 6](../assets/images/e15e13f3a4d40ef5c1cea018da5105d184cadab6724449e0fbf11f5d0c1871aa.png)  
![picture 7](../assets/images/0eebed8be5e6bdf74404c12d4f759f5c4725f4ed1dfa5ca9458d736909bd347e.png)  
![picture 8](../assets/images/b9eed64acad11f92169e301db65e5d09f01e03f878970090c61017d5521b3e47.png)  

>没看懂
>

![picture 9](../assets/images/71359eda97a863bf582ccf5429d8b62e341b15bf3192c1038b2ee4220bc709e8.png)  
![picture 10](../assets/images/3ce7b2e861820735062e600852b498951a05304232cbfc962ace03dda474046d.png)  
![picture 11](../assets/images/f89b4cc45b7d3e8da7fae2d24f050c1644e5178b0d14f3853635a3618b7967e0.png)  
![picture 12](../assets/images/54dbbab663239c429d0fd280e889a49ea03f01f5836fbce82e03284db8e91111.png)  
![picture 13](../assets/images/0f08ec2b01b592aa1111473d3d2d67960d3ae090d618acfc9779e54a715e41eb.png)  
![picture 14](../assets/images/27be0c821428f74c4f9de984798f8bca61dc49098171e1fc2b575468a394e69a.png)  
![picture 15](../assets/images/6bb3a993bbb142494361a72950123f83d71b9ec24e616bbfa534c5529ba24020.png)  
![picture 16](../assets/images/728014cbd6d5b56779fd9e8dc12daa5a7435ec98b9a2733fcf343f7440a9804c.png)  
![picture 17](../assets/images/8b1fb122d9704c985bcc61368e3767e3a58e48201a1ce5c46063125243f80b41.png)  
![picture 18](../assets/images/afb09fe69e891b0945588a6da3e3153b49baf01f4ee3c944ef83d460d7a5f7db.png)  
![picture 19](../assets/images/6d7f384a8aceca93dcc1e278cfb09447cd801d7d73d1dae884fc02bb2ea5bc46.png)  

>需要怎么多个穿越真无语了
>

![picture 20](../assets/images/9446978a7801b4f48ce2059f3fd28b07c20209b2716ebc6afe1163d551c92a3f.png)  

>爆破不了老多错误了，这可以猜测几个有用的目录比如用户目录，日志目录或php filter
>

![picture 21](../assets/images/8e74218672bc99ef4a081427fda520a3bf3409cb7abf3a8a6a05316c53f9ba87.png)  
![picture 22](../assets/images/92e8972d21210af13fe44098e0eb6e93a4e4521b36a0ea5a047dabecc2a8c8ae.png)  
![picture 23](../assets/images/12847ca34ed8e0148963be16c5643f8989aac0db550b730b5f011b52a7c3cc77.png)  

>因为报错感觉是error.log而且感觉log对我的靶机老问题
>

![picture 24](../assets/images/9fd40a287d262cc3a5d339da0ec0c25a4df5d0529f84bb9656b4087882eea346.png)  

>看了一下wp确实是log但是有一个问题就是我这边log不启动
>

>重装靶机了如果不行就搁置了
>

![picture 25](../assets/images/df4b65ad2b124f757b4ec0459a711654ff19ba3268b3d97d4dd609bb5f93d11a.png)  
![picture 26](../assets/images/a8c024c0be467e5c01d0ffd6962f232dbe2646843bb614ac808447324d69948f.png)  

>又挂了什么问题
>

![picture 28](../assets/images/f6aaef6bd7ffe28fd1cb7f19a494855b82825e81b9e95e6de5da183093fea6d8.png)  

![picture 27](../assets/images/b617d143720d88e76c4e9358d28b3f539ba23b884b93bea27d9955c5eadf87d8.png)  
![picture 29](../assets/images/30e6aaa41383df09a7d03eb65dd7e4495ed2de99299fc4fc24b4c53b7ef5d19b.png)  
![picture 30](../assets/images/ee58edf5847460062d641f2fbe09170e47f5bf32d06757240591b622b97536e0.png)  

>好了拿到shell了真麻烦
>

## 提权

![picture 31](../assets/images/6669fd9ed1e5916a396cc853d855eeff28c094f09ba7f4dd6a2ee8a6ebe44bbb.png)  
![picture 32](../assets/images/246faf59e78646725d1d5ee0d5513db0d1ab205d58e135124b8e0e78afdd3435.png)  

>这个是提权方案
>

![picture 33](../assets/images/44ba76d35c9ec2c435bedc7efa8208d86916b20207899b05eb32a3d5f4781a60.png)  
![picture 34](../assets/images/16f939961efc09e39f849edf968b6d8eb2297098b948d7b8c9f13f0a32cf5b34.png)  

>密码不是
>

![picture 35](../assets/images/6c2e0d3e2b80f6081990f82142ddebc1bbe8a31948dc3bea3ffecc9284a2a30d.png)  

>那里都不是
>

![picture 36](../assets/images/4cd787a1a678264840315d52677c4df743880747b73b7d553aec943f3dc11996.png)  

![picture 37](../assets/images/4fdb0929cd12bc00a703dc0ee908e7dc9073caca9e6f9f3f87e1a46fe4afcbae.png)  
![picture 38](../assets/images/5cc5430c7659c09e632e93fcd2d3436a9ad56b7b0b911521d5e6e58908411068.png)  

>这也不是啊
>

![picture 39](../assets/images/e3f83bb45079b97dab7cde94f008ddf9ab0631880b5d41763297855b9d5cb4a5.png)  
![picture 40](../assets/images/a2c43828b2b0d4ca6962e936f2b5294feddd48d192621d1a1ff43e5b6dba99b8.png)  

>看了wp确实在这个文件，不是这看的我眼睛疼主要过滤不出来
>

![picture 41](../assets/images/03e83970f54fef01fc08afc3a02af4b316a59d330bacdd176dc09f57e12daaa6.png)  
![picture 42](../assets/images/2d72a6df8d4205c1b11e1bba59ea19449eb941fc095f5afc291fd66ecef89aa8.png)  
![picture 43](../assets/images/f3b154e617ad39a29fa2809d4e05e1e4147c0ffc15f0e8861b96aee697b6b065.png)  

>这是perl的方案吧
>

![picture 44](../assets/images/5ca27e226e434196d54135fb3aa22eba368971f0298a69e4053760a9f05072d3.png)  

>整体难度不难就是看花眼了
>


>userflag:d930fe79919376e6d08972dae222526b
>
>rootflag:0780eb289a44ba17ea499ffa6322b335
>
