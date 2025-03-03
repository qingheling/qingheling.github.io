---
title: VulNyx Psymin靶机复盘
author: LingMj
data: 2025-01-21
categories: [VulNyx]
tags: [webmin,file]
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
192.168.26.196  00:0c:29:74:4f:18       (Unknown)
192.168.26.254  00:50:56:e5:dc:17       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.903 seconds (134.52 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.196
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-21 09:05 EST
Nmap scan report for 192.168.26.196 (192.168.26.196)
Host is up (0.0012s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp   open  http        nginx 1.22.1
|_http-title: Welcome to nginx!
|_http-server-header: nginx/1.22.1
3000/tcp open  nagios-nsca Nagios NSCA
MAC Address: 00:0C:29:74:4F:18 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 214.30 seconds
```

## 获取webshell
![图 1](../assets/images/c0f722cee15861fda1b3e89f5aac4cae026e893921007babca7fd6543da63f23.png)  
![图 0](../assets/images/4015ee1f98f0710b0be5fc84b317768803ff86306e72fea5cf87e55cb2d1cdf6.png)  
```
└─# nc 192.168.26.196 3000         
Psy Shell v0.12.4 (PHP 8.2.20 — cli) by Justin Hileman
Unable to check for updates
```
```
> id
id

   Error  Undefined constant "id".

> HELP
HELP

   Error  Undefined constant "HELP".

> ?
?
WARNING: terminal is not fully functional
Press RETURN to continue 

  help       Show a list of commands. Type `help [foo]` for information about [f
oo].      Aliases: ?                     
  ls         List local, instance or class variables, methods and constants.    
          Aliases: dir                   
  dump       Dump an object or primitive.                                       
                                         
  doc        Read the documentation for an object, class, constant, method or pr
operty.   Aliases: rtfm, man             
  show       Show the code for an object, class, constant, method or property.  
                                         
  wtf        Show the backtrace of the most recent exception.                   
          Aliases: last-exception, wtf?  
  whereami   Show where you are in the code.                                    
                                         
  throw-up   Throw an exception or error out of the Psy Shell.                  
                                         
  timeit     Profiles with a timer.                                             
                                         
  trace      Show the current call stack.                                       
                                         
  buffer     Show (or clear) the contents of the code input buffer.             
          Aliases: buf                   
  clear      Clear the Psy Shell screen.                                        
                                         
  edit       Open an external editor. Afterwards, get produced code in input buf
fer.                                     
  sudo       Evaluate PHP code, bypassing visibility restrictions.              
                                         
  history    Show the Psy Shell history.                                        
          Aliases: hist                  
  exit       End the current session and return to caller.                      
          Aliases: quit, q               
> 
```
![图 2](../assets/images/4d288d71a81ea270e7128650114e37bc44fd53bebf89d6728771d9248b70edc8.png)  
![图 3](../assets/images/4177195647a435ba0bae563a81b8cbc7bc463b7cb9aa62a6318455b188d5b1dc.png)  
![图 4](../assets/images/6c8db3d235aa84f194bceb873f30b39cea116760046eea6218379ff32fa5a716.png)  
![图 5](../assets/images/5d5072b938de16df1203a9cd58a81274de96554cc2d36cb934270766107e8bf6.png)  
>地址：https://psysh.org/?source=post_page-----2709cd121255--------------------------------
>
![图 6](../assets/images/7ed3ee7c33e894531881637a2ffb538225e157ea70fdbd25703d739dc8b77839.png)  
![图 7](../assets/images/5b27b29fab22aa944d3245a3e5b861d031f61ae2fbe412fc0515b2dd1d8aa0f2.png)  
![图 8](../assets/images/aeea466e30a4d49ad920568dbf1e6e94c94bf5b4c96490a66be660df84c569b2.png)  
![图 9](../assets/images/2f664bbab56b8b23d267e3e446925e715b79ff0b3cbabd0d4f6a1ee3489af16d.png)  

>跑一下id ssh 密码
>
![图 10](../assets/images/ddd56d96d9920d49be9993fcf5a513e4971465dcdf8abaae76432228494e887d.png)  
![图 11](../assets/images/72ba538ff753470320568d86f4b22cf30251227127a37e1473ae9f540c4faaf4.png)  

## 提权
```
alfred@psymin:~$ id
uid=1000(alfred) gid=1000(alfred) grupos=1000(alfred)
alfred@psymin:~$ sudo -l
-bash: sudo: orden no encontrada
alfred@psymin:~$ 
```
![图 12](../assets/images/46d9a3fc99ce2db7a4f4b1c4c18a6c8553c9392a26d948f885e959a6d390f21d.png)  
```
alfred@psymin:~$ ./socat TCP-LISTEN:8080,fork TCP4:127.0.0.1:10000 &
[1] 1937
alfred@psymin:~$ ss -lnput
Netid             State              Recv-Q             Send-Q                         Local Address:Port                            Peer Address:Port             Process                                      
udp               UNCONN             0                  0                                    0.0.0.0:68                                   0.0.0.0:*                                                             
udp               UNCONN             0                  0                                    0.0.0.0:10000                                0.0.0.0:*                                                             
tcp               LISTEN             0                  4096                               127.0.0.1:10000                                0.0.0.0:*                                                             
tcp               LISTEN             0                  5                                    0.0.0.0:8080                                 0.0.0.0:*                 users:(("socat",pid=1937,fd=5))             
tcp               LISTEN             0                  5                                    0.0.0.0:3000                                 0.0.0.0:*                 users:(("socat",pid=450,fd=5))              
tcp               LISTEN             0                  511                                  0.0.0.0:80                                   0.0.0.0:*                                                             
tcp               LISTEN             0                  128                                  0.0.0.0:22                                   0.0.0.0:*                                                             
tcp               LISTEN             0                  511                                     [::]:80                                      [::]:*                                                             
tcp               LISTEN             0                  128                                     [::]:22                                      [::]:*                                                             
alfred@psymin:~$ 
```
![图 13](../assets/images/d5b0528d68b194090a2b1abf6e3bde8e1c47261b4aecebf9aee30f4f040dc2d6.png)  
![图 14](../assets/images/d1a2590342c74ae8e7c092d0dbe7896fcfbf59b3a18e16da85003bed7915282b.png)  

>跑了脚本没有东西，得手找webmin的东西
>
>密码弱口令：root:root
>
![图 15](../assets/images/3424ffafc481c3f02d1b90831a1ffa7470821939cb8d8bf26519b1113b3eb3ec.png)  



>好了这个靶机结束


>userflag:e12853c615d191efce15c726a0684754
>
>rootflag:8968662c86171f7a5afe387a949fe665
>