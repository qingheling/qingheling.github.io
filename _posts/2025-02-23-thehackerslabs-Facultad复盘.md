---
title: thehackerslabs Facultad靶机复盘
author: LingMj
data: 2025-02-23
categories: [thehackerslabs]
tags: [wordpress,hash,group]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:0a:12:7f       (Unknown)
192.168.56.161  08:00:27:a9:9e:c0       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.886 seconds (135.74 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.161
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-22 19:30 EST
Nmap scan report for 192.168.56.161
Host is up (0.0024s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 af:79:a1:39:80:45:fb:b7:cb:86:fd:8b:62:69:4a:64 (ECDSA)
|_  256 6d:d4:9d:ac:0b:f0:a1:88:66:b4:ff:f6:42:bb:f2:e5 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Asignatura: Administraci\xC3\xB3n de Sistemas - Ingenier\xC3\xADa Inform\xC3\xA1...
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 08:00:27:A9:9E:C0 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 63.50 seconds
```

## 获取webshell
![图 0](../assets/images/51e75762e4c450e79ae475f16a9b5f5f0eb509e193dc298ae23011d9a2636bd7.png)  
![图 1](../assets/images/89713cbf91cf6208757f52c41a1d7672d5c8d23f56180670b42da96f60e61fee.png)  
![图 2](../assets/images/46cfc1260b0725c0f8b113cb2a9b5393011e32cecb31ed078de91a6e4e25140e.png)  
![图 3](../assets/images/8697badd7e67afa4566960e76c41048743fb07c4202a8008997bcdeafb76855b.png)  
![图 4](../assets/images/48baf014ed8944474c9d28fb353ef8774f08ed41f7316117745c44225133ba85.png)  

>感觉要么找信息要么爆破了，边爆破边找信息吧
>

![图 5](../assets/images/82605ce0e101cd07a88d7d7b1581af910d914f1551bec74427b1735ec2d3df3c.png)  
![图 6](../assets/images/f03d74bd82b8169db08613ac6bf13e8e9684db01f2fc80d63b87efc55015627a.png)  
![图 7](../assets/images/d870fdf655ba6302e4a1c67df98a92ca95dbf09cf102de374a27bbcbf4d42ab7.png)  
![图 8](../assets/images/e83b8b9555ef1696476b61e63cc62be1c6c24237a8002398f5ab674636f345d3.png)  

>又是wordpress算了，直接找wordpress吧
>

![图 9](../assets/images/e697a93533da368048fb3594b81385542f7fefb39f4bc37b7dcb44b6e3e365f0.png)  


>目前我没看到插件奥，也不知道跟插件有没有关系
>

![图 10](../assets/images/d0e565cb1ec13c9d7e77bc2a02e28b8fbbaedebf57b7ffcf3950771d7d35a864.png)  
![图 11](../assets/images/8ef9e265dee8ba3852e992cfea7440dd9e7a16e3203ff98694da8b16b9a84782.png)  
![图 12](../assets/images/eee544a3be36b285a9c9eb39cc8d8e4b130ba88b11775dd1a4930eea6b61653f.png)  

>存在域名奥，账号密码:facultad / asdfghjkl
>

![图 13](../assets/images/b4d49f7d09880a07885691bd18c2a80668cdc3ce10066c855e10befb131510de.png)
![图 15](../assets/images/cc7b0a1677c03c7b6ddabc7bedaea7cc902eb4f4c95231c4cc5037af10b65a06.png)  
![图 14](../assets/images/d72775c19b9547a5f3b8b2266cf5b65aef20776deb08dad2cafc1aeb4afa96fc.png)  

![图 16](../assets/images/f0e614c155ff94bf5818515c9faef4f59bcb091ba0ff653dd05f2982adf59bf3.png)  
![图 17](../assets/images/d85dde936cd208f2aa4a11b9926528b98533d0f0e87ad058b16d6308770555ad.png)  
![图 18](../assets/images/b33757994a4bc6ba289a9c4a04c932bf0eba917a0dbd326554b9d5285fbec44b.png)  
![图 19](../assets/images/049e75251423eab82ac33784696ba8b48a9ed390c67acf95746ab20f3ba1d9d8.png)  
![图 20](../assets/images/5df63c6fa08b68ce07aa005c68118f53140a9ef2c4c0eaf31bebd624d07a5133.png)  
![图 21](../assets/images/0e19ad2fd32c75c05d8596e6394cea06219518c694a7b2b7c5ccafc11cb20a87.png)  

>简单可以直接下一步了
>
![图 22](../assets/images/09fc06649b77823e1fbc5aaa169971457fb139a865ce7be93bbc9f0b90fa78e9.png)  
![图 23](../assets/images/27d0fc80d77d852245fa01c43fd5e7d4b97b0a7ffaa4760735c4e48b4e9873ca.png)  

## 提权
![图 24](../assets/images/d92f51c96dbd5c13a4aed5652dd41419333298a6ec6be786dde87b34f2faedbd.png)  

```
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| wordpress          |
+--------------------+
2 rows in set (0.001 sec)

MariaDB [(none)]> use wordpress
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [wordpress]> show table;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '' at line 1
MariaDB [wordpress]> show tables
    -> ;
+-----------------------+
| Tables_in_wordpress   |
+-----------------------+
| wp_commentmeta        |
| wp_comments           |
| wp_links              |
| wp_options            |
| wp_postmeta           |
| wp_posts              |
| wp_term_relationships |
| wp_term_taxonomy      |
| wp_termmeta           |
| wp_terms              |
| wp_usermeta           |
| wp_users              |
| wp_wpfm_backup        |
+-----------------------+
13 rows in set (0.001 sec)

MariaDB [wordpress]> select * from wp_users;
+----+------------+------------------------------------+---------------+------------+-------------------------------+---------------------+---------------------+-------------+--------------+
| ID | user_login | user_pass                          | user_nicename | user_email | user_url                      | user_registered     | user_activation_key | user_status | display_name |
+----+------------+------------------------------------+---------------+------------+-------------------------------+---------------------+---------------------+-------------+--------------+
|  1 | Facultad   | $P$BErY/zc8BR4TJBrKLWcxOwJuU6UrOY/ | facultad      | a@a.com    | http://facultad.thl/education | 2025-01-27 10:25:10 |                     |           0 | Facultad     |
+----+------------+------------------------------------+---------------+------------+-------------------------------+---------------------+---------------------+-------------+--------------+
1 row in set (0.000 sec)

MariaDB [wordpress]> 
```


![图 25](../assets/images/132438d74fb0f03b5c4ec12729537ae6a7fa53413f25d76f38febe9436f519b9.png)  
![图 26](../assets/images/85220b80fe41d85557a2ff762e414b6821ea7179f5239c301688e44a90e63f9b.png)  
![图 27](../assets/images/7d80864342cc26ba603a8e68a6d93b1806e39a6b79d96e84a3c2cb5090604db5.png)  
![图 28](../assets/images/5cbf26b861e788a15f777d433bd78ea57ff5133bdde29cb570256482d3add8d7.png)  
![图 29](../assets/images/ffe55c8f28fdcf7b8456c728f2e1df7c7050ee2570aedb092d1d98832bbfb2f4.png)  


```
gabri@TheHackersLabs-facultad:/var/mail/gabri$ ls -al
total 12
drwxr-sr-x 2 gabri gabri 4096 Jan 25 23:05 .
drwxrwsr-x 4 root  mail  4096 Jan 25 22:56 ..
-rw-r--r-- 1 gabri gabri  176 Jan 25 22:56 .password_vivian.bf
gabri@TheHackersLabs-facultad:/var/mail/gabri$ cat .password_vivian.bf 
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>++++++++.-----------.+++++++++++++++.---------------.+++++++++++++++++++.--.---.-.-------------.<<++++++++++++++++++++.--.++.+++.
gabri@TheHackersLabs-facultad:/var/mail/gabri$ 
```

>突然忘了是啥加密了去一个特定网站看看或者查查
>

![图 30](../assets/images/c8a621d27ca38406c9876926b7fdbbfc4b0490e8c91405b7bbf5613eb92b0604.png)  
![图 31](../assets/images/77bc41d89440bffa36d2b434f5cedb425a034eb4a0151755ac6613365832ad99.png)  

>之前有提示但是不知道是不是这个密码：lapatrona2025，无法su直接ssh
>

![图 32](../assets/images/61030020edc0d42d2b04f6c3f9f24adc3d110b4d021574b4e2bd5318087a0da3.png)  


```
    (ALL) NOPASSWD: /opt/vivian/script.sh
vivian@TheHackersLabs-facultad:~$ ls -al /opt/vivian/script.sh
-rwxr-xr-x 1 vivian vivian 58 ene 27 22:34 /opt/vivian/script.sh
vivian@TheHackersLabs-facultad:~$ cat /opt/vivian/script.sh
#!/bin/bash
echo "Ejecutado como vivian para mis alumnos"
vivian@TheHackersLabs-facultad:~$ 
```

>这里有有点没找到东西如果环境变量劫持的话可是sudo应该做不了直接工具跑一下，工具跑完没见啥，看来我目前得手动找线索了
>

![图 33](../assets/images/66d56491e1837d0c8145dc97dd4e457c25fb98e885dd97d27c6a54fd2ad87ffa.png)  

>能拦截的改这个文件么？但是他是easy靶机不应该是这个方案奥
>

![图 34](../assets/images/b6e9d097e9ccc1dc0409624cb4d3064ad4e1d5404298d85fe9b32e8e04c17f74.png)  

>试试王炸方案
>

![图 35](../assets/images/f003ba4a2f1bf17d11a0061fbb9484a58c5e22661033caf3ed6d2453e094a84d.png)  

![图 36](../assets/images/5eb41c510c09060d3e5f735e6ddb12794a509c1d5a1918347e9fba1cf0a97414.png)  

>好了结束了，我以为2条路呢才发现是一个方案，结束
>



>userflag:mcxvbniou345897hjbhjzx
>
>rootflag:nbfgjyui4r57834sdbhjcvhz
>
