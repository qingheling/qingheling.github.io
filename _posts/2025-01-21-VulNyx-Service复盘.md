---
title: VulNyx Service靶机复盘
author: LingMj
data: 2025-01-21
categories: [VulNyx]
tags: [Joomla,docker,brute]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l                    
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.188  00:0c:29:52:99:42       (Unknown)
192.168.26.254  00:50:56:e5:dc:17       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.928 seconds (132.78 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.188
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-20 23:41 EST
Nmap scan report for 192.168.26.188 (192.168.26.188)
Host is up (0.0017s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp   open  http    nginx 1.22.1
|_http-server-header: nginx/1.22.1
|_http-title: Welcome to nginx!
8080/tcp open  http    Apache httpd 2.4.54 ((Debian))
|_http-title: Welcome to nginx!
| http-robots.txt: 16 disallowed entries (15 shown)
| /joomla/administrator/ /administrator/ /api/ /bin/ 
| /cache/ /cli/ /components/ /includes/ /installation/ 
|_/language/ /layouts/ /libraries/ /logs/ /modules/ /plugins/
|_http-server-header: Apache/2.4.54 (Debian)
|_http-open-proxy: Proxy might be redirecting requests
MAC Address: 00:0C:29:52:99:42 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 74.99 seconds
```

## 获取webshell
![图 0](../assets/images/ca55e7e185d4714e5d9b1693b1b8e51fad639992792ea977eda2b0e39b260b7a.png)  
![图 1](../assets/images/7c416ad308beaac51acd494ae5ff92bd585530820db927e9efdd042b711fe299.png)  
![图 2](../assets/images/605657cedfbf6e63b8bf556d936cda445507ac86ef0617155ce0d36ceda5c963.png)  
![图 3](../assets/images/246a1499c737f91886736076e0ac154e25f5b53edbc638d4880532a158ff4dfc.png)  
![图 4](../assets/images/5a65d55b04bae10c93050b4b76c054145c20011c85ce7e54a095744a544e267b.png)  
![图 5](../assets/images/aed0cdbc1d264a02f416f5cd4d6e34985d1fe353f6aab6dd0d4c84aff013c999.png)  
![图 6](../assets/images/755da2dfd6235021c5aa8e7d68667218380ee3444f67aa095a0f20e839bd7749.png)  

>找到版本
>
![图 7](../assets/images/af2bd5848ffa40776fe146529aa51d64299e7336b4c1db61e32204b2d43c451e.png)  
![图 8](../assets/images/d666c2209e6eb25330bb12adbcfbefe46d6c1cdce630ab59a1a55d2b72b8c5c3.png)  

>貌似sql的一个注入，试了半天不是，是一个看js的漏洞
>
![图 9](../assets/images/7ea73458349bf3b597debfe5c3574eb2d7059c4143393a25857d6598c7285b74.png)  
![图 10](../assets/images/baf495cfe1429e561ac7c237027803094bcf0bb228e4a01025b66afa9a09cd8b.png)
![图 12](../assets/images/241eaa2a45cafbbbdd71e24b9c9dc2a1798414b561b18ff700407e50662e01e9.png)  
![图 11](../assets/images/ccda9b44ca4730f19d380a99721464dc1477596a08104d8b1392d43135f757ee.png)  
>这样我们就拿到了用户名和密码为admin，j00mL@123###
>
![图 13](../assets/images/b8ae04ed698bc71e4fbec511e90db8c92e4f6fd05234660bc901fa2923238b4c.png)  
![图 14](../assets/images/1643be6189f04114b019944977674615053c63e402ca1dd00ffaeb8bf8386c9c.png)  

>改网页就行了
>
![图 15](../assets/images/9b6b27759860660f03057eb921844909c17382383985f4f1893f02a57beb1b5c.png)  
![图 16](../assets/images/f76e13c95ffacc140a7ed04354ed55c21b7ec9a16b9903b8d86ac2c80f4c1d23.png)  
![图 17](../assets/images/73e6f5a3182aac6b27de791781193a3686aa2b3b669b1eb9036de7ebcad52def.png)  
![图 18](../assets/images/729a79d147fc06e7685b3b80436c1f7e1b18b6cc6a27a6ab72eebcc9366bd80b.png)  
![图 19](../assets/images/7a89b2b5ffe79b313193012fb28f68e6f894166f577deecdc90d2cd4d5b9dd47.png)  

>
>这个拿shell一直没成功
![图 20](../assets/images/f40e7405052867bf7c5ad9ce4dcdf636e595ad1509ea06631935830e0db978d5.png)  
![图 21](../assets/images/8c5bbf640580aeabf98f152074c5575ee76c22f718b931d1f8585884b7adad8f.png)  

>无busybox，无nc，无wget，选择手搓命令进去
![图 22](../assets/images/2d3393f43d5f3719eeae26a2506237dc8962c2823dcd5294ffa57bafffd6e136.png)  
![图 23](../assets/images/db709045aac0761195cb519b8d50dd4db1c7be23c9db8e1cfcb97cfacb25846e.png)  
![图 24](../assets/images/4ba0c5d27bce9d81dabc15449989e6efeb6c78b0cdd8808d9115a3bf1baee924.png)  

>感觉这个拿shell就是各显神通了

## 提权
```
www-data@640aa6d0dea4:/var/www/html/administrator$ ls -al
total 48
drwxr-xr-x 11 www-data www-data 4096 Jan 30  2023 .
drwxr-xr-x 17 www-data www-data 4096 May 23  2024 ..
drwxr-xr-x  3 www-data www-data 4096 May 23  2024 cache
drwxr-xr-x 37 www-data www-data 4096 Jan 30  2023 components
drwxr-xr-x  3 www-data www-data 4096 Jan 30  2023 help
drwxr-xr-x  2 www-data www-data 4096 Jan 30  2023 includes
-rw-r--r--  1 www-data www-data 1080 Jan 30  2023 index.php
drwxr-xr-x  4 www-data www-data 4096 Jan 30  2023 language
drwxr-xr-x  2 www-data www-data 4096 Jan 21 12:42 logs
drwxr-xr-x  5 www-data www-data 4096 Jan 30  2023 manifests
drwxr-xr-x 25 www-data www-data 4096 Jan 30  2023 modules
drwxr-xr-x  4 www-data www-data 4096 Jan 30  2023 templates
www-data@640aa6d0dea4:/var/www/html/administrator$ cd in
bash: cd: in: No such file or directory
www-data@640aa6d0dea4:/var/www/html/administrator$ cd includes/
www-data@640aa6d0dea4:/var/www/html/administrator/includes$ ls -al
total 20
drwxr-xr-x  2 www-data www-data 4096 Jan 30  2023 .
drwxr-xr-x 11 www-data www-data 4096 Jan 30  2023 ..
-rw-r--r--  1 www-data www-data 2287 Jan 30  2023 app.php
-rw-r--r--  1 www-data www-data 1144 Jan 30  2023 defines.php
-rw-r--r--  1 www-data www-data 3225 Jan 30  2023 framework.php
www-data@640aa6d0dea4:/var/www/html/administrator/includes$ sudo -l
bash: sudo: command not found
www-data@640aa6d0dea4:/var/www/html/administrator/includes$ cd /opt/
www-data@640aa6d0dea4:/opt$ ls -al
total 8
drwxr-xr-x 2 root root 4096 Feb  8  2023 .
drwxr-xr-x 1 root root 4096 May 23  2024 ..
www-data@640aa6d0dea4:/opt$ cd /var/backups/
www-data@640aa6d0dea4:/var/backups$ ls -al
total 12
drwxr-xr-x 2 root root 4096 Dec  9  2022 .
drwxr-xr-x 1 root root 4096 Feb  9  2023 ..
www-data@640aa6d0dea4:/var/backups$ 
```

```
www-data@640aa6d0dea4:/var/backups$ ls -al
total 12
drwxr-xr-x 2 root root 4096 Dec  9  2022 .
drwxr-xr-x 1 root root 4096 Feb  9  2023 ..
www-data@640aa6d0dea4:/var/backups$ cd 
bash: cd: HOME not set
www-data@640aa6d0dea4:/var/backups$ cd /home/
www-data@640aa6d0dea4:/home$ l -al
bash: l: command not found
www-data@640aa6d0dea4:/home$ ls -al
total 8
drwxr-xr-x 2 root root 4096 Dec  9  2022 .
drwxr-xr-x 1 root root 4096 May 23  2024 ..
www-data@640aa6d0dea4:/home$ ip a
bash: ip: command not found
www-data@640aa6d0dea4:/home$ 
```

>这竟然不是主机，还得找线索去拿用户
>

```
www-data@640aa6d0dea4:ls -al
-rw-r--r--  1 www-data www-data  2974 Jan 30  2023 web.config.txt
www-data@640aa6d0dea4:/var/www/html$ cat web.config.txt 
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
   <location path=".">
   <system.webServer>
       <directoryBrowse enabled="false" />
       <rewrite>
           <rules>
               <rule name="Joomla! Common Exploits Prevention" stopProcessing="true">
                   <match url="^(.*)$" ignoreCase="false" />
                   <conditions logicalGrouping="MatchAny">
                       <add input="{QUERY_STRING}" pattern="base64_encode[^(]*\([^)]*\)" ignoreCase="false" />
                       <add input="{QUERY_STRING}" pattern="(&gt;|%3C)([^s]*s)+cript.*(&lt;|%3E)" />
                       <add input="{QUERY_STRING}" pattern="GLOBALS(=|\[|\%[0-9A-Z]{0,2})" ignoreCase="false" />
                       <add input="{QUERY_STRING}" pattern="_REQUEST(=|\[|\%[0-9A-Z]{0,2})" ignoreCase="false" />
                   </conditions>
                   <action type="CustomResponse" url="index.php" statusCode="403" statusReason="Forbidden" statusDescription="Forbidden" />
               </rule>
               <rule name="Joomla! API Application SEF URLs">
                   <match url="^api/(.*)" ignoreCase="false" />
                   <conditions logicalGrouping="MatchAll">
                     <add input="{URL}" pattern="^/api/index.php" ignoreCase="true" negate="true" />
                     <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                     <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
                   </conditions>
                   <action type="Rewrite" url="api/index.php" />
               </rule>
               <rule name="Joomla! Public Frontend SEF URLs">
                   <match url="(.*)" ignoreCase="false" />
                   <conditions logicalGrouping="MatchAll">
                     <add input="{URL}" pattern="^/index.php" ignoreCase="true" negate="true" />
                     <add input="{REQUEST_FILENAME}" matchType="IsFile" ignoreCase="false" negate="true" />
                     <add input="{REQUEST_FILENAME}" matchType="IsDirectory" ignoreCase="false" negate="true" />
                   </conditions>
                   <action type="Rewrite" url="index.php" />
               </rule>
           </rules>
       </rewrite>
       <httpProtocol>
           <customHeaders>
               <add name="X-Content-Type-Options" value="nosniff" />
               <!-- Protect against certain cross-origin requests. More information can be found here: -->
               <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/Cross-Origin_Resource_Policy_(CORP) -->
               <!-- https://web.dev/why-coop-coep/ -->
               <!-- <add name="Cross-Origin-Resource-Policy" value="same-origin" /> -->
               <!-- <add name="Cross-Origin-Embedder-Policy" value="require-corp" /> -->
           </customHeaders>
       </httpProtocol>
   </system.webServer>
   </location>
</configuration>

```

>无线索，只有root，跑密码吧
>

![图 25](../assets/images/c1c20ad595ec859afe2ff160159bb93cc182a44c847d24f79ef2dfb796515341.png)

![图 26](../assets/images/a57b5032d3a48d8af0404eb90728ef9fd623ca9b7855a1da2d475c22eba1fa3d.png)  


>密码fucker

```
www-data@640aa6d0dea4:/tmp$ su - root 
Password: 
root@640aa6d0dea4:~# ls
root@640aa6d0dea4:~# ls -la
total 24
drwx------ 1 root root 4096 May 23  2024 .
drwxr-xr-x 1 root root 4096 May 23  2024 ..
-rw------- 1 root root   64 May 23  2024 .bash_history
-rw-r--r-- 1 root root  571 Apr 10  2021 .bashrc
-r-------- 1 root root 2590 May 23  2024 .joel_key
-rw-r--r-- 1 root root  161 Jul  9  2019 .profile
root@640aa6d0dea4:~# cat .joel_key 
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABlwAAAAdzc2gtcn
NhAAAAAwEAAQAAAYEA6uAPqYhBq+6nJjM1Sar0v/G5yamiu3fahLgG2pvlqi0HnrSXDVLs
qhiUBC/nbO1re6OO5G3x6mKZ22Bc3CfSvAEYkIfGwX9ZX5zSMT6B2BbP99J9ScNRcaKvNa
MluAbGq4gFGN4VhIh0msCq1uF05DPeNcIFUlKB81fn7WR2zH/OcBhRZGfhMvN/vXmf/gMU
R28edk3HbJpmhoea3zv3YRUS0IOI43bYBwtCBR9vXWFaz9fdYPMHDsi310KK+k3AiMPIEv
hYd4SCEfnBJmLC5xfvv35JmeKQsEC+9nn3MawaXvXv7rORkG8Kj33c1K7wZR2YoFgg5Mz7
RnPigtNv/hGXqaj+t54cSeeHZgUDRIevAomDMWMMw4m2jKefGBsyXl/cyo7hytoeGGZVzQ
0jlxA0B5E2PhEUGoVlIpgPRyM1OR2Zt9eFkVXNydTnemwScnIdPBXnWt9Qe0P4UCo3dQjg
t1qBAUvsxSvDyJ/jbGw9l/yehihsYCRH8zVdSsxjAAAFgNAuVJbQLlSWAAAAB3NzaC1yc2
EAAAGBAOrgD6mIQavupyYzNUmq9L/xucmport32oS4Btqb5aotB560lw1S7KoYlAQv52zt
a3ujjuRt8epimdtgXNwn0rwBGJCHxsF/WV+c0jE+gdgWz/fSfUnDUXGirzWjJbgGxquIBR
jeFYSIdJrAqtbhdOQz3jXCBVJSgfNX5+1kdsx/znAYUWRn4TLzf715n/4DFEdvHnZNx2ya
ZoaHmt8792EVEtCDiON22AcLQgUfb11hWs/X3WDzBw7It9dCivpNwIjDyBL4WHeEghH5wS
ZiwucX779+SZnikLBAvvZ59zGsGl717+6zkZBvCo993NSu8GUdmKBYIOTM+0Zz4oLTb/4R
l6mo/reeHEnnh2YFA0SHrwKJgzFjDMOJtoynnxgbMl5f3MqO4craHhhmVc0NI5cQNAeRNj
4RFBqFZSKYD0cjNTkdmbfXhZFVzcnU53psEnJyHTwV51rfUHtD+FAqN3UI4LdagQFL7MUr
w8if42xsPZf8noYobGAkR/M1XUrMYwAAAAMBAAEAAAGADbx/6Kl+3xLMihfkQxmGnzPVv/
JgrDfEuw6rhp0nz+LlTLCsGuE21SCDMOwqS+e7aauK9QIXFrRLN5Ye/YMuDcUUo4iapQC7
1d3X5H25iKKWPsdRK39gToCLYFwucwuhEobDIY1aKHpLM4JsG2LBJtgqY+4jyIjpxQpm69
knIiuhWP3wbf+t9IBi1U59Kb+9WmDTIRZzFioWL+z+BOs+uwSeqTzgGcl+td7AhDMunKv6
bH29RT5sCGnQZ/xMy3nA2arJplFglfkoF5nueRwTsmLZUSoeoguuGPQmzskcMJ46Gu291g
pzUyXWNXaKcdgO+cT5UBP3AzAeYg7ITm2uOf4bGeOM5EEuDDnUmHn+mGl03fbxE3aLSEBL
uvfbgPkkRJcln9kUTjO/h2jcQ98qMjQvRlAkHhEC6vXpZkTrymTUpOupRCUEkRn1l75Dqh
nBKfP9vGOmMimg83jUg/JnXiAZZlYfGZmGEdto73uyrkC/jcTwz+CS9J3abnRO7yWVAAAA
wQC9eINyHzjhX8J06C+Q8HGhTE3eNdArTn9qfU85dihgXdEb27EmqxLUXRkdnU/h7rIa3A
qsoc2Ckhy57fZkQErQQiIBJ0xhlDudvOvNVMO8Sybw5jch/ovS3D+e70jcYHlRBAk7el6h
2FONrylmbUfJCWdq3BX6d+d4HDNF0lXSlcxI2u2faTGa5tGpf/8wSEB9tgTzbCywt7IOF2
1gyXwGcY0SWr5FvUIkunEw7lVZ2zAGcaClq296RgkL/PXykukAAADBAP7SKyQNgrilfzze
TNMeb9XHjkWlAHJzcfWKAQNevnNorrtSV+/OlokzehcRHA/c77RSAV1FYAnyOR7s9k5W5E
I1hMsrp+AgpAyC0ROcEZNl0Ai0VGvoW893jZLMOFnCyyXOgWJzDhqhxnVnbqTor0iH3gKN
esUmKcpxUjeuWh24XDx0LjXS5aVCMwna6NrmhalX1fxJV2qxdyw9GHb0+uFygExCIa6Iq6
hgRx53Pxe3WlU16vZpn5SZnUhFkB3GRQAAAMEA6/ZEaqV9Xk7e+09vcl20/HX+M27EYlWQ
E5AkM0D129XP56SRGOmtE4LwwJkymZ4EiA5SL2gsY80mh6xYlQXqAmQwKyfqqfpqgmWgNG
GXnnMU7SIF/ZNxe1+x5EcPR82jptAD5jVPBaGoJKJI/ZcCDs97OPYqzLLg3/VTajId2OdJ
1h0rABy7vnH6kh9qQVcXDU75pUvQ+NzP8Kd5mD9muyQmkfo7QJks9emc0hoo1kZ+ncTCT/
HoEQ0BuIYclSaHAAAAB2pvZWxAdm0BAgM=
-----END OPENSSH PRIVATE KEY-----
root@640aa6d0dea4:~# 
```

>用户文件名
>
![图 27](../assets/images/344dc62beb5c7303ca39e1507e0d930425fa36cbdfeb9ae6f444209a2edd345c.png)  
![图 28](../assets/images/be46a46afa73364318cd1499351f5e1a847e8ff8c4cd378437264f785cc85d15.png)  
![图 29](../assets/images/0da47ea1f938df1cc236354579e593dd60e16cbd783ff4e72f0e10be34ed9442.png)  
![图 30](../assets/images/2a3aed838b59883cbf32945ce116e8c9511eef5c9664856bc7f80ddc5eba273f.png)  
![图 31](../assets/images/c3dd5e0eb9a0cc96a4c1b3427d4d4b46c36b988587b531b90be10c84626b659e.png)  
![图 32](../assets/images/9f40e4364982442f515a8664e7a75e4ec121447b8013eecb6149af98e61f7311.png)  

>到这里就结束了
>
>userflag:50dad8a7adbad3d44d1e3c38a92ae23d
>
>rootflag:7b37f653ac247107d990d55de68dc071
>