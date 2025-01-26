---
title: hackmyvm Devguru靶机复盘
author: LingMj
data: 2025-01-26
categories: [hackmyvm]
tags: [bcrypt,gitea,sudo-version]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:b9:9d:24       (Unknown)
192.168.56.118  08:00:27:0a:d1:c6       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.945 seconds (131.62 hosts/sec). 3 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.56.118
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-26 06:04 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.118
Host is up (0.0012s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 2a:46:e8:2b:01:ff:57:58:7a:5f:25:a4:d6:f2:89:8e (RSA)
|   256 08:79:93:9c:e3:b4:a4:be:80:ad:61:9d:d3:88:d2:84 (ECDSA)
|_  256 9c:f9:88:d4:33:77:06:4e:d9:7c:39:17:3e:07:9c:bd (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-generator: DevGuru
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-git: 
|   192.168.56.118:80/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|     Last commit message: first commit 
|     Remotes:
|       http://devguru.local:8585/frank/devguru-website.git
|_    Project type: PHP application (guessed from .gitignore)
|_http-title: Corp - DevGuru
8585/tcp open  unknown
| fingerprint-strings: 
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Content-Type: text/html; charset=UTF-8
|     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
|     Set-Cookie: i_like_gitea=ff09e94694095e13; Path=/; HttpOnly
|     Set-Cookie: _csrf=IpwCeQIqzROZZReRwVnMFMJg63k6MTczNzkxODM1MzY4NDAxMzUzNw; Path=/; Expires=Mon, 27 Jan 2025 19:05:53 GMT; HttpOnly
|     X-Frame-Options: SAMEORIGIN
|     Date: Sun, 26 Jan 2025 19:05:53 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-">
|     <head data-suburl="">
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta http-equiv="x-ua-compatible" content="ie=edge">
|     <title> Gitea: Git with a cup of tea </title>
|     <link rel="manifest" href="/manifest.json" crossorigin="use-credentials">
|     <meta name="theme-color" content="#6cc644">
|     <meta name="author" content="Gitea - Git with a cup of tea" />
|     <meta name="description" content="Gitea (Git with a cup of tea) is a painless
|   HTTPOptions: 
|     HTTP/1.0 404 Not Found
|     Content-Type: text/html; charset=UTF-8
|     Set-Cookie: lang=en-US; Path=/; Max-Age=2147483647
|     Set-Cookie: i_like_gitea=4a06887fd403dbac; Path=/; HttpOnly
|     Set-Cookie: _csrf=IPSaNR6_pk_SVYZMBOdSYJ8bPxw6MTczNzkxODM1NDgzMTU3Mzk0OA; Path=/; Expires=Mon, 27 Jan 2025 19:05:54 GMT; HttpOnly
|     X-Frame-Options: SAMEORIGIN
|     Date: Sun, 26 Jan 2025 19:05:55 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-">
|     <head data-suburl="">
|     <meta charset="utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <meta http-equiv="x-ua-compatible" content="ie=edge">
|     <title>Page Not Found - Gitea: Git with a cup of tea </title>
|     <link rel="manifest" href="/manifest.json" crossorigin="use-credentials">
|     <meta name="theme-color" content="#6cc644">
|     <meta name="author" content="Gitea - Git with a cup of tea" />
|_    <meta name="description" content="Gitea (Git with a c
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8585-TCP:V=7.94SVN%I=7%D=1/26%Time=67961713%P=x86_64-pc-linux-gnu%r
SF:(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x
SF:20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Ba
SF:d\x20Request")%r(GetRequest,1000,"HTTP/1\.0\x20200\x20OK\r\nContent-Typ
SF:e:\x20text/html;\x20charset=UTF-8\r\nSet-Cookie:\x20lang=en-US;\x20Path
SF:=/;\x20Max-Age=2147483647\r\nSet-Cookie:\x20i_like_gitea=ff09e94694095e
SF:13;\x20Path=/;\x20HttpOnly\r\nSet-Cookie:\x20_csrf=IpwCeQIqzROZZReRwVnM
SF:FMJg63k6MTczNzkxODM1MzY4NDAxMzUzNw;\x20Path=/;\x20Expires=Mon,\x2027\x2
SF:0Jan\x202025\x2019:05:53\x20GMT;\x20HttpOnly\r\nX-Frame-Options:\x20SAM
SF:EORIGIN\r\nDate:\x20Sun,\x2026\x20Jan\x202025\x2019:05:53\x20GMT\r\n\r\
SF:n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"theme-\">\n<hea
SF:d\x20data-suburl=\"\">\n\t<meta\x20charset=\"utf-8\">\n\t<meta\x20name=
SF:\"viewport\"\x20content=\"width=device-width,\x20initial-scale=1\">\n\t
SF:<meta\x20http-equiv=\"x-ua-compatible\"\x20content=\"ie=edge\">\n\t<tit
SF:le>\x20Gitea:\x20Git\x20with\x20a\x20cup\x20of\x20tea\x20</title>\n\t<l
SF:ink\x20rel=\"manifest\"\x20href=\"/manifest\.json\"\x20crossorigin=\"us
SF:e-credentials\">\n\t<meta\x20name=\"theme-color\"\x20content=\"#6cc644\
SF:">\n\t<meta\x20name=\"author\"\x20content=\"Gitea\x20-\x20Git\x20with\x
SF:20a\x20cup\x20of\x20tea\"\x20/>\n\t<meta\x20name=\"description\"\x20con
SF:tent=\"Gitea\x20\(Git\x20with\x20a\x20cup\x20of\x20tea\)\x20is\x20a\x20
SF:painless")%r(HTTPOptions,212E,"HTTP/1\.0\x20404\x20Not\x20Found\r\nCont
SF:ent-Type:\x20text/html;\x20charset=UTF-8\r\nSet-Cookie:\x20lang=en-US;\
SF:x20Path=/;\x20Max-Age=2147483647\r\nSet-Cookie:\x20i_like_gitea=4a06887
SF:fd403dbac;\x20Path=/;\x20HttpOnly\r\nSet-Cookie:\x20_csrf=IPSaNR6_pk_SV
SF:YZMBOdSYJ8bPxw6MTczNzkxODM1NDgzMTU3Mzk0OA;\x20Path=/;\x20Expires=Mon,\x
SF:2027\x20Jan\x202025\x2019:05:54\x20GMT;\x20HttpOnly\r\nX-Frame-Options:
SF:\x20SAMEORIGIN\r\nDate:\x20Sun,\x2026\x20Jan\x202025\x2019:05:55\x20GMT
SF:\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"theme-\"
SF:>\n<head\x20data-suburl=\"\">\n\t<meta\x20charset=\"utf-8\">\n\t<meta\x
SF:20name=\"viewport\"\x20content=\"width=device-width,\x20initial-scale=1
SF:\">\n\t<meta\x20http-equiv=\"x-ua-compatible\"\x20content=\"ie=edge\">\
SF:n\t<title>Page\x20Not\x20Found\x20-\x20\x20Gitea:\x20Git\x20with\x20a\x
SF:20cup\x20of\x20tea\x20</title>\n\t<link\x20rel=\"manifest\"\x20href=\"/
SF:manifest\.json\"\x20crossorigin=\"use-credentials\">\n\t<meta\x20name=\
SF:"theme-color\"\x20content=\"#6cc644\">\n\t<meta\x20name=\"author\"\x20c
SF:ontent=\"Gitea\x20-\x20Git\x20with\x20a\x20cup\x20of\x20tea\"\x20/>\n\t
SF:<meta\x20name=\"description\"\x20content=\"Gitea\x20\(Git\x20with\x20a\
SF:x20c");
MAC Address: 08:00:27:0A:D1:C6 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 152.50 seconds
```

## 获取webshell
![图 0](../assets/images/677d1dde17f865295034c2626b139cc150eb8f8b092b94698ac64d7b210acb05.png)  

>有一些线索，可以操作一手
>
![图 1](../assets/images/570e4a221342f0bb19b8c4bb1565e477db35961eac515dfd0d0b9dea340abf4b.png)  
![图 2](../assets/images/f7a4c0ad1dc8e398641fe1c356bc24ade0d62d479395451e646738c7b3a7dc76.png)  

>目前没有hmv的域名，有local
>
![图 3](../assets/images/c93f4cdc1c90a0bed276bd5fe2f2219a452a5fa4edc9a0ba027754a704056bd1.png)  
![图 4](../assets/images/9f2b1a94511df2c52a24a8d368f063c84d09af60c8b3a81bd0d666f29af4235e.png)  

>有版本，可以查一手，gitea是1.12.5，go是1.14.9
>
![图 6](../assets/images/eb42cfab76e679261839a3daa72340157e49d386ff2fec478a59dda8227af974.png)  
![图 7](../assets/images/f308f3401837a1b852a687e64c4ff0aabf2b6a1553c5389526743d564e8ce21b.png)  

>试gitea
> 
![图 8](../assets/images/585e56ee3f18921fb26888147a9740ebfe33205502a8d103108d17422fe19eb1.png)  

>这里需要用户和密码
>
![图 9](../assets/images/7b3bec99c46c59baa5b983ab829d4814bb4eb069e4cc1f94008247c877c196fd.png)  

>先做目录爆破，一伙再进行
>

![图 10](../assets/images/082f76791388363865fb6ffc8441b5924d79e91acefc1af45ac540851e93c273.png)  

>没啥东西，一个一个看
>
![图 11](../assets/images/6c6149d993acbe999f11ef67ff5769d5e3684eb13c0b647bf86aab37c1f54cee.png)  

>豁又一个登录，可以尝试一下
>
![图 12](../assets/images/ebef837ce4646132d6b0d8f5a1749db9e2cf7113cc2a3428facc95905d8321a2.png)  
![图 13](../assets/images/89cf12208231be230bfcc4d7dea8248336736e3751bc8866d9cbb3f41a2e6916.png)  

>爆破无果，看看漏洞了
>
![图 14](../assets/images/f1eaece74f714e5a7119b8f8b67e2e0279797e198fda7d4e3facfcf2d71dc4f5.png)  

>这是作者的提示，一开始我给了这个东西有git的，去测试一下了
>
![图 15](../assets/images/09495877aec3860b383c24454c96e13e12fe7199abb70de446ec96f40d54066f.png)  
![图 16](../assets/images/321c2ec5722589a50dce081e4312c9384c8a2a03349a7d4b479597b896abafdd.png)  
![图 17](../assets/images/2a0b3fcf4230f4a2ef9b26d26e43f4cef96580e8264d8b87020000f783b1ecff.png)  
![图 18](../assets/images/0eaab54326050f9f7f0d1052fb664eb2f97fce21604e3e0e44c5f8bb1a60eb77.png)  

>没有账号密码，但是我们git下来了可以自己找
>
![图 19](../assets/images/df2d3c627bee370ee4f105a56811f771485efe8d039d76eb49a94fd67f7d5977.png)  
![图 20](../assets/images/51ec69a009471f39b3796bd4f6ca2b68adf718ed9312f1ec3a58661d41221039.png)  
![图 21](../assets/images/c050194edf32335c10cc18883802c77c4f0aa650c9b79011fd5280692f9d4e74.png)  
![图 22](../assets/images/5033d0e056ffa671681cb1572d27bfbf766c6d45198b0685f4efdee46a5c94de.png)  
![图 23](../assets/images/9bbf4ee251b9118e50fe1861f3897fa88f7419f6f53dc4b8721f686e3150f7ec.png)  

>有说明加密方案，爆破不出可以利用一下。
>
![图 24](../assets/images/a4cc600bc25db4d089d6c8b920455c680d3dc17c53e48fe9f3dce76dc6a3e916.png)  
![图 25](../assets/images/8a74884f11c3ad2f536d39045bad416897543df1450a7bb30de5783d15facf25.png)  

>找到一个生成器
>
![图 26](../assets/images/c2424f750bed8ec9a4007892f463e7dc84a5398d4b21748ee16d5467adb293cf.png)  

>如果爆破不出直接用来修改，因为上面存在修改的按钮
>
>好像没成功直接改了
![图 27](../assets/images/bccebd5d4a252ac2fa1cc6bd6fef963469b562a68f0de5d7d002af57e651d831.png)  
![图 28](../assets/images/3b39a2cec3289a26733f319367d5e2d05b46521a2b7a9a902f119409eef230a3.png)  
![图 29](../assets/images/80addfe09a749256906f73550296c052f2219d89a1e88047f8bb2aa8150efc3d.png)  

![图 30](../assets/images/5c8a4722b34defa2d239c185e37cb41795db43e8b97c14b380968b7baf08442d.png)  

>靶机有点久，找漏洞的话时间可能会不太能找到，看看网页里面
>
![图 34](../assets/images/e885a564c14de3e6fed65b867002e975a86f05a43df1b62e1b9be2d70ddfca27.png)  

![图 31](../assets/images/e3e246b30663f80df7c5a6c656c88d19ac1fbaae2dfe9f535d73e5abb747c89a.png)  
![图 32](../assets/images/4af02347c43debad3668a13fc219f5a4e9cc99c8ff7f70e8da2dac6d8c4c4360.png)  

>网页可以修改，这里使用上面有的{}框架注入，只有十年前的东西
>
![图 33](../assets/images/f487c0f3f8d07431efa9b517f955bb8ed3c9a96105f4c793e4597145b9069204.png)  

>发现存在这个注入，直接进行操作
>
![图 35](../assets/images/3848d8feb48fbd3d5386b71535dccb459205861cc0118859d218658fe7e90515.png)  
![图 36](../assets/images/4d91163ede0fc75f6df6ad18dda6d41655c220675ad08c8f59840a96ac28635a.png)  

>这是python没成功，尝试php的
>
![图 38](../assets/images/84225f24303b0f5597dc805afafbd759347332539e7d994d582d2cfdf0ebdad1.png)  

![图 37](../assets/images/0751dec8f8b20047781863e45bed0e23b94835c0ebc30177aa26a1bfe3263d31.png)  

>搞崩了，换一换思路
>
![图 39](../assets/images/6ad71e3ccc9ca35f45b05d5bad654961f522ab9dbc26aa857d178028261598f2.png)  
![图 40](../assets/images/637c58e21a9fde97791f8f77089df5b2dcfa995b2e19e561a2bea0801395b655.png)  

>终于成功了，试了很多方案了
>
>不过我直接用nc没成功，但是可以wget
![图 41](../assets/images/fbaf70c7817971a12e58f13218a8c4e505bc0e7cd7dfec05e0c84c05f79abeca.png)  
![图 42](../assets/images/aa3ca5de48242dc3c86b5c09b144f19cfa44b82e9c093bf8fbc51959eeae841d.png)  
![图 43](../assets/images/d77e7dc5b312fadfc4b81d11897ec25a639a71fb411dfd714b3268fb37136955.png)  
![图 44](../assets/images/817cee654d86119e08b9b5f7258110c5d97ddea21312e6aeeee58ad69624a9e7.png)  
![图 45](../assets/images/27121abd22a84e239e53f8b834b3eb06727651fc73a2b3c9c6dab20c8ee0b368.png)  


## 提权
```
ww-data@devguru:/var/www/html$ cd /home/
www-data@devguru:/home$ ls -al
total 12
drwxr-xr-x  3 root  root  4096 Nov 18  2020 .
drwxr-xr-x 25 root  root  4096 Nov 19  2020 ..
drwxr-x---  7 frank frank 4096 Nov 19  2020 frank
www-data@devguru:/home$ cd frank/
bash: cd: frank/: Permission denied
www-data@devguru:/home$ sudo -l
[sudo] password for www-data: 
www-data@devguru:/home$ ^C
www-data@devguru:/home$ 
```

```
www-data@devguru:/home$ cd /opt/
www-data@devguru:/opt$ ls -al
total 16
drwxr-xr-x  4 root  root  4096 Nov 18  2020 .
drwxr-xr-x 25 root  root  4096 Nov 19  2020 ..
drwx--x--x  4 root  root  4096 Nov 18  2020 containerd
drwxr-x---  3 frank frank 4096 Jan 26 13:00 gitea
www-data@devguru:/opt$ cd gitea/
bash: cd: gitea/: Permission denied
www-data@devguru:/opt$ cd containerd/
www-data@devguru:/opt/containerd$ ls -al
ls: cannot open directory '.': Permission denied
www-data@devguru:/opt/containerd$ 
```
![图 47](../assets/images/6ca376385102a429d4cf6ce0cda348132b49bcb97073ea36489c02e908f195c8.png)  

>有线索了
>
![图 48](../assets/images/7472018df475bec7b19eeb140d0be1c4f8b4a649646c0845d1fc1a74ece79ec1.png)  
![图 49](../assets/images/4f2f790b1cc49922248d39620a58d19e61836e901d686025b19aff5481e8a29d.png)  
![图 50](../assets/images/e34d9d6b0ff57fa5b6f268050616843f69cf71b7b543bc51282fdabc617ae217.png)  
![图 51](../assets/images/bb5beea05a4b03df761fe428c5de586832bbcf5652ffe377dbd26ed8e2bd92a0.png)  

>就发现这个，其他啥也没发现，用工具把
>
![图 52](../assets/images/1881faac50ef611d75ce085f9434bc88a97f82ed975a9d46c9c7af94391109a4.png)  
![图 53](../assets/images/e1e2b7232fbaec537bf83eb5118a8810f68a593d160a9a5f1526383afedb36e3.png)  

```
www-data@devguru:/tmp$ ls -al /etc/gitea/app.ini
ls: cannot access '/etc/gitea/app.ini': Permission denied
www-data@devguru:/tmp$ cd /usr/local/bin/gitea
bash: cd: /usr/local/bin/gitea: Not a directory
www-data@devguru:/tmp$ ls -al /usr/local/bin/gitea
-rwxrwxr-x 1 frank frank 107443064 Nov 19  2020 /usr/local/bin/gitea
```

![图 54](../assets/images/22f74efe768535a7accf34c06321c02c8d860d768b4bbb4a357cb50d9213fa00.png)  

>无定时任务，得对另外一个端口进程操作
>
![图 55](../assets/images/fce5afec423576f46902230d73a50e1846673366093721b95524b0bbca00a9c1.png)  
![图 56](../assets/images/181edf266521cd2322307d03818b9fb2b435f12b23d224b201f985bb2a200ff8.png)  
![图 57](../assets/images/1b6ca97a42c09a80001b0d90c6bfa1d9b28487647efd3f2e7ee590044a378055.png)  

>更懵了，回到/var/backup/下面的app.ini.bak,这个是这个app.ini的备份把
>
![图 58](../assets/images/4aaae726f023fb1edb3325f9731b7ae021e6a3d868237e3a20b167123286af47.png)  

>继续重头开始看能发现点东西
>
![图 59](../assets/images/91eaebb884fdfc232c92cb19b71dad6058aacf9e92eda59065772132e8ea47fb.png)  
![图 60](../assets/images/0a8b57a4f0eb5062bf8d039fb0be6bcb38c53dd046d6bd7bbf1bcfed8aaf0f2d.png)  

>又有一个密码
>
![图 61](../assets/images/48375201f008530bb76612bcc1cabd6ab966bbb729a666d1b3c5f2eeaa52011c.png)  

>不是爆破
>

![图 62](../assets/images/51036b6e5c92b8f1d03c5e494bb43de110eb24d9d1d72574a4a169c88c8ac1f9.png)  

>给了一点线索把，进行的加密方式，rands，salt都有
>
![图 63](../assets/images/432e85cec4e8555d080750a9cf9d10650c41280383a9af4f63b25c4cc45d2f74.png)  

>算了像,第一个一样改一下密码，我们利用网站的mysql好改
![图 64](../assets/images/915d609e87b4b74818239e55e3e3b4a985d2b7ba7ea71a2dd6067bc31ef13c11.png)  
![图 65](../assets/images/51c14f7b218c30bf4c0463fa774aec7f33cd12e40b8559a2d10a0cabace3cfc6.png)  

>128有点短奥
>
![图 66](../assets/images/ec552bc7e4d126ee418799d6494ba4357e19c7db7403696cbd9722b9579c311c.png)  
![图 67](../assets/images/6517640e608e4310703ed2a020c83fff837d0a3e357693a144970d701abe39ed.png)  

>明显长度不够
>
![图 68](../assets/images/ce491b9da02103b07f162ce96aa52e2ab07b2fb2c7958b060579caa19fe27770.png)  

>这样又长了
![图 69](../assets/images/607c00b89fecae5d8c748ddb07103d72c6b7bed11dc3b6b6e6c548107a93139d.png)  

>迭代那个没看懂但是这样有一个提示，可以用之前的密码进行操作。
>
![图 70](../assets/images/ce979e7cbf4b093c45c4993133beadf818b2daa45460ca9d61d995ae886753a1.png)  

>ok,可以登录上去，我们还可以利用之前的poc
>
![图 71](../assets/images/4fd88737ccc8ad70df0f41cce1d276947d20e9f0ee56d1619e3ce8ee0a2ea57f.png)  
![图 72](../assets/images/cc505af2d2e0a77a3335f6da4c4a2ad2538198073feb0afb6e1b6b297b60290f.png)  

>好像是域名的事情，尝试的加一下
>
![图 73](../assets/images/2c9d4899d5e114987881be77210e650aa6cfd49bab1a18e6270818ec5cfb5a90.png)  
![图 74](../assets/images/4001d954466e353deae595eac34b6f9aa5d29c9c3a57d2d7d3dd2069c6a91d2a.png)  

>不见shell,弹回来
>
![图 75](../assets/images/02430631b4761c1b6e09bad40b2370767807c51753ef009d1b1e8c2ee1186fb3.png)  
![图 76](../assets/images/db5ea24e39e1ce39a2e9bb46a2d608b25126b2f43084ac300079a0f84db556d6.png)  


>还是不见
>
![图 77](../assets/images/1e4bd8a1c7c8bb6f65df15c8f96d45239fc3d3e2023eeac3ea09033b5fbcbf51.png)  

>手动创建一下，先创建，再到setting里面找git hooks，继续进行post那个的操作，粘贴代码然后就有提升怎么git了
>
![图 78](../assets/images/b2a3055b8a7d2a704173c226166790d161b226150488618133917cfb0b5f335b.png)  
![图 79](../assets/images/be1875cc843e484a653c278d2f6473054da5d2bc1978deb6da42a5f8d45b8cf6.png)  


```
www-data@devguru:/var/backups$ cd /tmp/
www-data@devguru:/tmp$ mkdir reverse
www-data@devguru:/tmp$ cd reverse/
www-data@devguru:/tmp/reverse$ touch README.md
www-data@devguru:/tmp/reverse$ git init
Initialized empty Git repository in /tmp/reverse/.git/
www-data@devguru:/tmp/reverse$ git add .
www-data@devguru:/tmp/reverse$ git commit -m "first commit"
[master (root-commit) 6938a07] first commit
 Committer: www-data <www-data@devguru.local>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly. Run the
following command and follow the instructions in your editor to edit
your configuration file:

    git config --global --edit

After doing this, you may fix the identity used for this commit with:

    git commit --amend --reset-author

 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
www-data@devguru:/tmp/reverse$ git remote add origin http://devguru.local:8585/frank/revrse.git
www-data@devguru:/tmp/reverse$ git push -u origin master
Username for 'http://devguru.local:8585': frank
Password for 'http://frank@devguru.local:8585': 
Counting objects: 3, done.
Writing objects: 100% (3/3), 210 bytes | 26.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)

```

![图 80](../assets/images/8b5fe35a28ea5ff559a265d67878155595e094d8663e2ee109169dc87236530f.png)  


```
frank@devguru:/home/frank$ sudo -l
Matching Defaults entries for frank on devguru:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User frank may run the following commands on devguru:
    (ALL, !root) NOPASSWD: /usr/bin/sqlite3
```

>这里新出了一个东西关于sudo的，直接问gtp
>
![图 81](../assets/images/7428389f0361d4ae2b238509403ace2faa43969563d7a4ea3600dcfc39075fcc.png) 
![图 83](../assets/images/0019e1a65c25723100f9659e8aa9eb47d149823801383345c721f52dd9b94394.png)  

![图 82](../assets/images/d5b21e2075625222861ff2a590daa7419a24ca1a0b79057e32040a4f85acb3ef.png)  

>版本小于可以使用
>
![图 84](../assets/images/082e4d2f97e1a46f83789b39428386121dadd5c58e33c6a073fcae49c8378673.png)  
![图 85](../assets/images/77556fbfce6b34a6a94642e3d3852d36bbae16db633208db4bd45b4de426fe32.png)  


>好了到这里结束了

>userflag:22854d0aec6ba776f9d35bf7b0e00217
>
>rootflag:96440606fb88aa7497cde5a8e68daf8f
>