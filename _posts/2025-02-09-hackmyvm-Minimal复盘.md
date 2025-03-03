---
title: hackmyvm Minimal靶机复盘
author: LingMj
data: 2025-02-09
categories: [hackmyvm]
tags: [pwn,phpfilter]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:28:57:32       (Unknown)
192.168.56.148  08:00:27:64:92:99       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.947 seconds (131.48 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.148        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-08 23:55 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.148
Host is up (0.00085s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 d2:73:06:e2:e4:84:54:8c:42:0f:4e:81:7c:78:b9:c2 (ECDSA)
|_  256 75:a0:cf:35:61:a1:c8:77:cf:1a:cb:bc:6d:5b:49:75 (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Minimal Shop
|_http-server-header: Apache/2.4.52 (Ubuntu)
MAC Address: 08:00:27:64:92:99 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.76 seconds
```

## 获取webshell
![图 0](../assets/images/233e9f371f6c54f3d97020cb1b0bd2eb502bfe982618256b433b8c21c8b9e94c.png)  
![图 1](../assets/images/4dbdb6f8123d4e416ed764c68e4b8cc1509f975914798150f5f038d7719abcc3.png)  
![图 2](../assets/images/fdcff941cc19228a2ab20208251ce3fb0f2156b60e0f4dc3db9f8c6aabadc434.png)  
![图 3](../assets/images/8225e1e120934a3ccf7db958b2a2543989b96647fff94b61044b9be520fdd355.png)  
![图 4](../assets/images/d9652e55e2bac6b22998d3b56a33736a25f94892b1d1233e30ba47248d8348cc.png)  

>可以测试sql
>

![图 5](../assets/images/88bc986c0d9b95e4caad09c8087090e55e9f605b25ce5bc81d9df7240d6f4dfa.png)  
![图 6](../assets/images/17a81ca7e6cf68398325bf48a42021916e9dabe1578d3b066c3d7db509a96260.png)  

>爆破把，没有mysql
>
![图 7](../assets/images/33be0d6e51f74ad90caddcf1201dd2192204d834fc2814eb32229f89ab281746.png)  

>无，看看目录
>

![图 8](../assets/images/dc9ea7ea007b1514469f7ff8713195c0978dd979a399b4cecbfbcfa6504d3f55.png)  
![图 9](../assets/images/a27546a14b552b8834633901e83572b99818d09e07483c3a955c664d339f0a34.png)  
![图 10](../assets/images/adfd8ea8f3e0d0fe6eea8c8d309cded0771fb029d6899dbed239588376b5c203.png)  

>注册
>

![图 11](../assets/images/18d1d90782a53cb82c67a981b7a6cdcd826a952634591037bc1d993a9303b1b5.png)  
![图 12](../assets/images/8ddbb4b5394e3047e3c2545002acb0f07d38ba46d20783186bd4322a20142cca.png)  
![图 13](../assets/images/1c723a85823b982a65fb5d107974025a8402470dc5a22a608cc17eab53d924d5.png)  

>还没跑出来
>

![图 14](../assets/images/b9ecad376974da7fa60c831c201aeb9213db98f2a32749821fe49240d1f23c91.png)  

>我怀疑是fuzz的问题
>

![图 15](../assets/images/773336d42bd6735b15e349c547bd9b7bf8f3069f2f62f907d8d2723223c78591.png)  

>单纯没有，看看phpfilter
>
![图 16](../assets/images/a19bfcaa5d905236b4b625dccdca5ba58f2fe3854e20cae1118b134657478f69.png)
![图 18](../assets/images/317b38e867b6e0d8bfe02275ea19c61e8511359266afdb290c5841ccec427e3b.png)  

![图 17](../assets/images/68791e3bb67cf52ba561a05eb5e0051e26dafe58335a4090419cc9a306011c72.png)  
![图 19](../assets/images/db548fbb45957158e0fd4958ccd4f8756709914ccb60e90e923038cf7e8ecc90.png)  

>确实有奥
>
![图 21](../assets/images/3e25af66a3fe10f8bd28dc590c50edd6c175db8bae808590cc890b5dd06a677d.png)  

![图 20](../assets/images/5f0adafacc765c2699b1dbd85344a654b591b8e9cc8028fa453fc9e5e1b3e2ac.png)  

![图 22](../assets/images/895b149e305f5a95f1c0876abf7b614b199d9ae26a9eecb808cabf2426687100.png)  

>还是busybox
>


## 提权
![图 23](../assets/images/d777aaa284816cc36b2a8b4453dcb131fb174e6fa260a1b03e24f3331d05b137.png)  
![图 24](../assets/images/426235b58183d19818d8e4929f0c35a82d2a61a0365a5f8d2f80700924c9385d.png)  

![图 25](../assets/images/d848277a4eecd26dacc76e9180509e7f4ef63077a6a44df2e0028f26b481f6b5.png)  
![图 26](../assets/images/2bce9434e9e13d69f707b4772781c4e9c72c5bb38ea5f9c19f0a58aaeadcea61.png)  
![图 27](../assets/images/0fcf2917848ff603fa8a2d993ef05c7fd3df710e9c40dd305abb49a8a52d2cb1.png)  
![图 28](../assets/images/39ccbf795c4b0e1b9398e28bf04233f1512768b46042bb951336d8039435591a.png)  

>程序的话进行反编译把
>

![图 29](../assets/images/92d595032d8686304ab238a31021edce4699f9dd81d256c261801d39b43b1600.png)  

>纯翻译的话就知道第一个是linux
>
![图 30](../assets/images/dcd95117ae3928cd48eb4dc43e7b82fcf38742dcae476167b991fc4368179b7e.png)  
![图 31](../assets/images/88855963b3a49dc0b800706e33baac9ccb14c23aa0b29dc4dc2b0374d8d1d39c.png)  

>培根啥的？
>
![图 32](../assets/images/9358833f93a5da42bdcbf7961fe8e7b4dd7efd70da5f87e7bebfec7cf1fb2d3b.png)  

>合理，因为就他是英文
>
![图 33](../assets/images/294a0f79b66c6d1a69291f2f10b68bf66d396287c339aa766506a9010147eafc.png)  

>第三个不好找
>

![图 34](../assets/images/a2e8a9bcd26cd37cd02e617e303c3c7b994746e8acc735e51e670d190eabeb36.png)  
![图 35](../assets/images/47c8e420058785ca448dfd5795d134609a27b3518973ffdefe932ae6dd4a337e.png)  

>这里说名一下他是跟目录，所以可以进行软连接
>

![图 36](../assets/images/b2b5db79049e323ced2dea67018c4bccd924ad8b0acc6eb2e255100284298bad.png)  

>没成功，为啥呢
>
![图 37](../assets/images/fb5805f66ea2abaa44157847721b4a47d6d178750c2209f028b62560b0bac2e4.png)  
![图 38](../assets/images/459fb34aecf85587fda3d261f191e800b694f539550a9272db24558b8e156a3d.png)  
![图 39](../assets/images/d4700745b9a3087f25ccd8619314850d4e9a92bd12a90a1bf365bc470ca83e57.png)  
![图 40](../assets/images/9754e7b71c0ec5581fce1d34758171db53c4c18168410374fe264696541ca22b.png)  
![图 41](../assets/images/7b98107bfbc5f8f93588398dca8f3b59054f29ed39c8b641cec660a84e89dee3.png)  

>感觉做溢出操作，但是我好像不会
>

![图 42](../assets/images/a9ee8204e520e737c6c5e1ced4e42622838af3d128d30928fe84a5d5a3a9f1ec.png)  

>果然又是权限问题，虽然提示很明显但是tmp为啥不行呢有点懵逼
>

![图 43](../assets/images/a9218768be737e5463601555b110b5f9d0c4984d224f28563e45f1eea2420c8e.png)  

>唯一的区别是所属目录权限组问题了
>

![图 44](../assets/images/4820a17ee571913482fb791a18f0d85cfa22cd259ccc66efdc61ce041a9568bb.png)  

>没有无法获取shell了就这样把，id_rsa也没有
>



>userflag:HMV{can_you_find_the_teddy_bear?}
>
>rootflag:HMV{never_gonna_ROP_you_down}
>
