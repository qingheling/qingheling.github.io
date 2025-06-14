---
title: hackmyvm DarkMatter靶机复盘
author: LingMj
data: 2025-06-08
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/xxoo# arp-scan -l    
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.148	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.092 seconds (122.37 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- -sC -sV 192.168.137.148
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-07 22:28 EDT
Nmap scan report for DarkMatter.mshome.net (192.168.137.148)
Host is up (0.042s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   3072 54:42:86:67:e3:5b:74:e1:87:9c:4d:80:0a:59:f3:4d (RSA)
|   256 b8:ae:fd:d6:01:e8:e4:0f:63:74:7c:ea:20:ac:fe:80 (ECDSA)
|_  256 f6:40:de:a2:c3:ec:2f:e0:f0:b9:76:21:3e:ee:a7:5d (ED25519)
80/tcp open  http    Apache httpd 2.4.51 ((Debian))
|_http-server-header: Apache/2.4.51 (Debian)
|_http-title: Apache2 Debian Default Page: It works
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.09 seconds
```

## 获取webshell

![picture 0](../assets/images/868cc8f0229a922932fb3afff7c440b4d79caef36d1c0078851d982318a02d0f.png)  
![picture 1](../assets/images/3c3d6e428f53a3b377f0cd074231b2eded24cea71c8adbfb09b03d63e0626d0a.png)  
![picture 2](../assets/images/e07e31ce90af68355ea1a81eb9994c223643d8db69dfab4a2c143b944f7cdc39.png)  

>有密码没账号
>

![picture 3](../assets/images/3487d3a981c9ecefb60473afbde7bd46b7058c88041177dcd956292b1a822e96.png)  

>感觉应该有子域名
>

![picture 4](../assets/images/c797b090f7f20cafae94064e71e4efb171a99420a97bff67c5c36aa86af63659.png)  
![picture 5](../assets/images/2c014f81ba9d0ed810fc6ef392ed08424cc5a5ecc5c190294f889021bda36d47.png)  

![picture 6](../assets/images/575e72fcfb872bff5122ea8d8a64d871f0cb212fe7c14f571a309f6255d98546.png)  

>用户应该在这里cewl一下
>

![picture 7](../assets/images/274ef6a44f7f84d238f0ecde472d0130d9d53e0cdd1363f36f3e02e19d497fb9.png)  

>admin不对
>

![picture 8](../assets/images/8867e7ff3ca3b2ac90a5315d1161bfb2c1f8be600315ae892171f5e7404f4e34.png)  

>有sql就简单了
>

![picture 9](../assets/images/a2f2bfbeb6a1787e5a40a8e67b436f74afb252b9905c4dda307e896d4ad62c48.png)  

>忘了咋用sqlmap了哈哈哈
>

![picture 10](../assets/images/5ef9858581d4501f9a2d608e507dca13a37d224a5c24e4e4015275d2e1e039ba.png)  

>不能直接os-shell，还是看字段吧
>

![picture 11](../assets/images/7072c1d6ca0aa7c8621073ff62c8d2e4e0047efea42c13fba954925e0b17ab51.png)  
![picture 12](../assets/images/b6d77f8941c708a342577db23104a25b64dda0fb87e42d030fc765e037ba15e5.png)  

>怎么卡在这里
>

![picture 13](../assets/images/1d9772d2ae83d4bcdd2615cd314067c0378b56dc149fec517d6d85a1133224e6.png)  

>这里咋自动登录了
>

```
root@LingMj:~/xxoo# stegseek dp.jpg                                         
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Progress: 99.71% (133.1 MB)           
[!] error: Could not find a valid passphrase.
                                                                                                                                                                                                        
root@LingMj:~/xxoo# exiftool dp.jpg  
ExifTool Version Number         : 12.76
File Name                       : dp.jpg
Directory                       : .
File Size                       : 50 kB
File Modification Date/Time     : 2021:11:14 03:10:16-05:00
File Access Date/Time           : 2025:06:07 23:28:28-04:00
File Inode Change Date/Time     : 2025:06:07 23:28:22-04:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Profile CMM Type                : Little CMS
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 2012:01:25 03:41:57
Profile File Signature          : acsp
Primary Platform                : Apple Computer Inc.
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : 
Device Model                    : 
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Little CMS
Profile ID                      : 0
Profile Description             : c2
Profile Copyright               : FB
Media White Point               : 0.9642 1 0.82491
Media Black Point               : 0.01205 0.0125 0.01031
Red Matrix Column               : 0.43607 0.22249 0.01392
Green Matrix Column             : 0.38515 0.71687 0.09708
Blue Matrix Column              : 0.14307 0.06061 0.7141
Red Tone Reproduction Curve     : (Binary data 64 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 64 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 64 bytes, use -b option to extract)
Image Width                     : 959
Image Height                    : 640
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 959x640
Megapixels                      : 0.614
```

![picture 14](../assets/images/348be24f9a4c493f9dc520d6407e242c538f32b31df09d824316f9582c772abe.png)  

>不是
>

![picture 15](../assets/images/1feeac1eb43344a9bad7839387bfa2f366710c0e9c4c8856827b19a4f9645b9e.png)  

>看了config表
>

![picture 16](../assets/images/8e016d73d6f1e243adec283962459365413b86e265c532474bee500d07da8ba9.png)  
![picture 17](../assets/images/ae2211348d576bd47611d407c0d76e41d36aa398668f9f47ce7d1ecc1099690c.png)  
![picture 18](../assets/images/abadca218c798bcc5d15ded8e822eeb3e0c8cb0860d79d1291aeb89031419de4.png)  

>有msf
>

![picture 19](../assets/images/101d9d3763a618ef058dd321496f492612b806754a91a1a8d6e6237b59122c0c.png)  
![picture 20](../assets/images/f0ca65d4a983bf74c56882a93cfe6d0c99458e447827ccc885778f8a5957aeeb.png)  

>OK后面可以简单了
>

## 提权

![picture 22](../assets/images/cad99f51c82f34af69f6fe1fb82726a446423b9e7d54dafdba8eef906e207b9e.png)  
![picture 23](../assets/images/a42528cf71b4221054430204f5beeb88c5c3f336819c5c12d4e9470f952a2ff9.png)  
![picture 24](../assets/images/2a2fc456fc3d548b459d01dc088d0ae25bb673678452e42d42e08ea54b13d5d7.png)  
![picture 25](../assets/images/1a214595f3ae6b6b82d94930d5cfdc9c8d365bd239ec183bb94dc3b8ac41804d.png)  

>没有读啥
>

![picture 26](../assets/images/19edd45ec5cc9034ae9a41dad602a44ff4df0e5ab8e306d490facd0e333774c0.png)  
![picture 27](../assets/images/ab2c4907f95c246c7be272b7b4e17c77004bd484d98e5899d237a80ec5af468c.png)  
![picture 28](../assets/images/14128ce0fe8def6df0d6d1bf9ee93298fbf99aede23614e248a5965b88a28974.png)  


>userflag:
>
>rootflag:
>
