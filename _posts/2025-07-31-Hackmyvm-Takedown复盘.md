---
title: hackmyvm Takedown靶机复盘
author: LingMj
data: 2025-07-31
categories: [hackmyvm]
tags: [upload]
description: 难度-hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:fb:0f:16, IPv4: 192.168.137.194
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.62	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.132	2e:5c:af:d4:ea:c8	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.116 seconds (120.98 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- 192.168.137.62                                                                        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-30 21:48 EDT
Nmap scan report for osiris.mshome.net (192.168.137.62)
Host is up (0.029s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 14.77 seconds
```

## 获取webshell

![picture 0](../assets/images/b781c8496a5e67a1e48815fb2ad6183b89223e7a9b41d0a0b96f441a2ee347a1.png)  

>有域名
>

![picture 1](../assets/images/bfcbe253089345dee1b5ed7298d29d3442c54ae59ab8d0e6ace83f1d92e34cd3.png)  

>也存在子域名
>

![picture 2](../assets/images/ddb699f156b88dc6988881e0d373f6cabc2c9655d9bc92befc1b1a27e6b3369a.png)  
![picture 3](../assets/images/bccb9c7d014cac779cf76cf82bfa8dc7d718371b4275b86e09a5689847c58450.png)  

>没有pin，只能先对submit进行注入
>

![picture 4](../assets/images/0b6a0b3684d783cbb56acd7275618f552ce335f30ddebc8bf30610fae5a83d47.png)  

>尝试模版发现是模版ssti注入
>

![picture 5](../assets/images/c9486c13a0fd1cc21531ea64d4bf28e31b09569abb8cbd0335833cf0dc8b23ba.png)  
![picture 6](../assets/images/9d2dc651837e10cc1c34314c201df8142f7f02b3b515dd75e96482477ecb61de.png)  

>OK拿到shell了
>


## 提权

![picture 7](../assets/images/06b5deccf30f64406ce00585852ca714a59ff3d9dfa0087cac3c193134235edc.png)  
![picture 8](../assets/images/343b05f611d12f0810356f79fe0d106a6c072d43edc36863380dd51f0752ca6c.png)  

>日志里有东西，是一个5分钟定时任务
>

![picture 9](../assets/images/98cec4326c9ddb27a64e22e74fca6d8617c724d3e4598848c860a4f19664810c.png)  
![picture 10](../assets/images/b4a9e07efba583f49d6efbad6c13d3f89532cc40b7f214c4318386fcef1fc226.png)  

>存在一个挂载
>

![picture 11](../assets/images/f1785d35d117f29f1a5ac4c51fe66cf8e8861c9ffc067ff56ef8366cf5c2e81c.png)  
![picture 12](../assets/images/6728c000ae0c733b5a3af8af2d497c81cdb24fa361e82e1d581d049b4825c9a7.png)  

>好了5分钟后弹回来了
>

![picture 13](../assets/images/9056da8210c05db6736054f4cb207367905d547b7641076d11d7c0b0e9b24c1d.png)  
![picture 14](../assets/images/0a44bb9292db11583704febb496ea6757eb2c536845bba75937fbe79108e05d3.png)  
![picture 15](../assets/images/6ca498b8d8537013fd1902da2a8b2cfd12933e3dd2b5200404f9e57300cf5bda.png)  

>不能死要登录，可以改用run
>

```
# -h
Command not found: -h
# sas -h
Available commands:
sas_call - listen to Services
ls - List files and directories
cat <filename> - Display contents of a file
whoami - Show current user
sas help (-h) - Show available commands
version (-v) - Show application version
run <filename> - Execute a file
dir - Show the content of the current directory in wide format
```

![picture 16](../assets/images/d3c0bb435459fa86fc71265422ec682f15cd7a88ddedc024df2ff266c89ffbe3.png)  
![picture 17](../assets/images/efb8e4cf9bbbeac1476e5cf8351ba803f9a36b58f5ecd8d4e05c6bf6988e893b.png)  

>拿到shell了
>

![picture 18](../assets/images/174e1b88b6e4a66373352af68f8e9545dcb70c87e47bc69cef4548e6dc5a893f.png)  

>这是一个密码学的东西
>

![picture 19](../assets/images/dbdc17ec607b0959b9a01f8b391ec0c9fabf97690341f1558f5f3deb918ce4d2.png)  

>昨晚已经有大佬完成了解密
>

```
>>> from Crypto.PublicKey import RSA
>>> from libnum import *
>>>
>>> key = RSA.importKey(open('publickey.pub', 'r').read())
>>> key.n
91451963281284582263822096491513116919368195592939782118118773662653066690833
>>> key.e
65537
>>>
>>> p, q = 272799705830086927219936172916283678397, 335234831001780341003153415948249295589  # use factordb
>>> d = invmod(key.e, (p - 1) * (q - 1))
>>> c = s2n(open('secret.enc', 'rb').read())
>>> n2s(pow(c, d, key.n))
b'\x02\x8fx~\x04\xdc;\x19\xbd\x99\x10\x96\x00sh1m0mur4Bl4ckh4t\n'
```

>这是解密payload，当然也有wp去看：https://pepster.me/HackMyVM-Hell-Walkthrough/#RSA%E8%A7%A3%E5%AF%86
>
>不过是之前hell靶机的复现
>

![picture 20](../assets/images/9c3d6a678f09c2dba088fc5355235206f7f0906c44492017e7168e0afb4293c1.png)  

>需要按照一下库
>

```
#!/usr/bin/env python3
from Crypto.PublicKey import RSA
 
with open("publickey.pub", "r") as f:
    key = RSA.import_key(f.read())
    e = key.e
    n = key.n
print("[+]e==>{}\n[+]n==>{}".format(e,n))
```

![picture 21](../assets/images/42b4baec21f16db65b1b700939e4a82f7180ebd03938c595cdc25ba464493365.png)  

>需要利用一下网站
>

![picture 22](../assets/images/3136f90cc98e5c8225dfd017d73edd23260a67d5d84a39fac43e627d20fc92a9.png)  

>最后payload还是仿照大佬的payload写的
>

![picture 23](../assets/images/80fcde6acacb42c33069844d7285ee4199293c89cfb2510e1f019eaf820bb702.png)  
![picture 24](../assets/images/1dcd48964ffdfbdbee8b84769d2e5b5a24375a1eb861ca6ad2f437809e4da453.png)  
![picture 25](../assets/images/ed831ece6b2c5ecef3c0582f5b24ed9e22ff833893e4eddada47ab6e5850ad9d.png)  

>选第二个直接确定就进入vim界面直接拿到root权限
>

>userflag:612701a03669485d94bc687449fdab39
>
>rootflag:1e271c5ce97e76ae8417a95c74085fba
>