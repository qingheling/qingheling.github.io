---
title: VulNyx Sun靶机复盘
author: LingMj
data: 2025-01-20
categories: [VulNyx]
tags: [upload,smb]
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
192.168.26.182  00:0c:29:91:ed:f1       (Unknown)
192.168.26.254  00:50:56:e8:96:d1       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.897 seconds (134.95 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.182                
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 21:36 EST
Nmap scan report for 192.168.26.182 (192.168.26.182)
Host is up (0.0013s latency).
Not shown: 65530 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
22/tcp   open  ssh         OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp   open  http        nginx 1.22.1
|_http-title: Sun
|_http-server-header: nginx/1.22.1
139/tcp  open  netbios-ssn Samba smbd 4.6.2
445/tcp  open  netbios-ssn Samba smbd 4.6.2
8080/tcp open  http        nginx 1.22.1
|_http-title: Sun
|_http-server-header: nginx/1.22.1
|_http-open-proxy: Proxy might be redirecting requests
MAC Address: 00:0C:29:91:ED:F1 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-time: 
|   date: 2025-01-20T10:38:20
|_  start_date: N/A
|_clock-skew: 7h59m59s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: SUN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.43 seconds
                                                             
```


## 获取webshell
![图 0](../assets/images/f44ac1cf080d5604e008746dd5900d41c70fc11df1668803d5c9fc5a4d804b64.png)  

```
└─# enum4linux -a 192.168.26.182                                                                              
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Jan 19 21:39:50 2025

 =========================================( Target Information )=========================================

Target ........... 192.168.26.182
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 192.168.26.182 )===========================


[+] Got domain/workgroup name: WORKGROUP


 ===============================( Nbtstat Information for 192.168.26.182 )===============================

Looking up status of 192.168.26.182
        SUN             <00> -         B <ACTIVE>  Workstation Service
        SUN             <03> -         B <ACTIVE>  Messenger Service
        SUN             <20> -         B <ACTIVE>  File Server Service
        ..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
        WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
        WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
        WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

        MAC Address = 00-00-00-00-00-00

 ==================================( Session Check on 192.168.26.182 )==================================


[+] Server 192.168.26.182 allows sessions using username '', password ''


 ===============================( Getting domain SID for 192.168.26.182 )===============================

Domain Name: WORKGROUP
Domain Sid: (NULL SID)

[+] Can't determine if host is part of domain or part of a workgroup


 ==================================( OS information on 192.168.26.182 )==================================


[E] Can't get OS info with smbclient


[+] Got OS info for 192.168.26.182 from srvinfo: 
        SUN            Wk Sv PrQ Unx NT SNT Samba 4.17.12-Debian
        platform_id     :       500
        os version      :       6.1
        server type     :       0x809a03


 ======================================( Users on 192.168.26.182 )======================================

index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: punt4n0  Name: punt4n0   Desc: 

user:[punt4n0] rid:[0x3e8]

 ================================( Share Enumeration on 192.168.26.182 )================================

smbXcli_negprot_smb1_done: No compatible protocol selected by server.

        Sharename       Type      Comment
        ---------       ----      -------
        print$          Disk      Printer Drivers
        IPC$            IPC       IPC Service (Samba 4.17.12-Debian)
        nobody          Disk      File Upload Path
Reconnecting with SMB1 for workgroup listing.
Protocol negotiation to server 192.168.26.182 (for a protocol between LANMAN1 and NT1) failed: NT_STATUS_INVALID_NETWORK_RESPONSE
Unable to connect with SMB1 -- no workgroup available

[+] Attempting to map shares on 192.168.26.182

//192.168.26.182/print$ Mapping: DENIED Listing: N/A Writing: N/A

[E] Can't understand response:

NT_STATUS_CONNECTION_REFUSED listing \*
//192.168.26.182/IPC$   Mapping: N/A Listing: N/A Writing: N/A
//192.168.26.182/nobody Mapping: DENIED Listing: N/A Writing: N/A

 ===========================( Password Policy Information for 192.168.26.182 )===========================



[+] Attaching to 192.168.26.182 using a NULL share

[+] Trying protocol 139/SMB...

[+] Found domain(s):

        [+] SUN
        [+] Builtin

