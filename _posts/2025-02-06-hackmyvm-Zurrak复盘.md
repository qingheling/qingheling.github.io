---
title: hackmyvm Zurrak靶机复盘
author: LingMj
data: 2025-02-06
categories: [hackmyvm]
tags: [jwt,vmdk,smb]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:a3:c4:7b       PCS Systemtechnik GmbH
192.168.56.140  08:00:27:f7:cd:f1       PCS Systemtechnik GmbH

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.355 seconds (108.70 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj/xxoo# nmap -p- -sC -sV 192.168.56.140        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-06 06:46 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.140
Host is up (0.012s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE     VERSION
80/tcp   open  http        Apache httpd 2.4.57 ((Debian))
| http-title: Login Page
|_Requested resource was login.php
|_http-server-header: Apache/2.4.57 (Debian)
139/tcp  open  netbios-ssn Samba smbd 4.6.2
445/tcp  open  netbios-ssn Samba smbd 4.6.2
5432/tcp open  postgresql  PostgreSQL DB 9.6.0 or later
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=zurrak
| Subject Alternative Name: DNS:zurrak
| Not valid before: 2023-10-20T19:29:16
|_Not valid after:  2033-10-17T19:29:16
| fingerprint-strings: 
|   SMBProgNeg: 
|     SFATAL
|     VFATAL
|     C0A000
|     Munsupported frontend protocol 65363.19778: server supports 3.0 to 3.0
|     Fpostmaster.c
|     L2195
|_    RProcessStartupPacket
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port5432-TCP:V=7.94SVN%I=7%D=2/6%Time=67A4A16C%P=x86_64-pc-linux-gnu%r(
SF:SMBProgNeg,8C,"E\0\0\0\x8bSFATAL\0VFATAL\0C0A000\0Munsupported\x20front
SF:end\x20protocol\x2065363\.19778:\x20server\x20supports\x203\.0\x20to\x2
SF:03\.0\0Fpostmaster\.c\0L2195\0RProcessStartupPacket\0\0");
MAC Address: 08:00:27:F7:CD:F1 (Oracle VirtualBox virtual NIC)

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: 7h59m58s
| smb2-time: 
|   date: 2025-02-06T19:48:02
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 70.30 seconds

```

## 获取webshell
![图 0](../assets/images/d6025174598bf769ae0b86f2f2bb6e73ecdf41dd631ed538c0f14dfe94f23aa0.png)  
![图 1](../assets/images/a945366bd177b9fa2638e570307b2f36e2bf45233c125954d9a4c71bacfc603e.png)  
![图 2](../assets/images/9985d5129ce88778c942fec6b0e198a1c662da6a33f0f4102c00c004928eb057.png)  
![图 3](../assets/images/b8396faa5490d7367168664c58c652c34db6b49c43c5537dee6c773fce6d34db.png)  
![图 4](../assets/images/4f53a49235d869c49ff27b8111f5633b18e384f8e7f82360b69dfe92666df1ba.png)  

>啊，信息怎么少
>

![图 6](../assets/images/9ec164a9f1015846c5540350114fc6539d4b7d00e987d249c7bd34387ff7cb0c.png)  

![图 5](../assets/images/a3690fb9a1e75ce46e6efd8a4ae2351ea0cea5a1b65d0205ec080d245cc9bd9f.png)  
![图 7](../assets/images/eaf491346918343ab7cb6954adb96a4eadedd4b357b0b1f50f645131e827de60.png)  
![图 8](../assets/images/d40646c3b9368a3e65db53d34aa4d924146de184f293a185e95d7de0b945b64a.png)  
![图 9](../assets/images/78c1262d3357f523e63d43e5ba5759a8ba4e987cd9dc90b64ca0d63fd9ff49ab.png)  
![图 10](../assets/images/479035df0fbcac42f680bb34e19f7baf120af77218d82c2449d905eab3380334.png)  
![图 11](../assets/images/d28e7835c70ec9cad270e39f9575823e43d6ba5c0867b4c24ff14b241620a3c5.png)
![图 13](../assets/images/6db5038ad458c3fbe64349a49070fafe44a0b35586d4e2402173a4d4ba26180e.png)  

![图 12](../assets/images/f1646d08ffdd50f6f3b7a0796e7416f7ed0c284a0ea91d1eff775d88b1871246.png)  
![图 14](../assets/images/736ce103c9e4e34928482427795083f9535da032fb07513b4afab5b3284cc478.png)  
![图 15](../assets/images/d648f31960210b88e32b3b288211c94a13e94458571c60b9f0d18a2083acbe43.png)  

>地址：https://github.com/ticarpi/jwt_tool
>
![图 16](../assets/images/a5bb7a8d06a3a3c0b3d30a36f0218ed41a85f350f195909651c14aece9aa90a2.png)  


```
root@LingMj:/home/lingmj/xxoo1/jwt_tool# python3 jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImludGVybmFsQHp1cnJhay5odGIiLCJpc0FkbWluIjp0cnVlLCJpYXQiOjEzNTY5OTk1MjQsIm5iZiI6MTM1NzAwMDAwMH0.GZiD9dJTb0NTJtHftOE01HXTVTW2arpE-w-xgWEJwZQ -jw /usr/share/wordlists/rockyou.txt 

        \   \        \         \          \                    \ 
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi      

No config file yet created.
Running config setup.
Configuration file built - review contents of "jwtconf.ini" to customise your options.
Make sure to set the "httplistener" value to a URL you can monitor to enable out-of-band checks.
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/jwt_tool# python3 jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImludGVybmFsQHp1cnJhay5odGIiLCJpc0FkbWluIjp0cnVlLCJpYXQiOjEzNTY5OTk1MjQsIm5iZiI6MTM1NzAwMDAwMH0.GZiD9dJTb0NTJtHftOE01HXTVTW2arpE-w-xgWEJwZQ -kf /usr/share/wordlists/rockyou.txt

        \   \        \         \          \                    \ 
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi      

Original JWT: 

=====================
Decoded Token Values:
=====================

Token header values:
[+] typ = "JWT"
[+] alg = "HS256"

Token payload values:
[+] email = "internal@zurrak.htb"
[+] isAdmin = True
[+] iat = 1356999524    ==> TIMESTAMP = 2012-12-31 19:18:44 (UTC)
[+] nbf = 1357000000    ==> TIMESTAMP = 2012-12-31 19:26:40 (UTC)

Seen timestamps:
[*] iat was seen
[*] nbf is later than iat by: 0 days, 0 hours, 7 mins

----------------------
JWT common timestamps:
iat = IssuedAt
exp = Expires
nbf = NotBefore
----------------------

                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/jwt_tool# python3 jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImludGVybmFsQHp1cnJhay5odGIiLCJpc0FkbWluIjp0cnVlLCJpYXQiOjEzNTY5OTk1MjQsIm5iZiI6MTM1NzAwMDAwMH0.GZiD9dJTb0NTJtHftOE01HXTVTW2arpE-w-xgWEJwZQ -p /usr/share/wordlists/rockyou.txt

        \   \        \         \          \                    \ 
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi      

Original JWT: 

=====================
Decoded Token Values:
=====================

Token header values:
[+] typ = "JWT"
[+] alg = "HS256"

Token payload values:
[+] email = "internal@zurrak.htb"
[+] isAdmin = True
[+] iat = 1356999524    ==> TIMESTAMP = 2012-12-31 19:18:44 (UTC)
[+] nbf = 1357000000    ==> TIMESTAMP = 2012-12-31 19:26:40 (UTC)

Seen timestamps:
[*] iat was seen
[*] nbf is later than iat by: 0 days, 0 hours, 7 mins

----------------------
JWT common timestamps:
iat = IssuedAt
exp = Expires
nbf = NotBefore
----------------------

                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/jwt_tool# python3 jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImludGVybmFsQHp1cnJhay5odGIiLCJpc0FkbWluIjp0cnVlLCJpYXQiOjEzNTY5OTk1MjQsIm5iZiI6MTM1NzAwMDAwMH0.GZiD9dJTb0NTJtHftOE01HXTVTW2arpE-w-xgWEJwZQ -C -d /usr/share/wordlists/rockyou.txt

        \   \        \         \          \                    \ 
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi      

Original JWT: 

[*] Tested 1 million passwords so far
[*] Tested 2 million passwords so far
[*] Tested 3 million passwords so far
[*] Tested 4 million passwords so far
[*] Tested 5 million passwords so far
^C^C^C^C^C^C[*] Tested 6 million passwords so far
[*] Tested 7 million passwords so far
[*] Tested 8 million passwords so far
[*] Tested 9 million passwords so far
[*] Tested 10 million passwords so far
[*] Tested 11 million passwords so far
[*] Tested 12 million passwords so far
[*] Tested 13 million passwords so far
[*] Tested 14 million passwords so far
[-] Key not in dictionary

===============================
As your list wasn't able to crack this token you might be better off using longer dictionaries, custom dictionaries, mangling rules, or brute force attacks.
hashcat (https://hashcat.net/hashcat/) is ideal for this as it is highly optimised for speed. Just add your JWT to a text file, then use the following syntax to give you a good start:

[*] dictionary attacks: hashcat -a 0 -m 16500 jwt.txt passlist.txt
[*] rule-based attack:  hashcat -a 0 -m 16500 jwt.txt passlist.txt -r rules/best64.rule
[*] brute-force attack: hashcat -a 3 -m 16500 jwt.txt ?u?l?l?l?l?l?l?l -i --increment-min=6
===============================

                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/jwt_tool# python3 jwt_tool.py eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImludGVybmFsQHp1cnJhay5odGIiLCJpc0FkbWluIjpmYWxzZSwiaWF0IjoxMzU2OTk5NTI0LCJuYmYiOjEzNTcwMDAwMDB9.ufkwBsusc4IEYCCRszCbcSEv6irCtUSx-Uq08OThxso -C -d /usr/share/wordlists/rockyou.txt

        \   \        \         \          \                    \ 
   \__   |   |  \     |\__    __| \__    __|                    |
         |   |   \    |      |          |       \         \     |
         |        \   |      |          |    __  \     __  \    |
  \      |      _     |      |          |   |     |   |     |   |
   |     |     / \    |      |          |   |     |   |     |   |
\        |    /   \   |      |          |\        |\        |   |
 \______/ \__/     \__|   \__|      \__| \______/  \______/ \__|
 Version 2.2.7                \______|             @ticarpi      

Original JWT: 

[*] Tested 1 million passwords so far
[*] Tested 2 million passwords so far
[+] TEST123 is the CORRECT key!
You can tamper/fuzz the token contents (-T/-I) and sign it using:
python3 jwt_tool.py [options here] -S hs256 -p "TEST123"

```
![图 17](../assets/images/6fe61473e143abe39c812abd04a345befd1cf265e21a8d8b2b7797299be5dabd.png)  
![图 18](../assets/images/5a89fc9df885c5aa88f6bb0e2e2dbbc96c465c4988c1927d9fd822d09a9de2b5.png)  


>cookie:token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImludGVybmFsQHp1cnJhay5odGIiLCJpc0FkbWluIjp0cnVlLCJpYXQiOjEzNTY5OTk1MjQsIm5iZiI6MTM1NzAwMDAwMH0.gBpFlpNfVUBlv9HuqXqVzRtaHR265PFagumX_OAKCMY
>

```
root@LingMj:/home/lingmj/xxoo# exiftool zurrakhorse.jpg 
ExifTool Version Number         : 12.76
File Name                       : zurrakhorse.jpg
Directory                       : .
File Size                       : 674 kB
File Modification Date/Time     : 2023:10:24 13:03:45-04:00
File Access Date/Time           : 2025:02:06 07:48:12-05:00
File Inode Change Date/Time     : 2025:02:06 07:48:12-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Exif Byte Order                 : Big-endian (Motorola, MM)
Orientation                     : Horizontal (normal)
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Software                        : Adobe Photoshop CC 2018 (Windows)
Modify Date                     : 2023:10:24 20:02:16
Color Space                     : sRGB
Exif Image Width                : 2000
Exif Image Height               : 2068
Compression                     : JPEG (old-style)
Thumbnail Offset                : 318
Thumbnail Length                : 4962
IPTC Digest                     : 00000000000000000000000000000000
Displayed Units X               : inches
Displayed Units Y               : inches
Print Style                     : Centered
Print Position                  : 0 0
Print Scale                     : 1
Global Angle                    : 90
Global Altitude                 : 30
URL List                        : 
Slices Group Name               : Untitled-1
Num Slices                      : 1
Pixel Aspect Ratio              : 1
Photoshop Thumbnail             : (Binary data 4962 bytes, use -b option to extract)
Has Real Merged Data            : Yes
Writer Name                     : Adobe Photoshop
Reader Name                     : Adobe Photoshop CC 2018
Photoshop Quality               : 12
Photoshop Format                : Standard
XMP Toolkit                     : Adobe XMP Core 5.6-c142 79.160924, 2017/07/13-01:06:39
Creator Tool                    : Adobe Photoshop CC 2018 (Windows)
Create Date                     : 2023:10:24 20:02:16+03:00
Metadata Date                   : 2023:10:24 20:02:16+03:00
Instance ID                     : xmp.iid:d3c42f7a-fc08-564b-9680-0732f5c21a40
Document ID                     : adobe:docid:photoshop:c103a364-af37-f544-81a7-31dc1bd0ec79
Original Document ID            : xmp.did:a2b7154c-9def-2f41-9d90-56f8411511de
Format                          : image/jpeg
Color Mode                      : RGB
ICC Profile Name                : sRGB IEC61966-2.1
History Action                  : created, saved
History Instance ID             : xmp.iid:a2b7154c-9def-2f41-9d90-56f8411511de, xmp.iid:d3c42f7a-fc08-564b-9680-0732f5c21a40
History When                    : 2023:10:24 20:02:16+03:00, 2023:10:24 20:02:16+03:00
History Software Agent          : Adobe Photoshop CC 2018 (Windows), Adobe Photoshop CC 2018 (Windows)
History Changed                 : /
Profile CMM Type                : Linotronic
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 1998:02:09 06:49:00
Profile File Signature          : acsp
Primary Platform                : Microsoft Corporation
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : Hewlett-Packard
Device Model                    : sRGB
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Media-Relative Colorimetric
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Hewlett-Packard
Profile ID                      : 0
Profile Copyright               : Copyright (c) 1998 Hewlett-Packard Company
Profile Description             : sRGB IEC61966-2.1
Media White Point               : 0.95045 1 1.08905
Media Black Point               : 0 0 0
Red Matrix Column               : 0.43607 0.22249 0.01392
Green Matrix Column             : 0.38515 0.71687 0.09708
Blue Matrix Column              : 0.14307 0.06061 0.7141
Device Mfg Desc                 : IEC http://www.iec.ch
Device Model Desc               : IEC 61966-2.1 Default RGB colour space - sRGB
Viewing Cond Desc               : Reference Viewing Condition in IEC61966-2.1
Viewing Cond Illuminant         : 19.6445 20.3718 16.8089
Viewing Cond Surround           : 3.92889 4.07439 3.36179
Viewing Cond Illuminant Type    : D50
Luminance                       : 76.03647 80 87.12462
Measurement Observer            : CIE 1931
Measurement Backing             : 0 0 0
Measurement Geometry            : Unknown
Measurement Flare               : 0.999%
Measurement Illuminant          : D65
Technology                      : Cathode Ray Tube Display
Red Tone Reproduction Curve     : (Binary data 2060 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 2060 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 2060 bytes, use -b option to extract)
DCT Encode Version              : 100
APP14 Flags 0                   : [14]
APP14 Flags 1                   : (none)
Color Transform                 : YCbCr
Image Width                     : 2000
Image Height                    : 2068
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 2000x2068
Megapixels                      : 4.1
Thumbnail Image                 : (Binary data 4962 bytes, use -b option to extract)
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# exiftool zurrakhearts.jpg 
ExifTool Version Number         : 12.76
File Name                       : zurrakhearts.jpg
Directory                       : .
File Size                       : 2.9 MB
File Modification Date/Time     : 2023:10:24 13:41:37-04:00
File Access Date/Time           : 2025:02:06 07:48:38-05:00
File Inode Change Date/Time     : 2025:02:06 07:48:38-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Image Width                     : 5000
Image Height                    : 4875
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 5000x4875
Megapixels                      : 24.4
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# exiftool zurraksnake.jpg 
ExifTool Version Number         : 12.76
File Name                       : zurraksnake.jpg
Directory                       : .
File Size                       : 771 kB
File Modification Date/Time     : 2023:10:24 13:03:45-04:00
File Access Date/Time           : 2025:02:06 07:48:28-05:00
File Inode Change Date/Time     : 2025:02:06 07:48:28-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Exif Byte Order                 : Big-endian (Motorola, MM)
Orientation                     : Horizontal (normal)
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Software                        : Adobe Photoshop CC 2018 (Windows)
Modify Date                     : 2023:10:24 20:02:59
Color Space                     : sRGB
Exif Image Width                : 2000
Exif Image Height               : 2113
Compression                     : JPEG (old-style)
Thumbnail Offset                : 318
Thumbnail Length                : 6114
IPTC Digest                     : 00000000000000000000000000000000
Displayed Units X               : inches
Displayed Units Y               : inches
Print Style                     : Centered
Print Position                  : 0 0
Print Scale                     : 1
Global Angle                    : 90
Global Altitude                 : 30
URL List                        : 
Slices Group Name               : Untitled-1
Num Slices                      : 1
Pixel Aspect Ratio              : 1
Photoshop Thumbnail             : (Binary data 6114 bytes, use -b option to extract)
Has Real Merged Data            : Yes
Writer Name                     : Adobe Photoshop
Reader Name                     : Adobe Photoshop CC 2018
Photoshop Quality               : 12
Photoshop Format                : Standard
XMP Toolkit                     : Adobe XMP Core 5.6-c142 79.160924, 2017/07/13-01:06:39
Creator Tool                    : Adobe Photoshop CC 2018 (Windows)
Create Date                     : 2023:10:24 20:02:59+03:00
Metadata Date                   : 2023:10:24 20:02:59+03:00
Instance ID                     : xmp.iid:55ce4063-a1ae-2445-839f-9555816e0bbc
Document ID                     : adobe:docid:photoshop:90b830b4-61fe-5b45-9d2e-7fb64a62c211
Original Document ID            : xmp.did:32726908-a22b-404b-8560-081a5437a5cb
Format                          : image/jpeg
Color Mode                      : RGB
ICC Profile Name                : sRGB IEC61966-2.1
History Action                  : created, saved
History Instance ID             : xmp.iid:32726908-a22b-404b-8560-081a5437a5cb, xmp.iid:55ce4063-a1ae-2445-839f-9555816e0bbc
History When                    : 2023:10:24 20:02:59+03:00, 2023:10:24 20:02:59+03:00
History Software Agent          : Adobe Photoshop CC 2018 (Windows), Adobe Photoshop CC 2018 (Windows)
History Changed                 : /
Profile CMM Type                : Linotronic
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 1998:02:09 06:49:00
Profile File Signature          : acsp
Primary Platform                : Microsoft Corporation
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : Hewlett-Packard
Device Model                    : sRGB
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Media-Relative Colorimetric
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Hewlett-Packard
Profile ID                      : 0
Profile Copyright               : Copyright (c) 1998 Hewlett-Packard Company
Profile Description             : sRGB IEC61966-2.1
Media White Point               : 0.95045 1 1.08905
Media Black Point               : 0 0 0
Red Matrix Column               : 0.43607 0.22249 0.01392
Green Matrix Column             : 0.38515 0.71687 0.09708
Blue Matrix Column              : 0.14307 0.06061 0.7141
Device Mfg Desc                 : IEC http://www.iec.ch
Device Model Desc               : IEC 61966-2.1 Default RGB colour space - sRGB
Viewing Cond Desc               : Reference Viewing Condition in IEC61966-2.1
Viewing Cond Illuminant         : 19.6445 20.3718 16.8089
Viewing Cond Surround           : 3.92889 4.07439 3.36179
Viewing Cond Illuminant Type    : D50
Luminance                       : 76.03647 80 87.12462
Measurement Observer            : CIE 1931
Measurement Backing             : 0 0 0
Measurement Geometry            : Unknown
Measurement Flare               : 0.999%
Measurement Illuminant          : D65
Technology                      : Cathode Ray Tube Display
Red Tone Reproduction Curve     : (Binary data 2060 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 2060 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 2060 bytes, use -b option to extract)
DCT Encode Version              : 100
APP14 Flags 0                   : [14]
APP14 Flags 1                   : (none)
Color Transform                 : YCbCr
Image Width                     : 2000
Image Height                    : 2113
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 2000x2113
Megapixels                      : 4.2
Thumbnail Image                 : (Binary data 6114 bytes, use -b option to extract)
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# 
```

![图 19](../assets/images/133480d8a27ca618a987225e8e853838e7eaa1996b49d12348b31792d1a8940d.png)  
![图 20](../assets/images/308a96dcede841b3873589a35a0e2c35bdea41ac55eb5c1a171dbc85e2653c74.png)  
![图 21](../assets/images/2fb15131bfe932ba080a3f75651d74d5aefd642029dc82881c41dab83b629268.png)  
![图 22](../assets/images/fd1772a29249586fb80e5be58edafb20f8866e311d0a6f58bf81c84bcdcc51c9.png)  
![图 23](../assets/images/ae39309548241bf66ea0a38d2fe4cbce81a3f5cc94d62193678f8a68016d47a4.png)  

>不擅长看这玩意，算了一个一个点，目前看没信息了，提示是smb爆破
>
![图 24](../assets/images/8339e2aa224862b22c5210d76960e3a5d0ab5766cadacd8871811b68557905f7.png)  
![图 25](../assets/images/94454ea33794010547438e864daaa0a244ec38e83a0d5088553dccdabd7326c0.png)  

>有结果了
>
![图 26](../assets/images/b776989c7a9d6ad85c3928b31a3163bf51d2a1ca9c8184a3ac49a0e4ec319096.png)  

>没扫出来这个目录呢，不然挨个试一下
>
![图 27](../assets/images/65cd3dc97142bac17bdbefdea4c38ce71441285bd9f112571ac243a1ce1f630d.png)  

>我好像懂了
>
![图 28](../assets/images/1c7908e4a66402eb23097e2558bde63fd13b8d22a1331150be7dc4979a0f7e70.png)  

>ok
>

```
smb: \> dir
  .                                   D        0  Fri Oct 20 17:14:00 2023
  ..                                  D        0  Fri Oct 20 16:36:51 2023
  DONTDELETE                          D        0  Fri Oct 20 23:44:44 2023
  operations                          D        0  Sat Oct 21 00:04:30 2023
  backup.reg                          N     1792  Sun Jul 24 01:30:09 2011
  human_resources                     D        0  Sun Apr  2 01:30:09 2017
  launch_options.txt                  N       21  Tue Dec 13 22:55:16 2022

                9232860 blocks of size 1024. 5935824 blocks available
smb: \> cd DONTDELETE\
smb: \DONTDELETE\> dir
  .                                   D        0  Fri Oct 20 23:44:44 2023
  ..                                  D        0  Fri Oct 20 17:14:00 2023
  eric                                D        0  Fri Oct 20 23:45:22 2023
  New folder                          D        0  Fri Oct 20 23:43:30 2023

                9232860 blocks of size 1024. 5935824 blocks available
smb: \DONTDELETE\> cd eric\
smb: \DONTDELETE\eric\> dir 
  .                                   D        0  Fri Oct 20 23:45:22 2023
  ..                                  D        0  Fri Oct 20 23:44:44 2023
  190709234924.BMP                    N  2359350  Tue Jul  9 23:49:48 2019
  bios                                N     1586  Sun Jul  7 20:06:20 2019
  biosoc2                             N  4194304  Mon Aug 19 20:47:38 2019
  New Text Document.txt               N        0  Fri Oct 20 23:44:48 2023
  190709234911.BMP                    N  2359350  Tue Jul  9 23:49:22 2019
  biosoc                              N  4194304  Sun Jul  7 20:06:56 2019
  190709234935.BMP                    N  2359350  Tue Jul  9 23:49:06 2019

                9232860 blocks of size 1024. 5935824 blocks available
smb: \DONTDELETE\eric\> cd ..
smb: \DONTDELETE\> cd New folder\
cd \DONTDELETE\New\: NT_STATUS_OBJECT_NAME_NOT_FOUND
smb: \DONTDELETE\> cd ..
smb: \> dir 
  .                                   D        0  Fri Oct 20 17:14:00 2023
  ..                                  D        0  Fri Oct 20 16:36:51 2023
  DONTDELETE                          D        0  Fri Oct 20 23:44:44 2023
  operations                          D        0  Sat Oct 21 00:04:30 2023
  backup.reg                          N     1792  Sun Jul 24 01:30:09 2011
  human_resources                     D        0  Sun Apr  2 01:30:09 2017
  launch_options.txt                  N       21  Tue Dec 13 22:55:16 2022

                9232860 blocks of size 1024. 5935824 blocks available
smb: \> cd operations\
smb: \operations\> dir
  .                                   D        0  Sat Oct 21 00:04:30 2023
  ..                                  D        0  Fri Oct 20 17:14:00 2023
  binaries                            D        0  Tue Nov 14 04:08:42 2023
  operators.txt                       N      118  Tue Dec 18 01:30:09 2001
  New folder                          D        0  Tue Dec 18 01:30:09 2001

                9232860 blocks of size 1024. 5935824 blocks available
smb: \operations\> get operators.txt
getting file \operations\operators.txt of size 118 as operators.txt (5.8 KiloBytes/sec) (average 5.8 KiloBytes/sec)
smb: \operations\> cd binaries\
smb: \operations\binaries\> dir
  .                                   D        0  Tue Nov 14 04:08:42 2023
  ..                                  D        0  Sat Oct 21 00:04:30 2023
  WinSCP-6.1.1-Setup.exe              N 11120192  Tue Dec 18 01:30:09 2001
  python-3.12.0-amd64.exe             N 26507904  Tue Dec 18 01:30:09 2001
  LAPS.x64.msi                        N  1118208  Tue Dec 18 01:30:09 2001

                9232860 blocks of size 1024. 5935824 blocks available
smb: \operations\binaries\> cd ..
smb: \operations\> ls
  .                                   D        0  Sat Oct 21 00:04:30 2023
  ..                                  D        0  Fri Oct 20 17:14:00 2023
  binaries                            D        0  Tue Nov 14 04:08:42 2023
  operators.txt                       N      118  Tue Dec 18 01:30:09 2001
  New folder                          D        0  Tue Dec 18 01:30:09 2001

                9232860 blocks of size 1024. 5935824 blocks available
smb: \operations\> cd New folder\
cd \operations\New\: NT_STATUS_OBJECT_NAME_NOT_FOUND
smb: \operations\> cd ..
smb: \> ls
  .                                   D        0  Fri Oct 20 17:14:00 2023
  ..                                  D        0  Fri Oct 20 16:36:51 2023
  DONTDELETE                          D        0  Fri Oct 20 23:44:44 2023
  operations                          D        0  Sat Oct 21 00:04:30 2023
  backup.reg                          N     1792  Sun Jul 24 01:30:09 2011
  human_resources                     D        0  Sun Apr  2 01:30:09 2017
  launch_options.txt                  N       21  Tue Dec 13 22:55:16 2022

                9232860 blocks of size 1024. 5935824 blocks available
smb: \> cd human_resources\
smb: \human_resources\> idr
idr: command not found
smb: \human_resources\> dir
  .                                   D        0  Sun Apr  2 01:30:09 2017
  ..                                  D        0  Fri Oct 20 17:14:00 2023
  employees.csv                       N     4750  Fri Oct 20 23:49:46 2023
  status.txt                          N       42  Fri Oct 20 23:51:28 2023

                9232860 blocks of size 1024. 5935824 blocks available
smb: \human_resources\> get employees.csv 
getting file \human_resources\employees.csv of size 4750 as employees.csv (210.8 KiloBytes/sec) (average 113.2 KiloBytes/sec)
smb: \human_resources\> get status.txt    
getting file \human_resources\status.txt of size 42 as status.txt (3.2 KiloBytes/sec) (average 87.2 KiloBytes/sec)
smb: \human_resources\> cd ..
smb: \> dir
  .                                   D        0  Fri Oct 20 17:14:00 2023
  ..                                  D        0  Fri Oct 20 16:36:51 2023
  DONTDELETE                          D        0  Fri Oct 20 23:44:44 2023
  operations                          D        0  Sat Oct 21 00:04:30 2023
  backup.reg                          N     1792  Sun Jul 24 01:30:09 2011
  human_resources                     D        0  Sun Apr  2 01:30:09 2017
  launch_options.txt                  N       21  Tue Dec 13 22:55:16 2022

                9232860 blocks of size 1024. 5935824 blocks available
smb: \> get launch_options.txt
```

![图 29](../assets/images/48a8d8437c41c051986d35eebc2df765c286f00c7ee9d9c43ab3b70598f93ff3.png)  
![图 30](../assets/images/5156b711b37de001a8514290fd9c5c05b86ec4b25f0f0205c3df504029db7b3d.png)  
![图 31](../assets/images/b813b30885a0df088bb4676f2035ac85c743ab55214af2b73e2baf61866e8b09.png)  
![图 32](../assets/images/d3e591d54634f7470b0fdc46b54864bd673144ab9ae904a04c7f003bf1044d7c.png)  

>这个目录一直没进去
>
![图 34](../assets/images/6dadaddd8ff12c1a0364a7ec2e087094d58017921302997f980723790ae3fd55.png)  

![图 33](../assets/images/e94e5e71a061570b45ca33318d20d5fd225cf19de718673a86bc41aef474e424.png)  

>这里已经无思路了看一下小白wp
>
![图 35](../assets/images/f74212ad427c1c782fd93a51a08d352a520f1941840cbf082ed3c6edc8e7d8e1.png)  
![图 36](../assets/images/b01f2fb506dcf8de06d2de27d37d3c4641af793b1e7fd28c6a71e556eecce17b.png)  
![图 37](../assets/images/ca010a2370090f0a23bfb3dba3c2ce82e094345b9db3f9ec327b73f9a451822e.png)  
![图 38](../assets/images/ed9b5ca61acb6233b050807c0256a7f289fd097acc0ff16295bd0cee84cfb9e8.png)  
![图 39](../assets/images/88d4768baf3357542206061ea1947eb959166ec3f75b2bd87bb6e2c1ff3e8b07.png)
![图 40](../assets/images/a915f19344c29efe45006e6993be11a43951be8929bb788eb486efd3b4011708.png)  

>偷懒了，直接grup过一下这部分，太试了上面的账号密码，而且没有对应的端口
>
![图 41](../assets/images/7443252f30c289014a4a2971f30e3468a28cc18e5aedc9d56eb23090bddb3510.png)  
![图 42](../assets/images/241595331af7870a4a919d88510a665575d1f62bf22bbaefb3a418145d473c32.png)  
![图 43](../assets/images/bdd98e8f9d300a2b58400750b0a513bc3063d6433048da9705edd16fe43ab81c.png)  
![图 44](../assets/images/82b32f310a10f63e55e0b6347f733c80bfaabb856c2bdf343a4eb217b3990039.png)  
![图 45](../assets/images/0317aff3422cb894f6fb29136651f455d7e4c765a9876a5c6e863b10d7c53a78.png)  
![图 46](../assets/images/270b8960dc69c97e745101b3e2d18dd0dfc0ac205e190f1d8483140e85ef7487.png)  
![图 47](../assets/images/e1ab70eb3bad09d61fab21bbc27bfc8c733b4a7ae5a521c3bfcadb77fc9ee7f1.png)  
![图 48](../assets/images/ac0203accde1f26eb762f6fe8b6572764d2f72d6b23105270e65837b7bd7b8d8.png)  

![图 49](../assets/images/35b782033d08ded7e6e97b0b21e63fbb7bf38253fa7326c5a10dce320439cb31.png)  

>还有一个端口没用了
>
![图 50](../assets/images/f8d703855e22ead4f8ef520ea5f84546992d9536b5a1482babaaba76022f386a.png)  

![图 51](../assets/images/987de4588af65c275b9e9777d8e38e4fa70fd4956abfb13da711391b4e18d77f.png)  

>有poc直接用poc
>
![图 52](../assets/images/e075b2cd4a4c18f3f0364983396c3f6163d0ea2be9f82b6070d380078585c5d8.png)  


## 提权
![图 53](../assets/images/54bceca3dd1643658134753ecf736fcb52750606a3640cdff6074d386757b92b.png)  
![图 54](../assets/images/9df96e6b8424dc49122b582bc67af5b602fa661a93b84c25b78f4e43b3be7650.png) 
![图 55](../assets/images/0a73822f4fdec197694ec12e75a1ed5123eef7388f97bec136b5bef41c7292a9.png)  
![图 56](../assets/images/8558789c7afd9d2ef94cfc29aad1040a4ca8dd78a0d345e2ee41ed85e3927de9.png)  

>又没有自己打自己吧
>
![图 57](../assets/images/878f3f22b3125dd8f6bec9e2f59aa2d3f87c1e5508e23540a2cf8bcec72d1872.png)  

```
[ipc$]
hosts allow = 127.0.0.1
hosts deny = 0.0.0.0/0
guest ok = no
browseable = no

[share]
comment = "zurrak operations share"
path = /opt/smbshare
hosts allow = 0.0.0.0/0
guest ok = no
browseable = yes
writable = no
valid users = emre, asli

[internal]
comment = "zurrak internal share"
path = /opt/internal
hosts allow = 127.0.0.1
guest ok = no
browseable = yes
writable = yes
valid users = emre
create mask = 0777
directory mask = 0777
force user = root
magic script = emergency.sh
postgres@zurrak:/home/postgres$ 
```
>登录进去就结束了,但是密码不对
>
![图 58](../assets/images/de270facd3f5853d573915fca3fc872809ba30f14993702458c56f34b614bf4f.png)  
![图 59](../assets/images/414172f7232ebf712602d815fa70899d63a636b653b1761933670ffb9187c97c.png)  

>靠在这里哎受不了
>
![图 60](../assets/images/6d81b5508bd45dd93bb28eee27a57aa3fdee7d19e36f7c396a8398b4f2dca6ef.png)  

>终于结束了，这个靶场有意思
>


>userflag:fe8f97f109ceb0362c95e60338c4c1a8
>
>rootflag:66fce7650a88ac2afd99d061e1c6a4df
>