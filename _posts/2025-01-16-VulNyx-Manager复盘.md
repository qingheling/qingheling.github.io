---
title: VulNyx Manager靶机复盘
author: LingMj
data: 2025-01-16
categories: [VulNyx]
tags: [SNMP,SMB]
description: 难度-Hard
typora-root-url: ./..\assets\images
---

## 首先进行网段扫描：

>```
>└─# arp-scan -l                                                                                                                                                                                                 
>Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128                                                                                                                                     
>Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)                                                                                                                                  
>192.168.26.1    00:50:56:c0:00:08       VMware, Inc.                                                                                                                                                            
>192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.                                                                                                                                                            
>192.168.26.161  00:0c:29:9b:9c:2f       VMware, Inc.                                                                                                                                                            
>192.168.26.254  00:50:56:e3:3d:3d       VMware, Inc.                                                                                                                                                            
>                                                                                                                                                                                                                
>4 packets received by filter, 0 packets dropped by kernel                                                                                                                                                       
>Ending arp-scan 1.10.0: 256 hosts scanned in 2.587 seconds (98.96 hosts/sec). 4 responded 
>```

## 端口扫描：

>```
>└─# nmap -p- -sC -sV 192.168.26.161                                                                                                                                                                             
>Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-16 02:16 EST                                                                                                                                              
>Nmap scan report for 192.168.26.161 (192.168.26.161)                                                                                                                                                            
>Host is up (0.0012s latency).                                                                                                                                                                                   
>Not shown: 65532 closed tcp ports (reset)                                                                                                                                                                       
>PORT     STATE SERVICE      VERSION                                                                                                                                                                             
>22/tcp   open  ssh          OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)                                                                                                                                       
>| ssh-hostkey:                                                                                                                                                                                                  
>|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)                                                                                                                                                  
>|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)                                                                                                                                                 
>|_  256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)                                                                                                                                               
>80/tcp   open  http         nginx 1.18.0                                                                                                                                                                        
>|_http-title: Site doesn't have a title (text/html).                                                                                                                                                            
>|_http-server-header: nginx/1.18.0                                                                                                                                                                              
>4445/tcp open  microsoft-ds                                                                                                                                                                                     
>| fingerprint-strings:                                                                                                                                                                                          
>|   SMBProgNeg:                                                                                                                                                                                                 
>|     SMBr                                                                                                                                                                                                      
>|_    "3DUfw                                                                                                                                                                                                    
>1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :                                    
>SF-Port4445-TCP:V=7.94SVN%I=7%D=1/16%Time=6788B2AF%P=x86_64-pc-linux-gnu%r                                                                                                                                      
>SF:(SMBProgNeg,51,"\0\0\0M\xffSMBr\0\0\0\0\x80\0\xc0\0\0\0\0\0\0\0\0\0\0\0                                                                                                                                      
>SF:\0\0\0@\x06\0\0\x01\0\x11\x07\0\x03\x01\0\x01\0\0\xfa\0\0\0\0\x01\0\0\0                                                                                                                                      
>SF:\0\0p\0\0\0\0\0\0\0\0\0\0\0\0\0\x08\x08\0\x11\"3DUfw\x88");                                                                                                                                                  
>MAC Address: 00:0C:29:9B:9C:2F (VMware)                                                                                                                                                                         
>Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel                                                                                                                                                         
>
>Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                                                                                  
>Nmap done: 1 IP address (1 host up) scanned in 125.32 seconds
>```
>
>```
>└─# nmap -sU --top-ports 20 192.168.26.161                                                                                                                                                                      
>Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-16 02:15 EST                                                                                                                                              
>Nmap scan report for 192.168.26.161 (192.168.26.161)                                                                                                                                                            
>Host is up (0.0016s latency).                                                                                                                                                                                   
>
>PORT      STATE         SERVICE                                                                                                                                                                                 
>53/udp    closed        domain                                                                                                                                                                                  
>67/udp    closed        dhcps                                                                                                                                                                                   
>68/udp    open|filtered dhcpc                                                                                                                                                                                   
>69/udp    closed        tftp                                                                                                                                                                                    
>123/udp   closed        ntp                                                                                                                                                                                     
>135/udp   closed        msrpc                                                                                                                                                                                   
>137/udp   closed        netbios-ns                                                                                                                                                                              
>138/udp   closed        netbios-dgm                                                                                                                                                                             
>139/udp   closed        netbios-ssn                                                                                                                                                                             
>161/udp   open          snmp                                                                                                                                                                                    
>162/udp   closed        snmptrap                                                                                                                                                                                
>445/udp   closed        microsoft-ds                                                                                                                                                                            
>500/udp   closed        isakmp                                                                                                                                                                                  
>514/udp   closed        syslog                                                                                                                                                                                  
>520/udp   closed        route                                                                                                                                                                                   
>631/udp   closed        ipp                                                                                                                                                                                     
>1434/udp  closed        ms-sql-m                                                                                                                                                                                
>1900/udp  closed        upnp                                                                                                                                                                                    
>4500/udp  closed        nat-t-ike                                                                                                                                                                               
>49152/udp closed        unknown                                                                                                                                                                                 
>MAC Address: 00:0C:29:9B:9C:2F (VMware)                                                                                                                                                                         
>
>Nmap done: 1 IP address (1 host up) scanned in 15.38 seconds
>```
>
>这里可以看到出现了SMB和SNMP服务

