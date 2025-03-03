---
title: VulNyx Arpon靶机复盘
author: LingMj
data: 2025-01-20
categories: [VulNyx]
tags: [upload.hash,docker]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.186  00:0c:29:53:a9:b0       VMware, Inc.
192.168.26.254  00:50:56:e8:96:d1       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.536 seconds (100.95 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.186       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-20 07:15 EST
Nmap scan report for 192.168.26.186 (192.168.26.186)
Host is up (0.0015s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 e1:85:8b:7b:6d:a2:6b:1a:ed:18:8e:08:a0:90:87:2a (ECDSA)
|_  256 ad:fe:77:78:a0:57:70:cc:33:68:b5:84:26:a3:b3:63 (ED25519)
80/tcp open  http    Apache httpd 2.4.59 ((Debian))
|_http-server-header: Apache/2.4.59 (Debian)
|_http-title: Essex
MAC Address: 00:0C:29:53:A9:B0 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 78.85 seconds
```

## 获取webshell

![图 0](../assets/images/5cec3dac9d6e8ffe1c362523eeec4d5190a1e86b95a7f9b8b9acca7139186d6a.png)  
![图 2](../assets/images/e3282b99c54e4946e4bd4605a26fe534bb4e034d658f58e63e6729dd5ecf44f7.png)  

>没什么看点爆破一下，思路有了
>
![图 1](../assets/images/8520a58af204ccb58569e71f64590f4c84915c56710e6101a8dbed93f5a023ff.png)  
![图 3](../assets/images/2a4417041548adba849779336fbc93ba763efbdf84e4c1b180178ac7f09d3622.png)  

>有上传，研究上传
>
![图 4](../assets/images/fb78fadc4d1da1c21de475376303c0b06a78353e6b5b4a7b3ee92c8063dfc9c9.png)  

>这个靶机可以秒了，哈哈哈，不过没看出上传点在那
>
![图 5](../assets/images/160d6e92296dcad223ef225c6b355516cc3989f1eced987c0a41f7462d05bdd7.png)  
![图 6](../assets/images/8c25e5814f05c6f36802e0d227143eed58f1f3454f59a2f34443d4454947dfc6.png)  
![图 7](../assets/images/5970a2e9d8147b950a12306fcf5578601a62ee0269781b50767eaed824a71f8e.png)  
![图 8](../assets/images/10a5c4e188e51b9863a9c5be9b751e4ed4ee72e57c766bbab0ec4be62154535e.png)  
![图 10](../assets/images/9821ab9845052d111357ff4abb566fcfb4e6c790a26f7b07ff3990cda0ce2975.png)  

>统统不见，说早了，没路径秒不了
>
 ![图 11](../assets/images/fa791e26e7581886088d8eda4c4207ba02360b1a4c7a3aed9a875f8c0b6c3d17.png)  
![图 12](../assets/images/96e59a08bd4a428827b1de85e9e7b0c20d27a648d10c22f99de9885de39b970b.png)  

![图 13](../assets/images/3c70e0f3da730801f12ac0b950631e6425ad37a0005c7c7005d308b596f1cdd3.png)  
![图 14](../assets/images/7e005f8c3a7b9600d9b1b3c94cd633bb3f30b0df019a72c39635bfc01e82289f.png)  

>还是挺简单的只不过还是得目录扫全
>

## 提权
```
www-data@arpon:/var/www/html/backup/empty$ ls -al
total 24
drwxr-xr-x 3 www-data www-data 4096 Jan 20 13:27 .
drwxr-xr-x 3 www-data www-data 4096 May 12  2024 ..
drwxr-xr-x 2 www-data www-data 4096 May 13  2024 .hidden
-rw-r--r-- 1 www-data www-data   25 Jan 20 13:26 eval.html
-rw-r--r-- 1 www-data www-data   25 Jan 20 13:27 eval.phar
-rw-r--r-- 1 www-data www-data    1 May 12  2024 index.html
www-data@arpon:/var/www/html/backup/empty$ sudo -l
[sudo] password for www-data: 
sudo: a password is required
www-data@arpon:/var/www/html/backup/empty$ cd ..
www-data@arpon:/var/www/html/backup$ ls -al
total 20
drwxr-xr-x 3 www-data www-data 4096 May 12  2024 .
drwxr-xr-x 4 root     root     4096 May 13  2024 ..
drwxr-xr-x 3 www-data www-data 4096 Jan 20 13:27 empty
-rw-r--r-- 1 www-data www-data  421 May 12  2024 index.html
-rw-r--r-- 1 www-data www-data  919 May 12  2024 upload.php
www-data@arpon:/var/www/html/backup$ cd ..
www-data@arpon:/var/www/html$ ls -al
total 24
drwxr-xr-x 4 root     root     4096 May 13  2024 .
drwxr-xr-x 3 root     root     4096 May 12  2024 ..
drwxr-xr-x 3 www-data www-data 4096 May 12  2024 backup
drwxr-xr-x 2 root     root     4096 May 13  2024 imagenes
-rw-r--r-- 1 root     root     2447 May 13  2024 index.html
-rw-r--r-- 1 root     root       20 May 12  2024 index.php
```

```
www-data@arpon:/var/www/html/backup/empty$ cd /home/
www-data@arpon:/home$ ls -al
total 16
drwxr-xr-x  4 root      root      4096 May 14  2024 .
drwxr-xr-x 18 root      root      4096 May 11  2024 ..
drwx------  3 calabrote calabrote 4096 May 12  2024 calabrote
drwx------  5 foque     foque     4096 May 13  2024 foque
www-data@arpon:/home$ cd /opt/
www-data@arpon:/opt$ ls -al
total 12
drwxr-xr-x  3 root root 4096 May 13  2024 .
drwxr-xr-x 18 root root 4096 May 11  2024 ..
drwx--x--x  4 root root 4096 May 13  2024 containerd
www-data@arpon:/opt$ cdco
bash: cdco: command not found
www-data@arpon:/opt$ cd containerd/
www-data@arpon:/opt/containerd$ ls a-l
ls: cannot access 'a-l': No such file or directory
www-data@arpon:/opt/containerd$ ls -al
ls: cannot open directory '.': Permission denied
www-data@arpon:/opt/containerd$ ls -al
ls: cannot open directory '.': Permission denied
www-data@arpon:/opt/containerd$ cd ..
www-data@arpon:/opt$ ls -al
total 12
drwxr-xr-x  3 root root 4096 May 13  2024 .
drwxr-xr-x 18 root root 4096 May 11  2024 ..
drwx--x--x  4 root root 4096 May 13  2024 containerd
www-data@arpon:/opt$ cd /var/backups/
www-data@arpon:/var/backups$ ls -al
total 432
drwxr-xr-x  2 root root   4096 May 14  2024 .
drwxr-xr-x 12 root root   4096 May 12  2024 ..
-rw-r--r--  1 root root  40960 May 14  2024 alternatives.tar.0
-rw-r--r--  1 root root   9716 May 13  2024 apt.extended_states.0
-rw-r--r--  1 root root    943 May 12  2024 apt.extended_states.1.gz
-rw-r--r--  1 root root      0 May 14  2024 dpkg.arch.0
-rw-r--r--  1 root root    186 May 11  2024 dpkg.diversions.0
-rw-r--r--  1 root root    172 May 12  2024 dpkg.statoverride.0
-rw-r--r--  1 root root 368486 May 13  2024 dpkg.status.0
www-data@arpon:/var/backups$ 
```
![图 15](../assets/images/8f64e5732b7a293218ef7e240d1f5424a63a8583c27ac4ccd34cbd420059e3fe.png)  
![图 16](../assets/images/701df82b83fe1a47ddf85c294f6676c85679c09b99eea2cc8fc822dcd2d63fa2.png)  
![图 17](../assets/images/de520fd286af9a72c3f263e37fa6f8e1edb353e36d47157aa34ce7ea334f0a89.png)  
![图 18](../assets/images/04cfbe5618d3da35bd810392f399d753eea6bcefbf24b11091eab14512255e15.png)  
![图 19](../assets/images/daac0ad594debe8e5b29859c1f064a798baf35e3a13b92a76ab3d4200d18c57d.png)  

>差不多了
>

```
└─# zip2john a.zip > tmp
ver 2.0 efh 5455 efh 7875 a.zip/id_rsa_calabrote PKZIP Encr: TS_chk, cmplen=2106, decmplen=3369, crc=30838030 ts=B802 cs=b802 type=8
ver 2.0 efh 5455 efh 7875 a.zip/id_rsa_calabrote.pub PKZIP Encr: TS_chk, cmplen=602, decmplen=735, crc=155F3DD3 ts=B802 cs=b802 type=8
NOTE: It is assumed that all files in each archive have the same password.
If that is not the case, the hash may be uncrackable. To avoid this, use
option -o to pick a file at a time.
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# john tmp --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
swordfish        (a.zip)     
1g 0:00:00:00 DONE (2025-01-20 07:52) 20.00g/s 81920p/s 81920c/s 68266C/s 123456..oooooo
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# unzip a.zip
Archive:  a.zip
[a.zip] id_rsa_calabrote password: 
  inflating: id_rsa_calabrote        
  inflating: id_rsa_calabrote.pub 
```
![图 20](../assets/images/795803b2b447e2149fff8856358e42fb8fb586d6a702c76f06e143c4ca231b26.png)  
![图 21](../assets/images/95631d8625c154258a543f2b75054079d9890f2b79876f8fe62f8d573ab27cfa.png)  
![图 22](../assets/images/130a4f95586fc52d805767dcf937f6777f5c9e1bee97c1c8bef61c8d3d2ac498.png)  

>竟然没考docker
>
![图 23](../assets/images/f6942cb59a7e7d5af263e0c41f4f34002ea76b05b15e81e081ef6bbc1455a532.png)  

>按理来说已经完事了，不过我试一下提权
>
![图 24](../assets/images/a86aea5e53a679928f9998635b32346cbfaaa772a94ad72157afd10f285f3a3b.png)  
![图 25](../assets/images/8da516f0d859fa5919a9f1f1bc961d292a6ef964a836fddc4168e90fbc5bdd18.png)  

>保留再议
>


>userflag:4ce7368ace8130a6df2b47080dcdc16c
>
>rootflag:69db9f78edf072e03870a53b90aff647
>






























