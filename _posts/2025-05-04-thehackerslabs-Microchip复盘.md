---
title: thehackerslabs Microchip靶机复盘
author: LingMj
data: 2025-05-04
categories: [thehackerslabs]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.230	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.077 seconds (123.25 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.230
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-04 05:47 EDT
Nmap scan report for microchip.thl (192.168.137.230)
Host is up (0.0094s latency).
Not shown: 65530 closed tcp ports (reset)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 af:79:a1:39:80:45:fb:b7:cb:86:fd:8b:62:69:4a:64 (ECDSA)
|_  256 6d:d4:9d:ac:0b:f0:a1:88:66:b4:ff:f6:42:bb:f2:e5 (ED25519)
80/tcp    open  http    Apache httpd 2.4.58 ((Ubuntu))
|_http-server-header: Apache/2.4.58 (Ubuntu)
| http-robots.txt: 62 disallowed entries (15 shown)
| /*?order= /*?tag= /*?id_currency= /*?search_query= 
| /*?back= /*?n= /*&order= /*&tag= /*&id_currency= 
| /*&search_query= /*&back= /*&n= /*controller=addresses 
|_/*controller=address /*controller=authentication
| http-title: MicroChip
|_Requested resource was http://microchip.thl/index.php
3306/tcp  open  mysql   MySQL 8.0.42
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=MySQL_Server_8.0.42_Auto_Generated_Server_Certificate
| Not valid before: 2025-04-23T17:42:38
|_Not valid after:  2035-04-21T17:42:38
| mysql-info: 
|   Protocol: 10
|   Version: 8.0.42
|   Thread ID: 10
|   Capabilities flags: 65535
|   Some Capabilities: SupportsTransactions, IgnoreSigpipes, ConnectWithDatabase, Speaks41ProtocolNew, ODBCClient, IgnoreSpaceBeforeParenthesis, LongColumnFlag, Support41Auth, InteractiveClient, SwitchToSSLAfterHandshake, Speaks41ProtocolOld, LongPassword, SupportsCompression, FoundRows, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, SupportsMultipleStatments, SupportsMultipleResults, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: \x01~|LZ/Al,\x01
| }\x11\x13\x7F!*\x08:8
|_  Auth Plugin Name: caching_sha2_password
9000/tcp  open  http    Gophish httpd
|_http-title: Portainer
33060/tcp open  mysqlx  MySQL X protocol listener
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.42 seconds
```

## 获取webshell
![picture 0](../assets/images/550dc9338da1779d0e22abb8d2436f84483fcc701a1ea209178d8ebd27c24b02.png)  
![picture 1](../assets/images/b6fe3873e300929a7822e72518501a9882b60b94b3083bcb9c23f6ec84671982.png)  
![picture 2](../assets/images/7c2d53bcd2802d5a4103f7f071301a4628a0f194ef1da17c7d57a33cf2ea2ce4.png)  
![picture 3](../assets/images/5e22d0f51f98491bcb4d7097bee6ee701c4c1a4c01f2dd14cff811e40607bf04.png)  

>好像有漏洞，继续看看哪里操作
>

![picture 4](../assets/images/1bbb2330c717b4d336e4a2a3d92e59ffb3de1f4909e4ac2858ff16d0d4866b7e.png)  

>这里还是有点可能突破的，不过我试了用户密码都没对
>

![picture 5](../assets/images/5a4ad85ddc8a80de98ae18dd95ace94dd7ff15f8395be85d00162fc10c52b8df.png)  

>这里还有一个用户
>

![picture 6](../assets/images/201fe7c0fe6e1a8a15218e83b466ed3fc49b3f711d5df50c653a4f154203ca88.png)  
![picture 7](../assets/images/c5c5b709d16f81fc35a7bdb88b98030494dd0f638fab68240145e59aaed9b9a9.png)  

>注册一下
>

![picture 8](../assets/images/85f34e46497dabfad2b0c5225370e05de0a16d90f34adcc496c4552a1555f2a4.png)  
![picture 9](../assets/images/6bf40b70304c067a0d13f681c6b3a695f9b3936fb92c9aabee40bcab31eb739f.png)  
![picture 10](../assets/images/5314a62bc00f32a69ade6bbdb8c2fa0c41af10db51d5a7c6ca1bdc507e1d6a75.png)  

>这里会输出pdf跟那个之前一个靶机很像
>

![picture 11](../assets/images/96df18204813230abc2aefd5b2ec9b7867672cb7bacbbd1f48dfd2b8e8b83b8b.png)  
![picture 12](../assets/images/3c089017e5e5fcf165b3398850b5713f7cb91d53f135dfd936fba7eafac49b59.png)  



## 提权



>userflag:
>
>rootflag:
>