[+] Password Info for Domain: SUN

        [+] Minimum password length: 5
        [+] Password history length: None
        [+] Maximum password age: 37 days 6 hours 21 minutes 
        [+] Password Complexity Flags: 000000

                [+] Domain Refuse Password Change: 0
                [+] Domain Password Store Cleartext: 0
                [+] Domain Password Lockout Admins: 0
                [+] Domain Password No Clear Change: 0
                [+] Domain Password No Anon Change: 0
                [+] Domain Password Complex: 0

        [+] Minimum password age: None
        [+] Reset Account Lockout Counter: 30 minutes 
        [+] Locked Account Duration: 30 minutes 
        [+] Account Lockout Threshold: None
        [+] Forced Log off Time: 37 days 6 hours 21 minutes 



[+] Retieved partial password policy with rpcclient:


Password Complexity: Disabled
Minimum Password Length: 5


 ======================================( Groups on 192.168.26.182 )======================================


[+] Getting builtin groups:


[+]  Getting builtin group memberships:


[+]  Getting local groups:


[+]  Getting local group memberships:


[+]  Getting domain groups:


[+]  Getting domain group memberships:


 =================( Users on 192.168.26.182 via RID cycling (RIDS: 500-550,1000-1050) )=================


[I] Found new SID: 
S-1-22-1

[I] Found new SID: 
S-1-5-32

[I] Found new SID: 
S-1-5-32

[I] Found new SID: 
S-1-5-32

[I] Found new SID: 
S-1-5-32

[+] Enumerating users using SID S-1-5-32 and logon username '', password ''

S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)

[+] Enumerating users using SID S-1-5-21-3376172362-2708036654-1072164461 and logon username '', password ''

S-1-5-21-3376172362-2708036654-1072164461-501 SUN\nobody (Local User)
S-1-5-21-3376172362-2708036654-1072164461-513 SUN\None (Domain Group)
S-1-5-21-3376172362-2708036654-1072164461-1000 SUN\punt4n0 (Local User)

[+] Enumerating users using SID S-1-22-1 and logon username '', password ''

S-1-22-1-1000 Unix User\punt4n0 (Local User)

 ==============================( Getting printer info for 192.168.26.182 )==============================

No printers returned.


enum4linux complete on Sun Jan 19 21:42:07 2025
```
>有线索一个是用户名punt4n0，还有一个nobody          Disk      File Upload Path
>

>直接操作一手
>
![图 1](../assets/images/1b5758f3e7b554b2a00ce9f40356feca21712e5f5b1334a969e6451be24a8db2.png)  

>跑一下密码了
>

![图 2](../assets/images/284818f25263fbc672689d8e1be15f9fabf3d084e9e5d4119aed3f432b578a27.png)  

![图 3](../assets/images/649220131c043e92bae07200050ad0ea87ba11ffcb78aee3204121c206908489.png)  

>拿到了测试一下上传
>

![图 4](../assets/images/35705fac38a16f5d6b791dd30ce50a78624a9bedb19d9e1d41910c82a208b36f.png)  

![图 5](../assets/images/cb1f3fd0cfceb0f42ecc1c43b343f4d76b07882948e5420e943c25cae3521669.png)  

>这是一个windows的smb控制
>
![图 6](../assets/images/6564054b0e7d11b57f82f3faac50555196bc48a586da5b843c76794d68a1321e.png)  

>测试上传路径
>
![图 7](../assets/images/38c28936f86341e0036110df68ef1acdc767d9c7643014921cf09ceb5df5cd22.png)  
![图 8](../assets/images/54af759f9f807a45eefb20021e78fc35d079889ddf39fe0a3754b7966ebb5a84.png)  

>证明在8080端口，因为是windows所以不能直接用php，Windows的话用的是aspx，kali自带
>
![图 10](../assets/images/be6cbff902d4ce04bf8f55bcdc4cc780da30b3d5b771b0d6cb9a82dfb63dd849.png)  

![图 9](../assets/images/0a68d8c4ab13a61fc964b1206b55a7a0b59e2182d2af195adcd84d7188fc164c.png)  
![图 11](../assets/images/aaa85b822fb8e977221043c06a898ec32fcfcdcf31b62ff6472a11992be5d44b.png)  

>这些是前面控制的基本操作
>

![图 12](../assets/images/63fa8c4144c54100bf73e47762ecb99331bd15808104a3942f22becc76f6e907.png)  
![图 13](../assets/images/e4a20f2bf3ed02c621c61b077eb20252dfab0f490f70444c5051566f7d7959e8.png)  

>不能直接用空格
>
![图 14](../assets/images/f0eba58ca8e96de7f3120f9df3104c009beadfe4ea159029101f545186d61af1.png)  

>有关键的passwd和id_rsa
>
![图 15](../assets/images/178b47bb778dd71c83c2dc75670ba9b0ed187f8377cdfbcfc8c6d17727242de0.png)  
![图 16](../assets/images/633ce32c932d835b165efc89571de4d8398548439f0b24386a2c12623de622a0.png)  
![图 17](../assets/images/d368386a2c279fd91cad583e694b9ed839fbe9b463332eae93a0cc23971f914e.png)  

## 提权
```
punt4n0@sun:~$ sudo -l
-bash: sudo: orden no encontrada
```
![图 18](../assets/images/f0fe66e4763dd7d7ce038a91ffd007e360be8e999565c829aaf35f297c6d7e3d.png)  
```
punt4n0@sun:/opt$ ls -al
total 16
drwxr-xr-x  3 root root 4096 abr  2  2024 .
drwxr-xr-x 18 root root 4096 abr  1  2024 ..
drwx------  3 root root 4096 abr  1  2024 microsoft
-rwx---rw-  1 root root   97 abr  2  2024 service.ps1
punt4n0@sun:/opt$ cat service.ps1 
$idOutput = id

