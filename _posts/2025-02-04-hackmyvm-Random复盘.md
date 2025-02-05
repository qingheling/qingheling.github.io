---
title: hackmyvm Random靶机复盘
author: LingMj
data: 2025-02-04
categories: [hackmyvm]
tags: [ftp,brute,ssh,elf]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:/home/lingmj/xxoo1# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:cd:6d:19       PCS Systemtechnik GmbH
192.168.56.132  08:00:27:52:21:f4       PCS Systemtechnik GmbH

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.232 seconds (114.70 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj/xxoo1# nmap -p- -sC -sV 192.168.56.132
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 01:02 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.132
Host is up (0.0010s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.56.110
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr--    2 1001     33           4096 Oct 19  2020 html
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 09:0e:11:1f:72:0e:6c:10:18:55:1a:73:a5:4b:e5:64 (RSA)
|   256 c0:9f:66:34:56:1d:16:4a:32:ad:25:0c:8b:a0:1b:5a (ECDSA)
|_  256 4c:95:57:f4:38:a3:ce:ae:f0:e2:a6:d9:71:42:07:c5 (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.14.2
MAC Address: 08:00:27:52:21:F4 (Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.71 seconds
```

## 获取webshell
![图 0](../assets/images/3d199d19b9c4f174c5acd225e6ba787a2663d95464dbf98ab73744ba7da45402.png)  
![图 1](../assets/images/c4f50880101c69c73e15506c18695259b28f10a3540600c537aa966f57669e6b.png)  

>alan好像是用户，但是ssh不能用目前来看，还有一个是eleanor
>
![图 2](../assets/images/09993bfbb663deee8e4afd71b85bad3ad97926bdb69e709623b037256f09118c.png)  

>无其他信息选择爆破密码
>

```
root@LingMj:/home/lingmj# ftp 192.168.56.132  
Connected to 192.168.56.132.
220 (vsFTPd 3.0.3)
Name (192.168.56.132:lingmj): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||36548|)
150 Here comes the directory listing.
drwxr-xr--    2 1001     33           4096 Oct 19  2020 html
226 Directory send OK.
ftp> cd html
550 Failed to change directory.
ftp> get html
local: html remote: html
229 Entering Extended Passive Mode (|||44797|)
550 Failed to open file.
ftp> 
```

>根据提示可以看到alan禁用了eleanor的ssh,所以爆破情况只能爆破ftp
>

![图 3](../assets/images/5fa52991ab1611d7a3293803bc7d45ab52dd3bb188df248f3286f467f7cd4373.png)  
![图 4](../assets/images/429ffff120cb009b0e2e2a588d4892b7ac8c5061b5d36fefb28dd73f5badc947.png)  

>500的位置奥
>


```
root@LingMj:/home/lingmj# ftp 192.168.56.132 
Connected to 192.168.56.132.
220 (vsFTPd 3.0.3)
Name (192.168.56.132:lingmj): eleanor
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -al
229 Entering Extended Passive Mode (|||42238|)
150 Here comes the directory listing.
drwxr-xr-x    3 0        113          4096 Oct 19  2020 .
drwxr-xr-x    3 0        113          4096 Oct 19  2020 ..
drwxr-xr--    2 1001     33           4096 Oct 19  2020 html
226 Directory send OK.
ftp> cd html
250 Directory successfully changed.
ftp> ls -al
229 Entering Extended Passive Mode (|||64346|)
150 Here comes the directory listing.
drwxr-xr--    2 1001     33           4096 Oct 19  2020 .
drwxr-xr-x    3 0        113          4096 Oct 19  2020 ..
-rw-r--r--    1 33       33            185 Oct 19  2020 index.html
226 Directory send OK.
ftp> put rrr
local: rrr remote: rrr
ftp: Can't open `rrr': No such file or directory
ftp> exit
221 Goodbye.
                                                                                                                                                                                                                
root@LingMj:/home/lingmj# cd xxoo1
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1# ftp 192.168.56.132
Connected to 192.168.56.132.
220 (vsFTPd 3.0.3)
Name (192.168.56.132:lingmj): eleanor
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> put wp-load.php
local: wp-load.php remote: wp-load.php
229 Entering Extended Passive Mode (|||40635|)
550 Permission denied.
ftp> ls
229 Entering Extended Passive Mode (|||32808|)
150 Here comes the directory listing.
drwxr-xr--    2 1001     33           4096 Oct 19  2020 html
226 Directory send OK.
ftp> 
```


>不行不能上传只有get，index.html,就是页面没法进行getshell
>

```
root@LingMj:/home/lingmj/xxoo1# ssh eleanor@192.168.56.132
eleanor@192.168.56.132's password: 
This service allows sftp connections only.
Connection to 192.168.56.132 closed.
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1# 
```

>只允许sftp进行服务操作
>

![图 6](../assets/images/aa71e89c33ac0a6d16c290ff697858651a0ae9166717eefd8161be71e5bda72a.png)  

![图 5](../assets/images/f0e4e3e6c4e0bc96152e0bb2522155779b77ba3bb9ca5d7768aa0e0af42e0dd1.png)  

![图 7](../assets/images/be204ebe563e01d5fc289ba2bd86aa4394e56d6440dce6827f7c0bc23ecd4b6f.png)  



## 提权
![图 8](../assets/images/74c376dcfed8a5f08faf580cc8996f07a200cb2ae8dea7f669bdfe20563844b3.png)  

>这样可以登录密码
>

![图 9](../assets/images/734ecf1ad8fa2cf2a2cdeb1afb2146444719ccbdd0c293b02a87635ee4783953.png)  
![图 10](../assets/images/a4facdd5847ac4a4d30f4c872a59d4d6566d4f9ee173f95916946f94ca8f46bc.png)  
![图 11](../assets/images/8523dad57eab619b55307acebfad005af06814adcf1075e314e5b301b12a9bbe.png)  

>有一个suid权限的文件，逆向看看
>

![图 12](../assets/images/a2ededaea045bbc212f02f982293ad6e5614c392c5e4c263359953d961dc35fd.png)  

![图 13](../assets/images/519b5543ffd149375e8b3429ead274f3b1255b9980ab5e85ef57ab3e8888c32b.png)  
![图 14](../assets/images/c2eeac7156472d061eb33870d4506d0bd224d7e38dab7fcb3c49c7dd3bd267a1.png)  
![图 15](../assets/images/fe813e4c711e0440546537f9bcb15e11096c9c3d4f675295952d6c78232c6c1d.png)  


>这里说明了一件是就是当我们猜到他输入的random值就会返回函数内容
>

![图 16](../assets/images/6099195239d4d819b5d98efc9f91e7d45c8dbd6c4c58a21c5b043b03d396e954.png)  

>主机没有这个东西，去靶机测试
>

![图 17](../assets/images/a650cf4547227a3b791ea16356e57390500a8a4e1d2a5de1d84fb1441b8ca7ce.png)  

![图 19](../assets/images/889e5f559c0d28c1b3c2243429f6661206e5d2ec24ac177f79c665b675b97073.png)  


![图 18](../assets/images/c36377fa19ad99050017eb898b6c46cd478d28f852a3adcc6961d5e5c864cf6b.png)  


>1-9范围所以0不行
>

![图 20](../assets/images/7e1798acb35f1de0a8878f567403bff27832b5e5483411a0b3cf93651f126a67.png)  

>找到这个函数的位置改一下，目前是这个思路
>

![图 21](../assets/images/bfc86418fe3a92c6ab3a86dc7d01db2a374ee79175b6f5b0c2f5dc29646e9e9b.png)  
![图 22](../assets/images/9e6c76fe5e409fae95de79017544bf559bf9183c367373c3b0e15072b25e7e46.png)  
![图 23](../assets/images/d6ad78f9c1edad3e16d93c8974ba1d186c3f34030990df57ef047ac62358cc61.png)  

>这个是写好的我应该在其他地方编辑好然后扔进去就你能成功了
>

![图 24](../assets/images/ea7eed19bdf78a8275e59f3eeba3ece8b9b28ed3b3278c47db4c27e46055b2d0.png)  
![图 25](../assets/images/82043678dc4c3628826602a7d669ec06089b9d21a68e939b0438d562b335e52a.png)  
![图 26](../assets/images/735a6c2aeb3a61256a152ee7bcd1b417d14b229e1fd4b1d2786406fb636c3d6c.png)  
![图 27](../assets/images/1bea161dc0c4854af0a238802ac2f4ae537ad43b6bac8967db309b6fcad92fdc.png)  
![图 28](../assets/images/44213345bbeb35276b18188dc8405e30edc5cfd6aaf906b4ce4e2ed7d60c4df1.png)  

>成功了，有意思，到这里就结束了这个靶场
>


>userflag:ihavethapowah
>
>rootflag:howiarrivedhere
>