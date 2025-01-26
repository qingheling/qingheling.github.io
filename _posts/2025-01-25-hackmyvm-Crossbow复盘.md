---
title: hackmyvm Crossbow靶机复盘
author: LingMj
data: 2025-01-25
categories: [hackmyvm]
tags: [ssh-agent,snefru,ansible]
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
192.168.56.100  08:00:27:63:b7:be       (Unknown)
192.168.56.117  08:00:27:36:67:ef       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.954 seconds (131.01 hosts/sec). 3 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.56.117
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-25 03:19 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.117
Host is up (0.00087s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 9.2p1 Debian 2+deb12u1 (protocol 2.0)
| ssh-hostkey: 
|   256 dd:83:da:cb:45:d3:a8:ea:c6:be:19:03:45:76:43:8c (ECDSA)
|_  256 e5:5f:7f:25:aa:c0:18:04:c4:46:98:b3:5d:a5:2b:48 (ED25519)
80/tcp   open  http        Apache httpd 2.4.57 ((Debian))
|_http-server-header: Apache/2.4.57 (Debian)
|_http-title: Polo's Adventures
9090/tcp open  zeus-admin?
| fingerprint-strings: 
|   GetRequest, HTTPOptions: 
|     HTTP/1.1 400 Bad request
|     Content-Type: text/html; charset=utf8
|     Transfer-Encoding: chunked
|     X-DNS-Prefetch-Control: off
|     Referrer-Policy: no-referrer
|     X-Content-Type-Options: nosniff
|     Cross-Origin-Resource-Policy: same-origin
|     X-Frame-Options: sameorigin
|     <!DOCTYPE html>
|     <html>
|     <head>
|     <title>
|     request
|     </title>
|     <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
|     <meta name="viewport" content="width=device-width, initial-scale=1.0">
|     <style>
|     body {
|     margin: 0;
|     font-family: "RedHatDisplay", "Open Sans", Helvetica, Arial, sans-serif;
|     font-size: 12px;
|     line-height: 1.66666667;
|     color: #333333;
|     background-color: #f5f5f5;
|     border: 0;
|     vertical-align: middle;
|_    font-weight: 300;
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port9090-TCP:V=7.94SVN%I=7%D=1/25%Time=67949ED8%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,DB1,"HTTP/1\.1\x20400\x20Bad\x20request\r\nContent-Type:\x2
SF:0text/html;\x20charset=utf8\r\nTransfer-Encoding:\x20chunked\r\nX-DNS-P
SF:refetch-Control:\x20off\r\nReferrer-Policy:\x20no-referrer\r\nX-Content
SF:-Type-Options:\x20nosniff\r\nCross-Origin-Resource-Policy:\x20same-orig
SF:in\r\nX-Frame-Options:\x20sameorigin\r\n\r\n29\r\n<!DOCTYPE\x20html>\n<
SF:html>\n<head>\n\x20\x20\x20\x20<title>\r\nb\r\nBad\x20request\r\nc2c\r\
SF:n</title>\n\x20\x20\x20\x20<meta\x20http-equiv=\"Content-Type\"\x20cont
SF:ent=\"text/html;\x20charset=utf-8\">\n\x20\x20\x20\x20<meta\x20name=\"v
SF:iewport\"\x20content=\"width=device-width,\x20initial-scale=1\.0\">\n\x
SF:20\x20\x20\x20<style>\n\tbody\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20margin:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20font-family:\x20\"RedHatDisplay\",\x20\"Open\x20Sans\",\x20Helveti
SF:ca,\x20Arial,\x20sans-serif;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20font-size:\x2012px;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20line-height:\x201\.66666667;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20
SF:\x20\x20\x20color:\x20#333333;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20\x20background-color:\x20#f5f5f5;\n\x20\x20\x20\x20\x20\x20\x20\x2
SF:0}\n\x20\x20\x20\x20\x20\x20\x20\x20img\x20{\n\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20\x20\x20border:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20\x20\x20vertical-align:\x20middle;\n\x20\x20\x20\x20\x20\x20\x20
SF:\x20}\n\x20\x20\x20\x20\x20\x20\x20\x20h1\x20{\n\x20\x20\x20\x20\x20\x2
SF:0\x20\x20\x20\x20\x20\x20font-weight:\x20300;\n\x20\x20\x20\x20\x20\x20
SF:\x20\x20}\n\x20\x20\x20\x20\x20\x20\x20\x20p\x20")%r(HTTPOptions,DB1,"H
SF:TTP/1\.1\x20400\x20Bad\x20request\r\nContent-Type:\x20text/html;\x20cha
SF:rset=utf8\r\nTransfer-Encoding:\x20chunked\r\nX-DNS-Prefetch-Control:\x
SF:20off\r\nReferrer-Policy:\x20no-referrer\r\nX-Content-Type-Options:\x20
SF:nosniff\r\nCross-Origin-Resource-Policy:\x20same-origin\r\nX-Frame-Opti
SF:ons:\x20sameorigin\r\n\r\n29\r\n<!DOCTYPE\x20html>\n<html>\n<head>\n\x2
SF:0\x20\x20\x20<title>\r\nb\r\nBad\x20request\r\nc2c\r\n</title>\n\x20\x2
SF:0\x20\x20<meta\x20http-equiv=\"Content-Type\"\x20content=\"text/html;\x
SF:20charset=utf-8\">\n\x20\x20\x20\x20<meta\x20name=\"viewport\"\x20conte
SF:nt=\"width=device-width,\x20initial-scale=1\.0\">\n\x20\x20\x20\x20<sty
SF:le>\n\tbody\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20margi
SF:n:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20font-family:\
SF:x20\"RedHatDisplay\",\x20\"Open\x20Sans\",\x20Helvetica,\x20Arial,\x20s
SF:ans-serif;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20font-size:\
SF:x2012px;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20line-height:\
SF:x201\.66666667;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20color:
SF:\x20#333333;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20backgroun
SF:d-color:\x20#f5f5f5;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\x20\x20\x20\x
SF:20\x20\x20\x20\x20img\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x2
SF:0\x20border:\x200;\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20ver
SF:tical-align:\x20middle;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\x20\x20\x2
SF:0\x20\x20\x20\x20\x20h1\x20{\n\x20\x20\x20\x20\x20\x20\x20\x20\x20\x20\
SF:x20\x20font-weight:\x20300;\n\x20\x20\x20\x20\x20\x20\x20\x20}\n\x20\x2
SF:0\x20\x20\x20\x20\x20\x20p\x20");
MAC Address: 08:00:27:36:67:EF (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 220.07 seconds
```

## 获取webshell
>存在80和9090端口，上web看一眼
>

![图 0](../assets/images/30cf01e793b62d62418fcf1c562bab514d78f62ca8e73ea70a26dcd1f3b049c8.png)  
![图 1](../assets/images/0318d7db9b7395246ee709d3765dbec3e61cfc47a1dd61eee3864cd9cf79ee12.png)  
![图 2](../assets/images/d804c61de3169f969e0faab41968afef0f3c2ea1658097378f6220e57a4d08a5.png)  
![图 3](../assets/images/5891c270a10c4e7b92d25c5e767493d5e6cb4619c97a3fa974af03e1bae86dad.png)  
![图 4](../assets/images/11e4f206837e98f52f51f38aaa0ec16d819705ae9505edcbac7a080478170fc9.png)  
![图 5](../assets/images/83c8769945181977aae49d146d7aa4477db7531452e696fff2c2a2672e239b88.png)  
![图 6](../assets/images/6d0a909bdebef976e8f628d7a154159ead21fd6673d1b346988b24faf943ff0c.png)  

>爆破无果
>
![图 7](../assets/images/ac3413f39df16d4521c7fde61e758ff6f516b5be475e0c4892bf68e14a962562.png)  
![图 8](../assets/images/2c30ab22ec064fbb55a3c876e1f9b89781411d24b9170ea9a83ed4779f67eb78.png)  
![图 9](../assets/images/25706f597038b80f0b6c5cc385eb6658f6e92fac4343b8e165d5fe3ade0c3636.png)  
![图 10](../assets/images/fe8f1e99cb456eef394c49fc98a5d5dfb4295067698cc23b8b7c1aa7776382d2.png)  
![图 11](../assets/images/aeff7ec87e31559fca0ab026b475c003a169d9e9f18d824e68fa2fc9a74ea785.png)  
![图 12](../assets/images/83a7d40ee3c9acec1aebdd671a135ebc3087fdbd5e2e2973d87f9a6ab32f6ad5.png)  
![图 13](../assets/images/69455d7192cd716ca7470ce7e6b4546c07bfea304e3de9fe23a0e204df492001.png)  

>到这里我们发现存在域名，phishing.crossbow.hmv
>
![图 14](../assets/images/e8522187d32914b1000c71e5daaae5b3fa928123e545fbd7a63f2fdf8c4a4883.png)  
>扫描无果，继续扫描子域名，浪费了一段时间回归网页的js，里面的key可以试着解开
>
![图 15](../assets/images/bcd120ff92ffa0bf39cee206bf96ea9da65cd6c555de04f9d6202c3fcef5d863.png)  
![图 16](../assets/images/adaa5a682508c19844f19cc5754d4084da79aad1a645fc45502f3a4e88628f83.png)  

>找到的解密网址：https://md5hashing.net/
>

![图 17](../assets/images/614789615fc097c226ab9b0415193e604ba42aedf6f5ed27b2c89c7620a16b68.png)  
![图 18](../assets/images/ae3220ce267cdad40f0a2ebf0f69827f1951e57a2e4ebcf9e01c7d9c27004dab.png)  

>记得存在的登录页面，但是不知道用户名，不是admin，可以试主界面的polo
>
![图 19](../assets/images/393be28fecf66685728b2b9debcadf86e04181aefb5740a767290dab86d31c89.png)  
![图 20](../assets/images/12d3edf9a329eed14ed13e2782567fc1d952a7c1cbeffbaf440b40a12a8d7077.png)  
![图 21](../assets/images/168f09bb2d2e401ea21668badfdfdfa06eed1cfe97f36b17863b1306b16f6809.png)  


## 提权
```
polo@crossbow:~$ ls -al
total 48
drwx------ 1 polo polo 4096 Sep 16  2023 .
drwxr-xr-x 1 root root 4096 Sep 18  2023 ..
lrwxrwxrwx 1 root root    9 Sep  5  2023 .bash_history -> /dev/null
-rw-r--r-- 1 polo polo  220 Sep  3  2023 .bash_logout
-rw-r--r-- 1 polo polo 3527 Sep 16  2023 .bashrc
drwx------ 2 polo polo 4096 Sep 15  2023 .cache
drwx------ 3 polo polo 4096 Sep 16  2023 .gnupg
drwxr-xr-x 3 polo polo 4096 Sep 16  2023 .local
-rw-r--r-- 1 polo polo  807 Sep  3  2023 .profile
drwx------ 1 root root 4096 Sep  3  2023 .ssh
polo@crossbow:~$ sudo -l
[sudo] password for polo: 
Sorry, user polo may not run sudo on crossbow.
polo@crossbow:~$ 
```

```
polo@crossbow:/home$ ls -al
total 28
drwxr-xr-x 1 root root 4096 Sep 18  2023 .
drwxr-xr-x 1 root root 4096 Dec 14  2023 ..
drwxr-xr-x 1 lea  lea  4096 Sep 18  2023 lea
drwx------ 1 polo polo 4096 Sep 16  2023 polo
polo@crossbow:/home$ cd lea/
polo@crossbow:/home/lea$ ls a-l
ls: cannot access 'a-l': No such file or directory
polo@crossbow:/home/lea$ ls -al
total 48
drwxr-xr-x 1 lea  lea  4096 Sep 18  2023 .
drwxr-xr-x 1 root root 4096 Sep 18  2023 ..
lrwxrwxrwx 1 root root    9 Sep  5  2023 .bash_history -> /dev/null
-rw-r--r-- 1 lea  lea   220 Apr 23  2023 .bash_logout
-rw-r--r-- 1 lea  lea  3527 Sep 18  2023 .bashrc
drwx------ 2 lea  lea  4096 Sep 18  2023 .keychain
drwxr-xr-x 1 lea  lea  4096 Dec 14  2023 .local
-rw-r--r-- 1 lea  lea   807 Apr 23  2023 .profile
drwx------ 1 lea  lea  4096 Dec 14  2023 .ssh
polo@crossbow:/home/lea$ 
```

```
polo@crossbow:/var$ ss -lnput
Netid                State                 Recv-Q                Send-Q                               Local Address:Port                                Peer Address:Port                Process                
tcp                  LISTEN                0                     128                                        0.0.0.0:22                                       0.0.0.0:*                                          
tcp                  LISTEN                0                     511                                        0.0.0.0:80                                       0.0.0.0:*                                          
tcp                  LISTEN                0                     10                                               *:9090                                           *:*                                          
tcp                  LISTEN                0                     128                                           [::]:22                                          [::]:*                                          
polo@crossbow:/var$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 08:16 ?        00:00:18 /usr/bin/python3 /usr/bin/supervisord
root           6       1  0 08:16 ?        00:00:00 /bin/sh /usr/sbin/apachectl -D FOREGROUND
root           8       1  0 08:16 ?        00:00:37 /usr/lib/cockpit/cockpit-ws --no-tls
lea           11       1  5 08:16 ?        00:06:52 /bin/bash /home/lea/.local/agent
root          16       6  0 08:16 ?        00:00:00 /usr/sbin/apache2 -D FOREGROUND
root          27       1  0 08:16 ?        00:00:00 /usr/sbin/cron
root          30       1  0 08:16 ?        00:00:00 sshd: /usr/sbin/sshd [listener] 0 of 10-100 startups
lea         1081       1  0 08:16 ?        00:00:00 ssh-agent
www-data 1555777      16  0 09:16 ?        00:00:08 /usr/sbin/apache2 -D FOREGROUND
www-data 2043171      16  0 09:36 ?        00:00:04 /usr/sbin/apache2 -D FOREGROUND
www-data 2043620      16  0 09:36 ?        00:00:04 /usr/sbin/apache2 -D FOREGROUND
www-data 2043621      16  0 09:36 ?        00:00:04 /usr/sbin/apache2 -D FOREGROUND
www-data 2043622      16  0 09:36 ?        00:00:04 /usr/sbin/apache2 -D FOREGROUND
www-data 2192187      16  0 09:41 ?        00:00:03 /usr/sbin/apache2 -D FOREGROUND
www-data 2192978      16  0 09:41 ?        00:00:03 /usr/sbin/apache2 -D FOREGROUND
www-data 2192980      16  0 09:41 ?        00:00:03 /usr/sbin/apache2 -D FOREGROUND
www-data 2495615      16  0 09:55 ?        00:00:00 /usr/sbin/apache2 -D FOREGROUND
www-data 2550880      16  0 09:57 ?        00:00:00 /usr/sbin/apache2 -D FOREGROUND
root     2834847       8  0 10:07 ?        00:00:00 /usr/lib/cockpit/cockpit-session localhost
polo     2834859       1  0 10:07 ?        00:00:00 /usr/bin/ssh-agent
polo     2834868 2834847  0 10:07 ?        00:00:00 cockpit-bridge
polo     2834871 2834868  0 10:07 ?        00:00:00 dbus-daemon --print-address --session
polo     2844414 2834868  0 10:08 pts/0    00:00:00 /bin/bash
polo     2875978 2844414  0 10:09 pts/0    00:00:00 bash
polo     2883198 2875978  0 10:09 pts/0    00:00:00 /usr/bin/script /dev/null -qc /bin/bash
polo     2883200 2883198  0 10:09 pts/1    00:00:00 /bin/bash
polo     2977841 2883200  0 10:13 pts/1    00:00:00 ps -ef
lea      2977842      11  0 10:13 ?        00:00:00 find /tmp -name ssh-* -type d
polo@crossbow:/var$ 
```
>利用工具进行操作一下，没啥线索
>
![图 22](../assets/images/a2313ac25187c74f9edab2930e7cbb6ed02531a921baaa3cdca7b7a74cb60e5c.png)  
![图 23](../assets/images/0c86d6dcd95926c90c28526844c65a3f64b55241a6075f846eb12a47c632caf4.png)  

>用了一下没啥突破没没有读取文件，命令注入不知道怎么注入
>
![图 25](../assets/images/e7b4202d483f1486586dcbfbf8250943828b2b354ef34a17cdcb3ca4a8a978b3.png)  

>这里我才发现他是一个docker，所以使用ssh才会出错，
>
![图 26](../assets/images/49a58a2f22a84c5b2ffd740158cfc23d78f75370ce1b6c7f33fcb2829dcaa8b6.png)  

>可以看到一共三个用户，除了本身都可以进行实验
>
![图 27](../assets/images/1124e3749d42e7d3d0a7ba69bb9af9dbdeb8ca39962963fbb0a0d878933761d7.png)  
![图 28](../assets/images/7153d53976a237bf910a0af03b754ca0f49518ff5bbc273c4af5fd2ed43318d9.png)  

>ok,拿到下一个用户
>
![图 29](../assets/images/01e9adb95eef199aef8809414258fa0c5e54b31962910bddfa6faa6e0474289f.png)  

>存在3306和3000
>

```
╭─pedro@crossbow ~/.gnupg 
╰─$ ls -al
total 20
drwx------ 3 pedro pedro 4096 Sep 16  2023 .
drwx------ 6 pedro pedro 4096 Jan 26 05:12 ..
drwx------ 2 pedro pedro 4096 Sep 16  2023 private-keys-v1.d
-rw------- 1 pedro pedro   32 Sep 16  2023 pubring.kbx
-rw------- 1 pedro pedro 1200 Sep 16  2023 trustdb.gpg
╭─pedro@crossbow ~/.gnupg 
╰─$ cd private-keys-v1.d 
╭─pedro@crossbow ~/.gnupg/private-keys-v1.d 
╰─$ ls -al
total 8
drwx------ 2 pedro pedro 4096 Sep 16  2023 .
drwx------ 3 pedro pedro 4096 Sep 16  2023 ..
╭─pedro@crossbow ~/.gnupg/private-keys-v1.d 
╰─$ sudo -l  
[sudo] password for pedro: 
sudo: a password is required
╭─pedro@crossbow ~/.gnupg/private-keys-v1.d 
╰─$  
```

![图 30](../assets/images/ec9fb9772a7f814dd17994158ddf32dc003d7c4080e008b5b0db729fcf306aae.png)  

>opt 有一个东西，看看怎么利用，没想法直接用工具
>
![图 31](../assets/images/de5a85cb3423a1d28bac3e552a50c51050749de5de883c4361b7b7b4535e4a9a.png)  
![图 32](../assets/images/850a3d367496659466e1baf5ffb264549cbfc64a8fed3850fdd55edb83e23409.png)  

>这个利用点不太懂
>

```
╰─$ ./socat TCP-LISTEN:8080,fork TCP4:127.0.0.1:3000 &  
[1] 148396
╭─pedro@crossbow ~ 
╰─$ ss -lnput
Netid           State            Recv-Q           Send-Q                     Local Address:Port                     Peer Address:Port           Process                                      
udp             UNCONN           0                0                                0.0.0.0:68                            0.0.0.0:*                                                           
tcp             LISTEN           0                128                              0.0.0.0:22                            0.0.0.0:*                                                           
tcp             LISTEN           0                4096                             0.0.0.0:80                            0.0.0.0:*                                                           
tcp             LISTEN           0                4096                           127.0.0.1:3000                          0.0.0.0:*                                                           
tcp             LISTEN           0                80                             127.0.0.1:3306                          0.0.0.0:*                                                           
tcp             LISTEN           0                5                                0.0.0.0:8080                          0.0.0.0:*               users:(("socat",pid=148396,fd=5))           
tcp             LISTEN           0                4096                             0.0.0.0:9090                          0.0.0.0:*                                                           
tcp             LISTEN           0                128                                 [::]:22                               [::]:*                                                           
tcp             LISTEN           0                4096                                [::]:80                               [::]:*                                                           
tcp             LISTEN           0                4096                                [::]:9090                             [::]:*                                                           
╭─pedro@crossbow ~ 
╰─$ 
```

![图 33](../assets/images/68b4407ad3d2ee5e60ca56c84879db8c505f8dad6d076cf84b85fcc3f5546223.png)  
![图 34](../assets/images/796358733f91ff62e1aead42e27246730cca77b17643b94b8191da0582d71642.png)  
![图 35](../assets/images/6ed96b059b4b56332f71dde5fea769263681d0f4aa5e72fc1981354375881792.png)  

>无想法，去看一眼wp，默认的弱口令，我还以为找东西
>
![图 36](../assets/images/f869437e2b069aa9968fc11963294049a2972a36296798cb7daab92b58622e75.png)  

![图 37](../assets/images/ac62110863cbdb48d5273d78450e54cd51037076b752a394bb2dfb04bd01d021.png)  
![图 38](../assets/images/f7b7a97e387327527f9e20c92b1c2b7427a542d725a87b46a540adf02b484ae1.png)  

>这个确实是有漏洞的，可以利用一下，地址：https://www.alevsk.com/2023/07/a-quick-story-of-security-pitfalls-with-execcommand-in-software-integrations/
>

![图 40](../assets/images/b58d53025a0e0b6af194f8ebf6c97cbf233c583c879c29e6e21b9cd57f7e394c.png)  

![图 39](../assets/images/bdc37818c0bceda436b54124ae363b6b5c0dbd0ae7b4eafd5c7c972dc8097091.png)  

![图 41](../assets/images/71b39795e4b25a3c381b27057b4e9293343a6a4cf6081365314558c5e9d6efe6.png)  
![图 42](../assets/images/1674506c5bf551539ac2809a9e0dfdd9e5d70f4931f33b98e6e398fc994aaeb5.png)  
![图 43](../assets/images/68e9034b1ae5bc470ee9d49de1c967bf26259f04ea2d1c65ed56620caaba892c.png)  
![图 44](../assets/images/f541c4e1c48d98aea7876ad9de315c751faaaaf8d24783ea9b63e545f3f728ad.png)  
![图 45](../assets/images/e015cc39bd38d27e4001335ec0e8b468075e4b47e9c6df81942f9b0871962284.png)  
![图 46](../assets/images/bd34a458bd2a18d332eafd6f06a493bdc9cbfe0aa03cd641920b9f950da10ff0.png)  
![图 47](../assets/images/e47e29169ec7c1582883fc14ad5b7edb09f602cbbef3150da34c7dbafb8e7e31.png)  
![图 48](../assets/images/c02cef1c0c6519a999ee5f6d9cd50dec00996cc9b1d023c9a1a5a5e8bde1e88d.png)  

>到这里就结束了
>



>userflag:58cb1e1bdb3a348ddda53f22ee7c1613
>
>rootflag:7a299c41b1daac46d5ab98745b212e09
>