## SNMP服务扫描
![图 0](../assets/images/29337be339f8f4b5132cab1276810e0d8e99fb891414b15fac0a745547524b35.png)  

>接下来需要利用一下snmpbulkwalk这个款工具
![图 1](../assets/images/94bfe818fa4b09aa38c90e7addba013c4e959df039092f3dfd18fa45bae0242b.png)  
> 这里我们获取到了目录以及smb的用户和hash
![图 2](../assets/images/b219553e5f203945b252e0eace75387dea96d717cd013c3132e73f8efb42b1d1.png)  
>这里还有一个域名，需要给hosts加上
![图 3](../assets/images/9d726d0a33ecb40dcb1e46343f58e74d7541f607fdac3ea942dec24cb2ef374a.png)  
>本地主机也设置
![图 5](../assets/images/753401d0307d3a8b4727cdb264066290d81685236de18fb1852c84ab74d9a679.png)  
>出现403正常现象
## SMB登录操作
>这里利用netexec工具进行信息获取
![图 6](../assets/images/1e1843800f48dbdc00c58a1c274105501e5304ec8d387bbeda6e89404d53a24f.png)  
>这里可以进行smb服务的登录，思路来自于ll104567和softyhack大佬开荒思路
![图 7](../assets/images/3415211b78e79c33e648fe6dfe00939fe28008c831fde5c416e796e143cbdde0.png)  
>这里获取到对应的disk进行文件上传
![图 8](../assets/images/3ec46a19e1caf1324d72ce8a0b2ccea404f080395c5ab109ad240ea4540ef6d2.png)  
>这里把需要利用的php代码上传上去,这里发现php不解析需要利用后缀名去操作
![图 9](../assets/images/84c7f45d9ec18618442cd8329324b5c94e8416593d91196b8d8d1912272564d8.png)  
>可以利用HackTricks的upload进行尝试操作
![图 10](../assets/images/0a6c83ddf6b27e473199bd2936fad464d6c4e951c6fb3844d9a58a581d140521.png)
## Webshell操作  
>尝试完无果，但是有一个明显的地方是我们是域名会不会隐藏在子域名上
![图 11](../assets/images/ccc424960cb32a79d17c8b69e6cd672f590aedd711740ef4a9d4bab76499cc53.png)  
>很快出来了，根据那个子域名可以解析这个php，解析回显为0
![图 12](../assets/images/5fa646eaf7bca3d24cc6c3f457d2e774640f58eb7d2f545e3121361474dba0f1.png)  
>接下来就是常规的webshell操作
## 提权
![图 13](../assets/images/b199b3485ecf3524a5922e15b80e317f2ef3d4b0f0d6a0625db9cc4185625099.png)  
>这里可以看到没有sudo -l 但是有10000，利用socat传递出来看看
![图 14](../assets/images/cf56455ec1f1d1e73293a371feae3b795ae407f04344dee521055963b4d4ff7f.png)  
![图 15](../assets/images/e85f5aa9fdbfe781ecac864c81aabf2dd999794898e8b2c0e2f80c4be89e8a2b.png)  
>目前来看可能需要登录账号和密码，进行查找文件
>
![图 16](../assets/images/a6fa98d36c3eedd7d429f708ae0e7621e93719d0f399008e910bbca979758694.png)  
>这里存在定时任务
>
![图 17](../assets/images/da625cec30bb19f8ee03cefbcbcdcaf49cf97d9f12f1398e86f8184fecfaa924.png)  
>这里有一个可以更改的文件
>
![图 18](../assets/images/36d032791158f9d68a4ca0a433290eb0f2cfe8b14e4ecb67fc288b2861a8b484.png)  
>像是一个passwd,试更改一下
>
![图 19](../assets/images/5fe39d18abf63a4c4d95bea553d8bb81179b72d9a421ad7189dc1e1ed6ef588f.png)
>  
>尝试登录发现存在登录密码设计，我们这里设计一个用户是root就可以登录了
>
![图 22](../assets/images/cb25952c8ca489558596775d2d3a477bc522dba86654ff35be807efb439eb262.png)  
>尝试登录
>
![图 23](../assets/images/15ca0ef254f43cd6faa7633c5ffbf3261fb6f03a4544d25765039cf4b234ef45.png)  
>发现root账号登录
>
![图 24](../assets/images/65e1187e7d5fea44ec0f6353519c457867b3d94c0c18a8a711b709d34f43eade.png)
>userflag:331f2b89261b006cac32f7e7df7e6247
>
>rootflag:a63b115640f6466c0d37ba166ea42d10
