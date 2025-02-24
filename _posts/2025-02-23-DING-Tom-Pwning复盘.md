---
title: DING-Tom Pwning靶机复盘
author: LingMj
data: 2025-02-23
categories: [HomemadeDrone]
tags: [Pwn]
description: 难度-Medium
---

## 网段扫描
```                                      
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:0a:12:7f       (Unknown)
192.168.56.162  08:00:27:91:56:86       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.865 seconds (137.27 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.162
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-22 21:57 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.162
Host is up (0.0015s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 93:a4:92:55:72:2b:9b:4a:52:66:5c:af:a9:83:3c:fd (RSA)
|   256 1e:a7:44:0b:2c:1b:0d:77:83:df:1d:9f:0e:30:08:4d (ECDSA)
|_  256 d0:fa:9d:76:77:42:6f:91:d3:bd:b5:44:72:a7:c9:71 (ED25519)
80/tcp   open  http    Apache httpd 2.4.59 ((Debian))
|_http-title: Don't Hack Me
|_http-server-header: Apache/2.4.59 (Debian)
6666/tcp open  irc?
|_irc-info: Unable to open connection
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port6666-TCP:V=7.95%I=7%D=2/22%Time=67BA8EDE%P=x86_64-pc-linux-gnu%r(He
SF:lp,25,"\n\[!\]\x20\xe6\x8d\x95\xe8\x8e\xb7\xe4\xbf\xa1\xe5\x8f\xb7:\x20
SF:11\xef\xbc\x8c\xe6\x9c\x8d\xe5\x8a\xa1\xe7\xbb\x88\xe6\xad\xa2\n");
MAC Address: 08:00:27:91:56:86 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 50.52 seconds
```

## 获取webshell
![图 0](../assets/images/4b303f6655f2d0b42828101aa4a43a69a6521ba50ea980eb54a0d4d8886bdf9b.png)  
![图 1](../assets/images/4dc4f165faa7d4b8daa4bdae150f60838e2bb1492fae592f9a20b8c919e591fc.png)  
![图 2](../assets/images/d9504c32b5acb45bb8854565dd49f69ffd2b5fead9be29e8e591e8e6d49342d6.png)  

>枚举目录么，我直接爆破一下吧，但好像测试服没加其他字典奥
>

![图 3](../assets/images/a69cbc32a051749f48b6935ff8b27e5a976b0420445d6512077d7f6ead4cf39a.png)  
![图 4](../assets/images/727363104ffc1a4ba42f6284d27aabe4c168d4526b8bbd5962ac452ba0c2fa22.png)  
![图 5](../assets/images/b0b12dba4eb39aaa2bd8c45cad831907262c29f8a266ce3a7a66de9c45412cc4.png)  

>端口么？我只见一个6666还访问不了，他不能连接目前看
>

![图 6](../assets/images/d9be4783defbdcf81f7d699257c4025e53e433e24c75eb28f442fefee556edb9.png)  
![图 7](../assets/images/0dd28604ef48456621e3d7c186c569bde127681c441a212c79937fa4dda76916.png)  

>好像只能在6666这块花费功夫了
>

![图 8](../assets/images/ea4a78b0e6bdc4c4d965e89e0a4690e1ecb272ca66bc65966cd1acb7b1ba6181.png)  
![图 9](../assets/images/76605f66f1b0340c3ab0ede500c2c13c222c7bcf4c5aedd0c9c9c167da7b1b99.png)  

>没想法，看来前面这个入口我得花点时间了
>

![图 10](../assets/images/52b96cf7d6a8010e7eb6f0dd37c84b69e2b3cccdc1afceb4b7bbe4d7c7625267.png)  

![图 11](../assets/images/1b59a52efef700153c760dea9a0f617825380eb979f5dd64b78d7f6f5b17dc9f.png)  

>去看wp了，我把整个流程看了，现在自主复盘一下，我对于pwn部分实在是不会，所以只能这样了不过改找的信息都找了一下
>

![图 12](../assets/images/242fc9e796ec41a700744491302fd9af75ebb3f19284d94db8bc781a69c33bf1.png)  

>熟人嘛，无非就是一些群友或者群主，这里试过是群主
>

![图 13](../assets/images/af2b7351a5952921b9e5b9b40909ce3dde3df30a980097a1239fe03d67c038d8.png)  
![图 14](../assets/images/a9fbd96c39eb959d82c8426c720c142130a8ac123379c53f8e5eec024663d91b.png)  
![图 15](../assets/images/188fea0debf6de74a39a0ee5d6b6595e8ce78f8eae8efbe108568e2561ffffad.png)  

