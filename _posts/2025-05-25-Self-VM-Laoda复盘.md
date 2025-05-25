---
title: Self-VM Laoda复盘
author: LingMj
data: 2025-05-25
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~# arp-scan -l       
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.8	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.123 seconds (120.58 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.8
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-24 20:20 EDT
Nmap scan report for Laoda.mshome.net (192.168.137.8)
Host is up (0.038s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: \xE6\xB0\xB8\xE8\xBF\x9C\xE9\x93\xAD\xE8\xAE\xB0\xE7\x89\xA2\xE5\xA4\xA7
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.75 seconds
```

## 获取webshell

![picture 0](../assets/images/aaf9b9c2621a96827c22e205538669bbd23e7fd725ecad19d4c452a5e3d52f20.png)  
![picture 1](../assets/images/6e22b24cf3cee9d4aec1d3dd7c52a1bb629c03a2d261e979cd48da3df18ff5c5.png)  
![picture 2](../assets/images/ec1f2a1076b7741b67c41be1c81223d0647710a01a2f4f3a5b830ff17f17e296.png)  

>这里已经给了提示，你可以进行对应的file和php filter的操作
>

![picture 3](../assets/images/821c36839c419df17970297fe0a20e6fec3676351f1c12b6834d0d520a62bf4b.png)  

>所以这里有2条路完成前面webshell的提权操作
>

>第一利用php filter进行webshell获取
>

![picture 4](../assets/images/7c0522051a26c4228d7814506dbbad380a6ecb799169f914e0a05e576d9b0d32.png)  
![picture 5](../assets/images/44529404b540c7a70260332bf6753bd76be9a811be94ee9111193f6e6aa0f23c.png)  
![picture 6](../assets/images/2beb9222d7b6a28dc5b7a849c9825a787cbd2d627662b371b153f6955d8ac820.png)  

>方案二 file读取用户信息
>

![picture 7](../assets/images/20f93eee42adbdca53a22f1498dc1a96c68b74de60c51f872542019cbef9ab39.png)  

>可以发现c1rus的目录可以有读取权限
>

![picture 8](../assets/images/3deed769d4396594b99ba690a251604f6e8168ea0f6f3aab20e422d74ab6730b.png)  
![picture 9](../assets/images/87076dd5546d78bcc7bf04e5e34d29980536921f22162ff43517ed1704c8847a.png)  

>这里就出现用户账号密码了
>
>当然hydra也是可以的
>

![picture 10](../assets/images/f6ed44525af762a66c997852fc0842ba27e0dcb8355f042c1701834857ce389a.png)  

>所以获取webshell的方案很多
>

## 提权

![picture 11](../assets/images/b8210571ef700db23ccd8fb0eb9f2c7481db81caeb690eaeeb281f8fef350c75.png)  

```
VIM - Vi IMproved 8.2 (2019 Dec 12, compiled Oct 01 2021 01:51:08)

Usage: vim [arguments] [file ..]       edit specified file(s)
   or: vim [arguments] -               read text from stdin
   or: vim [arguments] -t tag          edit file where tag is defined
   or: vim [arguments] -q [errorfile]  edit file with first error

Arguments:
   --			Only file names after this
   -v			Vi mode (like "vi")
   -e			Ex mode (like "ex")
   -E			Improved Ex mode
   -s			Silent (batch) mode (only for "ex")
   -d			Diff mode (like "vimdiff")
   -y			Easy mode (like "evim", modeless)
   -R			Readonly mode (like "view")
   -Z			Restricted mode (like "rvim")
   -m			Modifications (writing files) not allowed
   -M			Modifications in text not allowed
   -b			Binary mode
   -l			Lisp mode
   -C			Compatible with Vi: 'compatible'
   -N			Not fully Vi compatible: 'nocompatible'
   -V[N][fname]		Be verbose [level N] [log messages to fname]
   -D			Debugging mode
   -n			No swap file, use memory only
   -r			List swap files and exit
   -r (with file name)	Recover crashed session
   -L			Same as -r
   -A			Start in Arabic mode
   -H			Start in Hebrew mode
   -T <terminal>	Set terminal type to <terminal>
   --not-a-term		Skip warning for input/output not being a terminal
   --ttyfail		Exit if input or output is not a terminal
   -u <vimrc>		Use <vimrc> instead of any .vimrc
   --noplugin		Don't load plugin scripts
   -p[N]		Open N tab pages (default: one for each file)
   -o[N]		Open N windows (default: one for each file)
   -O[N]		Like -o but split vertically
   +			Start at end of file
   +<lnum>		Start at line <lnum>
   --cmd <command>	Execute <command> before loading any vimrc file
   -c <command>		Execute <command> after loading the first file
   -S <session>		Source file <session> after loading the first file
   -s <scriptin>	Read Normal mode commands from file <scriptin>
   -w <scriptout>	Append all typed commands to file <scriptout>
   -W <scriptout>	Write all typed commands to file <scriptout>
   -x			Edit encrypted files
   --startuptime <file>	Write startup timing messages to <file>
   -i <viminfo>		Use <viminfo> instead of .viminfo
   --clean		'nocompatible', Vim defaults, no plugins, no viminfo
   -h  or  --help	Print Help (this message) and exit
   --version		Print version information and exit
```

>这里看参数可以知道它是使用vi这个编剧器
>

![picture 12](../assets/images/79f6ac04b1ca96ab4c85864aec4935642f8f8b39a96eaa28f80fb264089fb5b8.png)  

>当然原来名字是ex，我把他改名了而已
>

![picture 13](../assets/images/904724782a31557bfcbf6908af0764fbff2aca7cb7f10bef7edce013d7ab1808.png)  
![picture 14](../assets/images/0008f21d20fcf8e2cb2393be5d7e73690d068fce369d894ddf7ed81966be3a5e.png)  

>接下来是root提权，首先说明figlet是不能提权的，但是exiftool的提权方案很多，我这里介绍三种方案
>

>第一种传私钥的方案
>

![picture 15](../assets/images/c4db16a25bef62f25e2f2b2e82cf1941cc777fbadfecea69c5386d4f49c516fc.png)  

>首先讲user这个root的文件身份变成私钥的形式，接着我们拿能提权的私钥放进去,这里前提是找到root并且你有写身份文件
>

![picture 19](../assets/images/077f98d607de855d998a2aef204b57727005f2a316657284f80893b9c26aec1e.png)  

>有的但是没产生，但是有口
>

![picture 20](../assets/images/72026a7888057633ca07904f5d166636a261bac549dc9b52374a9367666379d4.png)  

>root原来有一个可以写的文件
>

![picture 22](../assets/images/741d3eefd696b117cc485ae8a7eace946874411ec09b5e494af32414c2bbfe67.png)  
![picture 21](../assets/images/c25cac44093a5c561d3237715dcd5280f2b4c59bd23e8664087ec7d8ba1a9c0c.png)  


>为啥呢，我检查是权限问题么，主要已经写进去但是没触发
>

![picture 23](../assets/images/f431392e881964a5c6ba87a7c1411b385ad2d375d24e69043d32dc77b274b5d8.png)  
![picture 24](../assets/images/a452efb0b44187ad3352d67b3c28956341094e35cd14763c2a97d5f95376dd78.png)  

>确实是文件权限问题，有点棘手
>

>鉴于找能写身份文件有点麻烦所以，我先第二方案
>

>第二个方案 figlet文件写入提权命令
>

![picture 16](../assets/images/424363812b95b29e59eb3f5a7c8a61551cb0b470ee89c17be74b0b9b2946436e.png)  
![picture 17](../assets/images/d8f95f439110b13c940a6bcd5f07a5f69cb32e8b5bf156e0f18c06f171f67453.png)  

>然后我给刚才的user文件给个写再推回来
>

![picture 18](../assets/images/24f5818b5543f25e9e3e6f777f372de7b0eb7b82c680f6abeaef9fc6b221cb05.png)  


>第三个方案 ld.so
>

![picture 25](../assets/images/85d6270dca53d14ab4423e949c79338839c1e055fba38809b7c019048db6b33f.png)  
![picture 26](../assets/images/b3f5533db26e8eb965b04c6c81f0002bed157aedec0798f988789fa6cacb6f74.png)  
![picture 27](../assets/images/a0f2a0798fc4b3d371f5d84d67239ef0a44093c64d1798bea091dd16a34af1ff.png)  
![picture 28](../assets/images/e6d2c1deb9df9b060ca10d935a769129444e3f06289171187f5e8f91490bbff7.png)  

>好了3个方案演示完毕，第一个方案肯定是可以的但是我应该是那个地方掌握不好
>

>userflag:flag{user-f870760d-3565-11f0-af96-000c2955ba04}
>
>rootflag:flag{root-04e184db-3566-11f0-a86c-000c2955ba04}
>
