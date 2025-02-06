---
title: hackmyvm SuidyRevenge靶机复盘
author: LingMj
data: 2025-02-06
categories: [hackmyvm]
tags: [phpfilter,c,brute]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:58:b0:b1       (Unknown)
192.168.56.137  08:00:27:26:41:19       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.915 seconds (133.68 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.137        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-06 00:24 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.137
Host is up (0.0033s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 99:04:21:6d:81:68:2e:d7:fe:5e:b2:2c:1c:a2:f5:3d (RSA)
|   256 b2:4e:c2:91:2a:ba:eb:9c:b7:26:69:08:a2:de:f2:f1 (ECDSA)
|_  256 66:4e:78:52:b1:2d:b6:9a:8b:56:2b:ca:e5:48:55:2d (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.14.2
MAC Address: 08:00:27:26:41:19 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.93 seconds
```

## 获取webshell
![图 0](../assets/images/41b515396f7d1d65b8f26988f66eba80d1a80aba4a02fe5472f8b7abeec07ff5.png)  
![图 1](../assets/images/5d5639261940b6db125e3a78824f0e61f62949c21becdb0808d5d931cc6cb84b.png)  
![图 2](../assets/images/da2eb450705d313635732f7ebf3940c93a16962dfc27fcb46b1c5f1c66cd86ca.png)  
![图 3](../assets/images/3538addf8f0da06027585be90ef64051f09e7b2e1bc66f06fec421f07c1d96e4.png)  

>提示很明显，跟干起来一样
>
![图 4](../assets/images/06cc67da1a7834526d29b931b0f042a0f76f10a02fef43a3dddbe49592ed8136.png)  

![图 5](../assets/images/43109f113bc0b27e4995b937630bce516339575d83cdbb302382fea203b5fb79.png)  

>手动尝试把，kali有webshell的文件
>
![图 6](../assets/images/d055e0c80d089d98a0fc03e09411bea8dce144200d6826c218b9a1396f45f694.png)  

![图 7](../assets/images/7661c0c4c72dd55358cf5176204674a17b6e41114632c7579927509f3fc42600.png)  

>ok找到了
>
![图 8](../assets/images/4db7a32d22206c3ca15a2ac5ddd2c4aec41f97ac5d03550c159012eae12914bb.png)  
![图 9](../assets/images/f86f1e1a8ebb8259e121e1996cdcc8b68874913095130757d73b1bfe8177bd5e.png)  
![图 10](../assets/images/d9a5bafc38ac7f30c05d78028fe1cd1666209fc8692a59b000c9b49f1f6b78e7.png)  
![图 11](../assets/images/65fdfbd417b6961b1b102f5434695020681203f84658f5b36551ab46008de68f.png)  
![图 12](../assets/images/4266a7b7b8a9ff366ad709c87ce48d01ad5717637952ced44245b4bdaca0db14.png)  

>空格过滤了
>

![图 13](../assets/images/92da66d16e8ce98953828f4ab14604d3e5bf20137a4c1b27d92c5803589e9f5a.png)  

>尝试php filter
>
![图 14](../assets/images/2857c3a55bb47dc876721623cba2139d471127b15676cd353877f8088a8ffa8d.png)  
![图 15](../assets/images/7995a7f930f4c887e71eadd5bfc117d45d41c9cf04fd2453697e4474b4323f2b.png)  
![图 16](../assets/images/323b8bcd035680a697797a656cd1b5f86cee9fa5918a7d6bf13b9e9d1a8e3773.png)  

>如果想研究一手的改源码了，不过我直接phpfilter，进行getshell了
>
![图 17](../assets/images/0b0feed71ac3bc47e5804168d157ae47cc871ab8f217fee2bcf560e2a6a8e96c.png)  
![图 18](../assets/images/6127838d8a40e884b3ead5e109c0338f62259c82b522dba1cfe0d57f3fae7ec5.png)
![图 20](../assets/images/933118c4df5a899f009129c8db5ae17d1455a94054b66dc8884ce5630c3f1c08.png)  

![图 19](../assets/images/d44b7f47d82276c4c699053f59d89e64e943a2c35ef119670f185665d42cef04.png)  
![图 21](../assets/images/579b3790b841987b794e9fb319a4e4805ccb88fce5efd1f8c1f7fce64ef0d88d.png)  


## 提权
![图 22](../assets/images/4065e5d1ffe8561279184d22fb629be413e092a4f2a829d5d957bcc8daaac33c.png)  
![图 23](../assets/images/4b7fa2cd2d42ecc79778932b634187e090b305c7fd7e6a9ecb785d7910979b71.png)  

>直接爆破，我这里直接在里面爆破，应该可以hydra
>
![图 24](../assets/images/983b81885efe967de38d2954ad01fadc1768de7c7339674f34005317220c9941.png)  
![图 25](../assets/images/e9370e398015260d0c4cea081913a3d6be1bd9f8d7c91a88bd175c2ff5c68895.png)  
![图 26](../assets/images/fbe50f4ea80653a67f424c700770d81a869b68a840ebd5cab9bb1e1ab289daaa.png)  

>当然有可能爆破用户不是mudra，是theuser，先等mudra吧
>
![图 27](../assets/images/9ec6ee4d3c89589cda1cb5c61602a8cc2480613b5400e83ac4b0d22ccf7792c9.png)  

>用户好多，看来爆破要很久，感觉不是爆破mudra了，1000多了，正常要真是爆破应该不会怎么久，换一个用户试试
>
![图 28](../assets/images/97dba15212de18df92df03eecfee14d24067cddcc818f1014aea192fc1412012.png)  

>靠爆破错了才发现，我说咋怎么就murda
>
![图 29](../assets/images/104e302052b400606a66f3389b2b72168dbc0bd40c3be6e64c093d38089a6992.png)  
![图 30](../assets/images/1988bf363c35689a6a30d55d395e89efd2b0be255cfb8e0e4893c8c3f9e9cd3e.png)  

>权限很充足
>

![图 31](../assets/images/fe13770f50747f0ac133d9a58e59837b70182d97564198578afc8c5ad1fd66fc.png)  
![图 32](../assets/images/87c7031efdd841d5cb788de6d2429e25747b74a3e989b6139dfaec22c40edd3a.png)  

>得拿用户
>


![图 33](../assets/images/0fde42289947040fbd1d74614047af2c4a9d34c065f135543f730dd1c8b0ae2b.png)  
![图 34](../assets/images/2f66d6460136e91dcdef36aef884f83f8f1d1767098a2e9511125f333c099c7d.png)  
![图 38](../assets/images/30667dc35360dc2e6cd12b0098d8396bf2e5897971e197f18f82b38311654876.png)  

>爆破无果，利用一下上面的提示
>

![图 35](../assets/images/273b4591360bc2adf59f95b80009ea010b3c343dbebf9c17fa1a290c02abd3b4.png)  

![图 36](../assets/images/ca19a3fdfed0c802b67f334c225c76862a21892f1cf017d0c953823c63644d23.png)  

![图 37](../assets/images/9231f6574107c87fc89d04da42fb90eaaf7dc7a521ca8ba689a116d5b46737c3.png)  

>他能改不知道能不能直接王炸方案
>

![图 39](../assets/images/51816a5bf4b7b4afa5d759d093416d6774b46b06339b3f56e199b5e1e309b8ad.png)  

>差点想改文件
>

![图 40](../assets/images/1aafb5784dc1b8c6903e86627d27eb96d65d730447f90b0ae99357d0a85cc48f.png)  

>还是想改文件，他是一个二进制的文件拥有suid权限
>

![图 41](../assets/images/579bf065d6844bdd9c6f9ea2ec95a059a9e4868de5915b8d1724df311a685d46.png)  
![图 42](../assets/images/d3ac967aae0b508abfb5722b72881ef14dfdb3b42d06b0d97ebccf2380807c40.png)  
 

![图 44](../assets/images/b3e3c72f7c9f4cf95007195dba155de556dfb317ecb58785ed9b6ef3bf411cad.png)  







>userflag:HMVbisoususeryay
>
>rootflag:HMVvoilarootlala 
>