>拖去ida，看看，它是一个c++的代码我看c的伪代码都费劲，更别说c++了不过大体逻辑还是知道就是不知道咋用
>
![图 16](../assets/images/c2eef9dee73a68c637bb1a1afb60582501945a937ef4937ae40d4a0fba31ec45.png)  

>这里是一个函数的地方，判断等一就退出的好比它是禁用字符什么的
>
![图 17](../assets/images/8362aaeb750e9875b60c26ec02ca523807b4e779f6e47c594352ba1ca7d93444.png)  
![图 18](../assets/images/ac6b7c55e758134f5877e6d41374389f67698f0322087cc01fdb18bcf2891570.png)  

>接下来就是函数调用
>

![图 19](../assets/images/0f594e36fb99c18374fd058a9a288f94b72c072bf2c9ed7656028657183ef942.png)  

>当你上面的通过会有一个v18的值进来传递到v22然后你就会调用v22完成函数
>

![图 20](../assets/images/839fb2ba0c3b14a351bd149982a383debb4280f318a2bcbb91dfe205bcddaebe.png)  

>这里可以看到我输入空格被干掉了但是会进行客户端连接，只要写一个绕过这个空格的反弹shell的16进制就能shellcode，作者给了payload方案奥
>

![图 22](../assets/images/5fdc63597af900210a4ec5fa1721cecb80bc16592d3c4b08d6d28158e356f290.png)  

![图 21](../assets/images/7eb87d283230d99e391a44132e578407c7b48d9b8fa711bc0bce1cee558c2985.png)  

![图 23](../assets/images/c29ffef9250f417f774b0b7c0bbb0d21fe3fdf50f329bafc62f3581bf4d4e6f2.png)  
![图 24](../assets/images/fb49fe8a97f4945af7e45def8ec103b0221103ae935e8068cdf36da226e173cb.png)  

## 提权

```
lamb@pwnding:~$ ./key 
54287lamb@pwnding:~$ cat note.txt 
There is only one way to become ROOT, which is to execute getroot!!!
成为ROOT的方法只有一条，就是执行 getroot !!!
lamb@pwnding:~$ cat this_is_a_tips.txt 
There is a fun tool called cupp.
Are there really people that stupid these days? haha.

有一个很好玩的工具叫做 cupp.
现在真的还会有人这么蠢吗？haha
```

![图 25](../assets/images/6ad4a7068190b88f24bc3f6a8b1c4be56913fd7f583216a6d3889607a50b7986.png)  

![图 26](../assets/images/55bfabcd29cfa94ea784465b4ee158b3d00060d52e2971bb9d38abe00eedcd67.png)  

>找密码吧上面有一个cupp的提示
>

![图 27](../assets/images/430180162e05a5908ead58e89f8553e461be45141a37b390a7b3fc788e6bef98.png)  
![图 28](../assets/images/b6832acc5bb84ee6a1e8a04e671794e5c7b62a78341c08f8a1e2eabf74718534.png)  

>我的suforce用不了我用的是sucrack的密码爆破形式
>

![图 29](../assets/images/791192293ee2a0e89b0855d584109c92acff2a1eb9f68f2fb035add493c19fa1.png)  

![图 30](../assets/images/d597177ee007612e068a63b610bdfae60700e47e0418be21e9265057902790b7.png)  

>存在隐藏文件所以可以去利用一手
>

![图 31](../assets/images/9e190d79f2aba453e964b20cf6f743cab5434d9aee1a0a4c9043769a65fcae07.png)  
![图 32](../assets/images/bf242b952c06b519065515719030341920b28c2e8dc9a5aa82f8045c71632876.png)  

>找到密码了
>

![图 33](../assets/images/3de00cec83af68e59ffee5e24e15dc058a4dee18ea55a8951a1ed2d852c6dce4.png)  
![图 34](../assets/images/558cb886f13887f5c07f2ec25fafc05497dea28d266c1eef9fc5c7980ffa0d11.png)  
![图 35](../assets/images/b416a8dd6bcd20760abd1ac3bd40202391da0e9443150cf3e0a119a6fb6b73f4.png)  
![图 36](../assets/images/6a5def0851232a54276434cb8e7528ff0c42e51a309be0b4d6778f69a50cd095.png)  

>密码就是哈希值不是12345，好了结束是一个非常好的靶机感谢DING Tom的靶机制作与提供！！
>



>userflag:flag{祝你新的一年开开心心啊!}
>
>rootflag:flag{7h4nk-y0u-f0r-pl4y1ng!!!}
>