---
title: VulNyx Jerry靶机复盘
author: LingMj
data: 2025-01-19
categories: [VulNyx]
tags: [upload,xxe,svg,escape,date]
description: 难度-Hard
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.179  00:0c:29:2d:49:a1       VMware, Inc.
192.168.26.254  00:50:56:ff:4b:3d       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.390 seconds (107.11 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.179       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 07:41 EST
Nmap scan report for 192.168.26.179 (192.168.26.179)
Host is up (0.00088s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 65:bb:ae:ef:71:d4:b5:c5:8f:e7:ee:dc:0b:27:46:c2 (ECDSA)
|_  256 ea:c8:da:c8:92:71:d8:8e:08:47:c0:66:e0:57:46:49 (ED25519)
25/tcp open  smtp    Postfix smtpd
| ssl-cert: Subject: commonName=jerry/organizationName=vulnyx.com/stateOrProvinceName=Spain/countryName=EU
| Not valid before: 2024-03-08T19:46:55
|_Not valid after:  2025-03-08T19:46:55
|_smtp-commands: vulnyx.com, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
|_ssl-date: TLS randomness does not represent time
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-server-header: Apache/2.4.57 (Debian)
|_http-title: jerry.nyx
MAC Address: 00:0C:29:2D:49:A1 (VMware)
Service Info: Host:  vulnyx.com; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.16 seconds
```

## 获取Webshell

>出现一个25端口可以进行操作一下，记得25好像是邮箱的
>
![图 0](../assets/images/3f48b2a6b6b7f4d77fec35f4a6f91e8504863dd4428522750e0f0b07e7a3f933.png)  
![图 1](../assets/images/925005d28efb227d753d5f5c24bd8b791e004500a1579af07c77c0a96cf2160e.png)  

>好尝试上面的命令看看有什么效果
>
![图 2](../assets/images/3dd5f71989158cf9c8cfd4cbd88e43b579519f249d89c158ec2d29eae089b9f3.png)  
![图 3](../assets/images/d5abd67ef8d575f50fcf724d7c10f08cbfcfc0d27ed956cbe5e565982a340440.png)  

>无想法，先看web吧
>
![图 4](../assets/images/e36e07bd11a92a081128bced166ba9f77d3f6d8c170479953e34a33a0f4de719.png)  

>存在域名
>
![图 5](../assets/images/4c38538c091a22dd6beef92ff0ec5f5b6cfa3a33326c3bb66363cf1975f9b5c4.png)  

>发现关键信息
>
![图 6](../assets/images/bbe153b8b27f63a064fefe97e921dd8f3e5936fce0e810901c54bbc3ef85d5d4.png)  
![图 7](../assets/images/2fb22fe8d2ea4fd76a025e23fe54ca76774af2dcdbae74bf7759674358a9daf9.png)  
![图 8](../assets/images/45bb900c22fe242e6ec402f69fcef752597523d4981db2e085fe52bab94888a4.png)  
![图 9](../assets/images/96b31246374d06fd31f23e9c44e7b2f96bd920530ab42f4b5bbe43c059187140.png)  
![图 10](../assets/images/44ccefe5f167b9aba0c20af3a21a1e87bf3eb31725ad3bb330aac26d4251e46e.png)  
![图 11](../assets/images/20888f8b9e94f26d56ba9840ee7e9263d2547650453ee02484e5634932fcb253.png)  

>目前发现肯定是注入进去了但不知道去那了
>
![图 12](../assets/images/345a2f1aad66cafc8ee30457f300f67b338ab790f8cfe9aecf605a7805f96986.png)  

>目前想法应该是在25端口的email服务看，整了半天无果去看wp获取一下路径的思路，描述是svg的xxe
>
![图 13](../assets/images/e161589a512b06c686aa78e9d230fe887c27fe994ad2184dfb49ab782f8bc93b.png)  
![图 14](../assets/images/ba4e39df731dfb96cb1c9a3ec4c736d236258afe107c24e2fac6263f87fbea12.png)  

>接下来就可以按照自己的思路走了，既然我们可以读文件，必须看看这个upload.php，把文件上传到那里了
>
![图 15](../assets/images/a51ffcea862c362edd68d9e138f4aba916e5d828a90c6b583e65a7acee6c67d1.png)  
![图 16](../assets/images/85df5dc710a6d1cf5f6a55e1f9b01653923a1019a146b30db2088f696ca1150c.png)  
![图 17](../assets/images/279cfc548e990146f10eef0a3b7994292a7abc3fbf8a73802b9377620d7ca027.png)
![图 20](../assets/images/01f96b6f17c2ff9bfe6ad4e55ff1ac27456d1c799eb81973216281613b77b9d1.png)  
![图 18](../assets/images/414810d6e06d646d4025235d1feca819507542263b5fb8740b7e02f7e6dd80d3.png)  
![图 19](../assets/images/07fb70c967cda00973144b4b62efdfac698cf5a5c77e821ec1fb78cca5ca509d.png)  
![图 21](../assets/images/07e16c13918785de923afe4ce195bded726f12ca19aac5d70b3dd8997ebd6cc7.png)  
![图 22](../assets/images/0cb4b563fdee169ecb107e579a988d5c5b543af245e61ccc1d9def8d2948574f.png)  

>试了半天发现是filename错误日期加_上传name
>
![图 23](../assets/images/b6705a90d48cacb27bc1e8479c6e37327016938f1bb9eea62694575c8b2fafc3.png)  
![图 24](../assets/images/49e7c30dd408c782c04d8bbdb5a33376de0faafb4f39ae6d3fae44c6a21d205d.png)  

![图 25](../assets/images/043f5684133cddd38ff5b39d1ce7380a8ced20e6dfe6dcaf93de3cca23d572f7.png)  
![图 26](../assets/images/3aa4b770e3801b70c32737f36cad913074d8111e2f5e8e9fd525e66b5bc38c43.png)  

>
>只要后两位，真是试了半天因为是小写的y

![图 27](../assets/images/ebd93005e4b71fee9567d0bb02b3b5577f94e71009114ce399b45714e135f3b6.png)  

>全是busybox
>

## 提权

```
www-data@jerry:/var/www/jerry/request$ sudo -l
[sudo] password for www-data: 
sudo: a password is required
www-data@jerry:/var/www/jerry/request$ 
```
```
www-data@jerry:/var/www/jerry$ cd 
bash: cd: HOME not set
www-data@jerry:/var/www/jerry$ cd /home/
www-data@jerry:/home$ ls
elaine  jerry  kramer
www-data@jerry:/home$ ls -al
total 20
drwxr-xr-x  5 root   root   4096 Mar  8  2024 .
drwxr-xr-x 18 root   root   4096 Mar  8  2024 ..
drwx------  3 elaine elaine 4096 Mar  8  2024 elaine
drwx------  2 jerry  jerry  4096 Mar  8  2024 jerry
drwx------  2 kramer kramer 4096 Mar  8  2024 kramer
www-data@jerry:/home$ cd elaine/
bash: cd: elaine/: Permission denied
www-data@jerry:/home$ cd jerry/
bash: cd: jerry/: Permission denied
www-data@jerry:/home$ cd kramer/
bash: cd: kramer/: Permission denied
www-data@jerry:/home$ 
```

![图 28](../assets/images/452e82dac24ac1eb644770f002afacddcec350f49ce656a5bd2a79f11bd35cff.png)  
![图 29](../assets/images/471c8da1281c40c33f898b5e6c2646a8a03bf40c925b8bf505ecbe64b1f7e64c.png)  
![图 30](../assets/images/1274269e70d41265070203049224ca60307813d1eae421e603e62c6508740071.png)
![图 32](../assets/images/7a04f7badc1f423397da96ed436d4c9c5ab9a517d91d4566540560d54cbca523.png)  
![图 31](../assets/images/edc71d2d0b1398a9543928982e261bf5cbbf4ca85b3d05fd0a470dc26fbbcb18.png)  

![图 33](../assets/images/f3d6577453eb7c7f6c64e1101b744a3dc64b37e4c0e2d42c914e0855726340de.png)  

```
└─# cat elaine    
From elaine@jerry  Fri Mar  8 10:03:40 2024
Return-Path: <elaine@jerry>
X-Original-To: elaine@vulnyx.com
Delivered-To: elaine@vulnyx.com
Received: by vulnyx.com (Postfix, from userid 1004)
        id 47219A0346; Fri,  8 Mar 2024 10:03:40 -0600 (CST)
Subject: Kramer & Newman Clash at New Years
To: <elaine@vulnyx.com>
User-Agent: mail (GNU Mailutils 3.15)
Date: Fri,  8 Mar 2024 10:03:40 -0600
Message-Id: <20240308160340.47219A0346@vulnyx.com>
From: elaine@jerry

Which millennium are you going to go to, Kramer's or Newman's?

From jerry@jerry  Fri Mar  8 10:03:40 2024
Return-Path: <elaine@jerry>
X-Original-To: jerry@vulnyx.com
Delivered-To: jerry@vulnyx.com
Received: by vulnyx.com (Postfix, from userid 1004)
        id 47219A0346; Fri,  8 Mar 2024 10:03:40 -0600 (CST)
Subject: Vacation weeks at Spain
To: <elaine@vulnyx.com>
User-Agent: mail (GNU Mailutils 3.15)
Date: Fri,  8 Mar 2024 10:03:40 -0600
Message-Id: <20240308160340.47219A0346@vulnyx.com>
From: jerry@jerry


Hi Elaine, 

If I remember correctly you were going on vacation in Spain for a few weeks, right? 
I just wanted to confirm that the password for the gym was 'imelainenotsusie',
I don't want to be there and not be able to pick up the glasses from the gym locker.

Best regards!
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo/var/mail]
└─# cat jerry 
From elaine@jerry  Fri Mar  8 10:03:34 2024
Return-Path: <elaine@jerry>
X-Original-To: jerry@vulnyx.com
Delivered-To: jerry@vulnyx.com
Received: by vulnyx.com (Postfix, from userid 1004)
        id AD58AA0346; Fri,  8 Mar 2024 10:03:34 -0600 (CST)
Subject: Kramer & Newman Clash at New Years
To: <jerry@vulnyx.com>
User-Agent: mail (GNU Mailutils 3.15)
Date: Fri,  8 Mar 2024 10:03:34 -0600
Message-Id: <20240308160334.AD58AA0346@vulnyx.com>
From: elaine@jerry

Which millennium are you going to go to, Kramer's or Newman's?
```

>有密码出现imelainenotsusie在Elaine用户
>

```
elaine@jerry:~$ id
uid=1004(elaine) gid=1004(elaine) groups=1004(elaine)
elaine@jerry:~$ 
```

```
elaine@jerry:~$ sudo -l
Matching Defaults entries for elaine on jerry:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User elaine may run the following commands on jerry:
    (ALL) NOPASSWD: /usr/bin/node /opt/scripts/*.js
elaine@jerry:~$ 
```

>尝试拿root权限了
>
![图 34](../assets/images/9fca211e72ba72e7af94f4145b04fdd3e07e19de0690d2242fb714b0d2b0f17c.png)  

>好像是会执行js后缀的，我打算在js中整入反弹shell，或者path zip这个东西，这样应该也能提权
>
![图 35](../assets/images/d91235670d0903e75df0c60e647dfb14e5bb5eeb3bc7a7f9997e7b953de80004.png)  

![图 36](../assets/images/4f2f45b4e082429f385b98e846f30287bc6b1e867e70d0cdb0fb9a0a02d6d257.png)  

>这里表示没有nc -e这个选项奥，可以逃逸一下，目前思路清晰了
>

![图 39](../assets/images/b7cafa21d7954229ec05a183d09db06eb7ab5215299fae1caadc7d32acaf385f.png)  

![图 38](../assets/images/ac32f6d071b59fc408e993c91384772df770b6d013599c61f41ea1e937fa9f98.png)  

![图 37](../assets/images/3c41b3e07cfb04f8b879f587febac9093d2e9a7e3d12b2991eb1027725d07217.png)  

>
>这样就避开了nc -e 这个选项，好了复盘结束
>
>userflag:676ced18c8f480a80ddb4351d66d5f28
>
>rootflag:4948a57231e2aed713664e3ed2659f99
>








































