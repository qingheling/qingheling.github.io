---
title: hackmyvm Influencer靶机复盘
author: LingMj
data: 2025-02-08
categories: [hackmyvm]
tags: [wordpress,cupp,theme,exiftool]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:11:42:18       (Unknown)
192.168.56.147  08:00:27:55:e1:25       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.863 seconds (137.41 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.147
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-08 07:03 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.147
Host is up (0.0052s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.52 (Ubuntu)
2121/tcp open  ftp     vsftpd 3.0.5
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.56.110
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 0        0           11113 Jun 09  2023 facebook.jpg
| -rw-r--r--    1 0        0           35427 Jun 09  2023 github.jpg
| -rw-r--r--    1 0        0           88816 Jun 09  2023 instagram.jpg
| -rw-r--r--    1 0        0           27159 Jun 09  2023 linkedin.jpg
| -rw-r--r--    1 0        0              28 Jun 08  2023 note.txt
|_-rw-r--r--    1 0        0          124263 Jun 09  2023 snapchat.jpg
MAC Address: 08:00:27:55:E1:25 (Oracle VirtualBox virtual NIC)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 35.79 seconds
```

## 获取webshell
![图 0](../assets/images/222778604669b20d4afb9abb913fc39b3350d67021db1169b5367f5df73a392f.png)  
![图 1](../assets/images/e0c9612678b6dff9e178a2aaea4aa13cd359ca2d7680c037e9dc40c6dfd5518e.png)  
![图 2](../assets/images/224b24d50660d0703eceb250329d5d5be401c228191cccdaa0797c459b8de32d.png)  

>wordpress的靶机，先看ftp吧
>
![图 3](../assets/images/49d8af40e54572160c066f5b0c72da31952e2f50c6e8616b5ff60bfc864e7bda.png)  
![图 4](../assets/images/09147f47f147b9373647daeb818d293146cd1a7125796e6e73bb71deab474056.png)  
![图 5](../assets/images/b216f2b9a0f4b80d64f0581754c476a61d0367ba9cff6ff496889c33f86a93ed.png)  
![图 6](../assets/images/e01cd61de557c12187cea09903317c174d8450f87db099e642a3db1c855a7341.png)  
![图 7](../assets/images/02d2e4f4e0435c4b7633d7f7d896719f2b263e036f33ad4de95b98069b19692c.png)  
![图 8](../assets/images/e5fa68a7cbce2c29eae7766e07d6849e78464c947473c9477b3ff9f99a2d673d.png)  
![图 9](../assets/images/431c906811c2650ea7636dda1d4fa7cf0bad1951c72c10dc22d1c4a36d8a0d07.png)  


```
root@LingMj:/home/lingmj/xxoo# exiftool instagram.jpg 
ExifTool Version Number         : 12.76
File Name                       : instagram.jpg
Directory                       : .
File Size                       : 89 kB
File Modification Date/Time     : 2023:06:09 06:49:22-04:00
File Access Date/Time           : 2025:02:08 07:09:08-05:00
File Inode Change Date/Time     : 2025:02:08 07:09:08-05:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
Exif Byte Order                 : Little-endian (Intel, II)
Photometric Interpretation      : RGB
Orientation                     : Horizontal (normal)
Samples Per Pixel               : 3
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Software                        : Adobe Photoshop CC 2015 (Macintosh)
Modify Date                     : 2016:05:11 21:23:06
Exif Version                    : 0221
Color Space                     : Uncalibrated
Exif Image Width                : 924
Exif Image Height               : 764
Compression                     : JPEG (old-style)
Thumbnail Offset                : 398
Thumbnail Length                : 3876
Current IPTC Digest             : cdcffa7da8c7be09057076aeaf05c34e
Coded Character Set             : UTF8
Application Record Version      : 0
IPTC Digest                     : cdcffa7da8c7be09057076aeaf05c34e
Displayed Units X               : inches
Displayed Units Y               : inches
Print Style                     : Centered
Print Position                  : 0 0
Print Scale                     : 1
Global Angle                    : 30
Global Altitude                 : 30
URL List                        : 
Slices Group Name               : instagram_2016_icon
Num Slices                      : 1
Pixel Aspect Ratio              : 1
Photoshop Thumbnail             : (Binary data 3876 bytes, use -b option to extract)
Has Real Merged Data            : Yes
Writer Name                     : Adobe Photoshop
Reader Name                     : Adobe Photoshop CC 2015
Photoshop Quality               : 10
Photoshop Format                : Standard
XMP Toolkit                     : Adobe XMP Core 5.6-c067 79.157747, 2015/03/30-23:40:42
Original Document ID            : xmp.did:28c63ca7-9ca2-4996-84a5-7ef15c7e2f26
Document ID                     : xmp.did:87344C950EC411E6A514AEBCFA4BC85B
Instance ID                     : xmp.iid:49e71cba-93b4-4312-a915-b1db326d6638
Creator Tool                    : Adobe Photoshop CC 2015 (Macintosh)
Create Date                     : 2016:05:11 18:04:19+02:00
Metadata Date                   : 2016:05:11 21:23:06+02:00
Format                          : image/jpeg
Color Mode                      : RGB
ICC Profile Name                : Adobe RGB (1998)
Derived From Instance ID        : xmp.iid:d9d82c5c-1417-48b9-a7d0-a64b1e0c2fad
Derived From Document ID        : adobe:docid:photoshop:3b018f21-570f-1179-bd1b-ae993c068af8
History Action                  : saved
History Instance ID             : xmp.iid:49e71cba-93b4-4312-a915-b1db326d6638
History When                    : 2016:05:11 21:23:06+02:00
History Software Agent          : Adobe Photoshop CC 2015 (Macintosh)
History Changed                 : /
Profile CMM Type                : Adobe Systems Inc.
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 1999:06:03 00:00:00
Profile File Signature          : acsp
Primary Platform                : Apple Computer Inc.
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : none
Device Model                    : 
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Adobe Systems Inc.
Profile ID                      : 0
Profile Copyright               : Copyright 1999 Adobe Systems Incorporated
Profile Description             : Adobe RGB (1998)
Media White Point               : 0.95045 1 1.08905
Media Black Point               : 0 0 0
Red Tone Reproduction Curve     : (Binary data 14 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 14 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 14 bytes, use -b option to extract)
Red Matrix Column               : 0.60974 0.31111 0.01947
Green Matrix Column             : 0.20528 0.62567 0.06087
Blue Matrix Column              : 0.14919 0.06322 0.74457
DCT Encode Version              : 100
APP14 Flags 0                   : [14]
APP14 Flags 1                   : (none)
Color Transform                 : YCbCr
Image Width                     : 924
Image Height                    : 764
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Image Size                      : 924x764
Megapixels                      : 0.706
Thumbnail Image                 : (Binary data 3876 bytes, use -b option to extract)
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# strings -n 9 instagram.jpg 
Adobe Photoshop CC 2015 (Macintosh)
2016:05:11 21:23:06
VPhotoshop 3.0
printOutput
printSixteenBitbool
printerNameTEXT
printProofSetupObjc
proofSetup
builtinProof
        proofCMYK
printOutputOptions
Rd  doub@o
Grn doub@o
Bl  doub@o
BrdTUntF#Rlt
Bld UntF#Rlt
RsltUntF#Pxl@R
vectorDatabool
LeftUntF#Rlt
Top UntF#Rlt
Scl UntF#Prc@Y
cropWhenPrintingbool
cropRectBottomlong
cropRectLeftlong
cropRectRightlong
cropRectToplong
boundsObjc
slicesVlLs
sliceIDlong
groupIDlong
originenum
ESliceOrigin
autoGenerated
ESliceType
boundsObjc
altTagTEXT
cellTextIsHTMLbool
cellTextTEXT
        horzAlignenum
ESliceHorzAlign
        vertAlignenum
ESliceVertAlign
bgColorTypeenum
ESliceBGColorType
        topOutsetlong
leftOutsetlong
bottomOutsetlong
rightOutsetlong
http://ns.adobe.com/xap/1.0/
<?xpacket begin="
" id="W5M0MpCehiHzreSzNTczkc9d"?> <x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="Adobe XMP Core 5.6-c067 79.157747, 2015/03/30-23:40:42        "> <rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"> <rdf:Description rdf:about="" xmlns:xmpMM="http://ns.adobe.com/xap/1.0/mm/" xmlns:stRef="http://ns.adobe.com/xap/1.0/sType/ResourceRef#" xmlns:stEvt="http://ns.adobe.com/xap/1.0/sType/ResourceEvent#" xmlns:xmp="http://ns.adobe.com/xap/1.0/" xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:photoshop="http://ns.adobe.com/photoshop/1.0/" xmpMM:OriginalDocumentID="xmp.did:28c63ca7-9ca2-4996-84a5-7ef15c7e2f26" xmpMM:DocumentID="xmp.did:87344C950EC411E6A514AEBCFA4BC85B" xmpMM:InstanceID="xmp.iid:49e71cba-93b4-4312-a915-b1db326d6638" xmp:CreatorTool="Adobe Photoshop CC 2015 (Macintosh)" xmp:CreateDate="2016-05-11T18:04:19+02:00" xmp:ModifyDate="2016-05-11T21:23:06+02:00" xmp:MetadataDate="2016-05-11T21:23:06+02:00" dc:format="image/jpeg" photoshop:ColorMode="3" photoshop:ICCProfile="Adobe RGB (1998)"> <xmpMM:DerivedFrom stRef:instanceID="xmp.iid:d9d82c5c-1417-48b9-a7d0-a64b1e0c2fad" stRef:documentID="adobe:docid:photoshop:3b018f21-570f-1179-bd1b-ae993c068af8"/> <xmpMM:History> <rdf:Seq> <rdf:li stEvt:action="saved" stEvt:instanceID="xmp.iid:49e71cba-93b4-4312-a915-b1db326d6638" stEvt:when="2016-05-11T21:23:06+02:00" stEvt:softwareAgent="Adobe Photoshop CC 2015 (Macintosh)" stEvt:changed="/"/> </rdf:Seq> </xmpMM:History> </rdf:Description> </rdf:RDF> </x:xmpmeta>                                                                                                                        <?xpacket end="w"?>
@ICC_PROFILE
mntrRGB XYZ 
Copyright 1999 Adobe Systems Incorporated
Adobe RGB (1998)
qUP|1ULUx?~*
|>.cr?LG9
v90PCt9;aMb
,Yb'    }Q?x
LxYq-(hpp
<,mpOl<+m
UYGLUUG|UT
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# stegseek instagram.jpg    
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Progress: 99.95% (133.4 MB)           
[!] error: Could not find a valid passphrase.
```

![图 10](../assets/images/732cf7c535cff85f593576ffbddd4b58287df3088e4f799f94fab1ce65f85f7a.png)  
![图 11](../assets/images/af992ebd4193336a8780db92da780d6f49620bb5957a4ad95113005b67a2fdab.png)  
![图 12](../assets/images/fec15658778ba3195abdce0ca6f326dd524360f09b72c1eca745931da1fb9cb4.png)  
![图 13](../assets/images/7eed2b574fe382cdc327609464e19dfdfa885eb20c621d6879e3be46985c3b27.png)  
![图 14](../assets/images/8b99f2c4506f2e86272fae53cd2973ec20b0bf1f63a51630cd950561c7ac091f.png)  

>密码不对，算了尝试ftp爆破
>
![图 15](../assets/images/651a0f55918c7b647363b0ec5701de39606649aa3a35676b40d8ae339d0589e1.png)  
![图 16](../assets/images/9031b0f6bdb1597df174757ef043e7621213a870e729de378261cd54f320296c.png)  

>ftp没啥，太久了不跑了，跑wpscan了，不行的话我就找插件漏洞
>

![图 17](../assets/images/8daeb1e292decc00d38e2572f4603d5a1d7ba6ab2c70d76516aa4060f54eced1.png)  
![图 18](../assets/images/a60d517b9c2a78306831da8bf091cd88b6cc15a1eec4a63a8212b9723b9bd622.png)  

>把能测的都测了，看UDP了或者ipv6
>
![图 19](../assets/images/4dc814de317a9bcdc8a920ebae7a0fb0894a9bd60eebd37928c65a4ae62b1b67.png)  
![图 20](../assets/images/bd69652b26a6f40d421d247735b15ff7d05cac4816f1d300f325213d4c8ed3d8.png)  

>应该还有什么信息没有，扫目录了
>
![图 22](../assets/images/a16b5888341af039d1ebf82fe79f70c15b4d412eda64cddf6bad80569e95826f.png)  

![图 21](../assets/images/91537cbed337d0b261260829237d22fd664a5ffe3247269d68878bc89b68ac8d.png)  

>用一下生成密码工具
>
![图 23](../assets/images/113a9694b385860e8d144e0620d5d33e95d4fd95a76d93cf9303f06712f34b9f.png)  
![图 24](../assets/images/0f5980a941cada606f31ce3dcfd2820578b41c008d66477cb90a56f06b8acd06.png)  
![图 25](../assets/images/7622e3fa51e200a3551c1d196763d11394e1cce217d8232f76686e3a6a03fcba.png)  
![图 26](../assets/images/11a275b10dae0d51602a8f3ba03b0941eab52641c2b0c30ac8cebdc60720e4c0.png)  
![图 27](../assets/images/58df4569400aef16cd2d16826292c37ac859646f5363f49f7e8075e343c7066f.png)  

>没看到地址在那
>
![图 28](../assets/images/032712732163df918546036b4d89e96a441a1c29edec6bd867aa3378d96b3dec.png)  
![图 29](../assets/images/7a88d18995d6bf307433cff214ba8796223c07393eda4e1528d3fed8de47307e.png)  

>好像只能改一次，确实是成功执行，但是算了插件这个后面报错看看主题
>

![图 30](../assets/images/d6c9531b3c3160fffedd83dd169bbbad5f11120f6897da10cd5c62e1310d960a.png)  
![图 31](../assets/images/3c6cb834072dbcb9ca29e66b9bd726fef6df6db2faa04ec4763ff11f85601519.png)  
![图 32](../assets/images/5329ebe2493f184f6a79238aa3539ab998ad2f988e93db4153047e2c92115e98.png)  
![图 33](../assets/images/3e23652bd9870fa22205a79e39605b83d97529d763da79f1e9a9b69786437aee.png)  
![图 34](../assets/images/5b6eb28046c5d71b048b7073d4ba722804c00828d678af4d5f9fee54dc16d558.png)  
![图 35](../assets/images/754d40ffc73909110ca35dfe2e362e7a1bf5d7dc74838b4a2edff8864f1e9a5d.png)  

>突然忘了咋做了，看看直接注入
>
![图 36](../assets/images/fdb336cb4e06208f8cbb29cdb88e3d53b50baf3e185c923158da4d21c2d931fb.png)  
![图 37](../assets/images/98a2afe5c53b40724b1fa8da36e6b0b49da853acf2cfb2652908424c81e3efab.png)  
![图 38](../assets/images/6758567db0b772be03b711488479ff1f0cc9d5bfafa6a1314ea36003d616f59c.png)  
![图 39](../assets/images/81b954746707b1c961d8c6ab6bf7115d007067c3a2deef748cec6a3137e62c1a.png)  
![图 40](../assets/images/eb4273a1bbc814452a6c80cff7ba56a1822eeff2b1d12bd1e04e9460f8d2ff41.png)  
![图 41](../assets/images/0d4eba6e9bfc813f29ce96990ac90eff12f04b534565ef2f63cb6dae83de4962.png)  

>终于成功了，没nc，是busybox
>

## 提权
![图 42](../assets/images/e087fc14724db1d04afa2ed64fe49599a0d3535a1dfcdb753f514aaeaf2d7bd9.png)  
![图 43](../assets/images/ce1d2dec220fbd69dbff1b6068f8cc6105d0cdef07a75f83d9036119e1cb67ef.png)  
![图 44](../assets/images/90814b035ca654e96d1012894ce37c66e4e403875479c23e8d0118acd1e05fb9.png)  

>mysql 不对，但是我们有2个密码尝试登录user
>
![图 45](../assets/images/c3aac8df7b0ff60ca6578774d8195923c9ae39e2979c9969150675778ed534b1.png)  

>要我安装，我拿来网和权限，肯定不是这样登录了
>

```
www-data@influencer:/home$ cd /opt/
www-data@influencer:/opt$ ls -al
total 8
drwxr-xr-x  2 root root 4096 Feb 17  2023 .
drwxr-xr-x 19 root root 4096 Jun  8  2023 ..
www-data@influencer:/opt$ cd /var/backups/
www-data@influencer:/var/backups$ ls -al
total 48
drwxr-xr-x  2 root root  4096 Feb  8 12:44 .
drwxr-xr-x 14 root root  4096 Jun  8  2023 ..
-rw-r--r--  1 root root 39940 Jun 10  2023 apt.extended_states.0
www-data@influencer:/var/backups$ ss -lnput
Netid               State                Recv-Q               Send-Q                                     Local Address:Port                               Peer Address:Port               Process               
udp                 UNCONN               0                    0                                          127.0.0.53%lo:53                                      0.0.0.0:*                                        
udp                 UNCONN               0                    0                                  192.168.56.147%enp0s3:68                                      0.0.0.0:*                                        
tcp                 LISTEN               0                    128                                            127.0.0.1:1212                                    0.0.0.0:*                                        
tcp                 LISTEN               0                    32                                               0.0.0.0:2121                                    0.0.0.0:*                                        
tcp                 LISTEN               0                    80                                             127.0.0.1:3306                                    0.0.0.0:*                                        
tcp                 LISTEN               0                    4096                                       127.0.0.53%lo:53                                      0.0.0.0:*                                        
tcp                 LISTEN               0                    511                                                    *:80                                            *:*  
```

![图 46](../assets/images/2bacadd4de8f6fa004f4ec20cfe1a60efe3b8c64d056b015c6784fd40f919a8a.png)  
![图 47](../assets/images/7e85b9085a545974bb1b717d600817730d489505801a7ba441c51bba8667387e.png)  
![图 48](../assets/images/32fea853e57765f878f61f04e4641e07500f5b4cec4810c644ba0670290133ce.png)  
![图 49](../assets/images/a1adbc9b86b7f5b55e53d33da2c10bdba9c335aea6531116025eeb40f0d1044a.png)  
![图 50](../assets/images/4086d0f48a56a0c2db3538fd2049a9c5ed0db3bb0b01b32f533ea373d83e4ae8.png)  

>豁突破口
>
![图 51](../assets/images/9f9d3cb17678032673222d8e6a4b928d39a6f2be86506f1e5ab96f78945049f5.png)  

>一切计划之中果然是密码和找ssh端口
>
![图 52](../assets/images/ec50004a5e12a6520d7c8212704326f60a90c7724243ac5496a7776674645b10.png)  

>我有一个问题他里面有lxd能不能直接提root，先常规把
>

![图 53](../assets/images/1d8b322ddc2b0ebc0f3ac2e049880a1565f14320e27b52481393b3c5ead46b17.png)  
![图 54](../assets/images/c1d34fb04ab1041791315c2f3dd1a11a36d310dffb5688e7407cf7f8936d5bbd.png)  

>有点无语
>
![图 55](../assets/images/17a0c10ac44196bf0e644f00fb6ddbd2f272e856d4b6b4b06530711d08d37377.png)  

>研究研究，思考一下，好像明白了一直写错东西
>
![图 56](../assets/images/93da11e32304c8e16908850f56f19975731f61e6b834d9837862b2628afe16e9.png)  
![图 57](../assets/images/3e62f80eeeb2fb09754a3533720c40aa22a85d48f2f3b02096fba22a5130a54c.png)  
![图 58](../assets/images/af391ed970525417e146a0eee463d21cb2941df035983a3ca215885d26a75b8e.png)  

>果然是路径问题，他是利用juan的用户找我们注入的文件，所以得去tmp
>

![图 59](../assets/images/c29274d41df0ebacd4d8be4fec6a0b7e525d00c37f7085f2da0478a662e28af9.png)  
![图 60](../assets/images/dad8ad01ad08a6bea1154aa101de628bc323e1ea1becfd21a66ae811fa09baa1.png)  

>太easy了直接王炸就行了，ok结束
>

>userflag:goodjobbro
>
>rootflag:19283712487912
>