$outputFilePath = "/dev/shm/out"

$idOutput | Out-File -FilePath $outputFilePath
punt4n0@sun:/opt$ cd /var/www/html/
punt4n0@sun:/var/www/html$ ls -al
total 112
drwxr-xr-x 2 punt4n0 punt4n0  4096 abr  2  2024 .
drwxr-xr-x 4 punt4n0 punt4n0  4096 abr  1  2024 ..
-rw-r--r-- 1 punt4n0 punt4n0   263 abr  2  2024 index.html
-rw-r--r-- 1 punt4n0 punt4n0 98346 abr  2  2024 sun.jpg
punt4n0@sun:/var/www/html$ cd /var/backups/
punt4n0@sun:/var/backups$ ls -al
total 588
drwxr-xr-x  2 root root   4096 ene 20 11:37 .
drwxr-xr-x 12 root root   4096 abr  1  2024 ..
-rw-r--r--  1 root root  30720 abr  2  2024 alternatives.tar.0
-rw-r--r--  1 root root  27662 abr  2  2024 apt.extended_states.0
-rw-r--r--  1 root root   2395 abr  1  2024 apt.extended_states.1.gz
-rw-r--r--  1 root root   2345 abr  1  2024 apt.extended_states.2.gz
-rw-r--r--  1 root root   2320 abr  1  2024 apt.extended_states.3.gz
-rw-r--r--  1 root root      0 abr  2  2024 dpkg.arch.0
-rw-r--r--  1 root root    186 nov 15  2023 dpkg.diversions.0
-rw-r--r--  1 root root    100 nov 15  2023 dpkg.statoverride.0
-rw-r--r--  1 root root 510706 abr  1  2024 dpkg.status.0
```
>可以看看有无定时任务还是直接利用这个
>

![图 19](../assets/images/3bb1fb7f1c7e7136eb694fc375618dcd07525270d55c5ac0aeaa289f2b767642.png)  

>好像明白了
>

![图 20](../assets/images/84a274511e956b9bae42f183a19c6a91b13d0bfb313c9cc58ed881279e37af4f.png)  
![图 21](../assets/images/721eadf5ff6ccd763d97aa6d65ce30c485369bc2d6c363d52580b46591bfec8a.png)  
![图 22](../assets/images/5be7449aacbeda1349a4b8cd6908cfae8b5cb9c8845fa98dfb9555e8a4c37e32.png)  

![图 23](../assets/images/15783cbb7fcd884fbd891d35af3193a831ad3cb9a182dd00651342c84d77e72d.png)  

>不能传文件，算了手动看定时任务
>
![图 24](../assets/images/513c3338639f16bc2184c7695fb834f6d03defae9e83d8eab46f8f9196bcf462.png)  

![图 25](../assets/images/abf156450c66b7241e50d6aa8c201e8f4cc8dc07c15a496be789af565faf220a.png)  

>这样可以看到定时任务
>

![图 26](../assets/images/396fe511272b676c24a84fdbd7c9121219b04f1780b875d5831cca2ad496e8fb.png)  

>算了随便试
>
![图 27](../assets/images/0b731889e3ab971155b772a6a5b4fad9b992cd2474a6e5039bbc30f34cee4835.png)  
![图 28](../assets/images/45e115ade0c8de611d495853f09eb1b7bd48f0aa56c717852ca3ad91ccf02d1a.png)  

>为啥写busybox，因为这个主机有，不过得root所以才这样写，不行就换一个没事影响,ok结束
>

>userflag:3b16b996837f6e87ffb20ab19edb88b7
>
>rootflag:e1e7f5e01538acad8c272a5da450f9f6
>


























