---
title: hackmyvm Zday靶机复盘
author: LingMj
data: 2025-04-19
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.92	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.089 seconds (122.55 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.92           
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-12 20:23 EDT
Nmap scan report for zday.mshome.net (192.168.137.92)
Host is up (0.0052s latency).
Not shown: 65524 closed tcp ports (reset)
PORT      STATE SERVICE  VERSION
21/tcp    open  ftp      vsftpd 3.0.3
22/tcp    open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 ee:01:82:dc:7a:00:0e:0e:fc:d9:08:ca:d8:7e:e5:2e (RSA)
|   256 44:af:47:d8:9f:ea:ae:3e:9f:aa:ec:1d:fb:22:aa:0f (ECDSA)
|_  256 6a:fb:b4:13:64:df:6e:75:b2:b9:4e:f1:92:97:72:30 (ED25519)
80/tcp    open  http     Apache httpd 2.4.38 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.38 (Debian)
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      37191/udp6  mountd
|   100005  1,2,3      43361/tcp   mountd
|   100005  1,2,3      47359/tcp6  mountd
|   100005  1,2,3      58337/udp   mountd
|   100021  1,3,4      32876/udp   nlockmgr
|   100021  1,3,4      33341/tcp6  nlockmgr
|   100021  1,3,4      43183/udp6  nlockmgr
|   100021  1,3,4      44673/tcp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
443/tcp   open  http     Apache httpd 2.4.38
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Apache2 Debian Default Page: It works
2049/tcp  open  nfs      3-4 (RPC #100003)
3306/tcp  open  mysql    MariaDB 5.5.5-10.3.27
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.27-MariaDB-0+deb10u1
|   Thread ID: 89
|   Capabilities flags: 63486
|   Some Capabilities: ODBCClient, SupportsCompression, Speaks41ProtocolOld, LongColumnFlag, SupportsTransactions, IgnoreSigpipes, InteractiveClient, FoundRows, Speaks41ProtocolNew, Support41Auth, DontAllowDatabaseTableColumn, ConnectWithDatabase, IgnoreSpaceBeforeParenthesis, SupportsLoadDataLocal, SupportsMultipleResults, SupportsMultipleStatments, SupportsAuthPlugins
|   Status: Autocommit
|   Salt: P\]>];NWIFdFo~L?5+w9
|_  Auth Plugin Name: mysql_native_password
43361/tcp open  mountd   1-3 (RPC #100005)
44673/tcp open  nlockmgr 1-4 (RPC #100021)
53035/tcp open  mountd   1-3 (RPC #100005)
54119/tcp open  mountd   1-3 (RPC #100005)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Host: 127.0.1.1; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 28.31 seconds
```

## 获取webshell

>好的端口，一个一个看
>

![picture 1](../assets/images/d6754fbc06ae7c80ad8aac3cc63cc5b5d4bdf96f53ff7c66f1523c90bacccc36.png)  

![picture 0](../assets/images/797851b42872cbac77d161cdee322c0cb51b896553ea2f06f34df7d018dcd1a6.png)  
![picture 2](../assets/images/156f2249884443f803c60297925ad619a6cfe5d68f7ab06110c7a75bd6cbc7a7.png)  
![picture 3](../assets/images/7f3cb41a79a831292ed8f08807337aa14c34495efec13ebd38bf915a323844df.png)  

>默认账户密码
>

![picture 4](../assets/images/2179f8b0bc3767f98b05a94ad410a69b4a37fcaa1c4ed3fd94d57006ddb439a8.png)  
![picture 5](../assets/images/35c86d93546a28cc54133280fd78510c6f4980f7a0cfd8ebf752229429381674.png)  

>没见上传成功
>

![picture 6](../assets/images/fe0e0cc19e67bcf0344c3e83e4a1e9976d87253429be19d3fbfc411e0631cc01.png)  

>点一下保存即可
>

>没见server在那需要用curl？
>

![picture 7](../assets/images/a490f52d29a3e1357b32bc470a7e5bc3709652ce53fe397e60e6959152ab46ca.png)  

>无，看看msf了
>

![picture 8](../assets/images/b7a2da96fe28c0fd92a155964bd9e994207a0373d8b7a5d65af501d0fe32628d.png)  

>目录是有的，看看扫出来行么
>

![picture 9](../assets/images/54ffe777a57f7ea4a086da2210d9e669d925f413086fca3ffb0464fca633aed9.png)  
![picture 10](../assets/images/b751f1eeaf869321c4f66632eff7e36c342bdea714276a3857e751da318cb51b.png)  
![picture 11](../assets/images/3e08dde9211b3bc3d10f89662302a391205078adb70e1242b988ae97c28dbf78.png)  
![picture 12](../assets/images/0287c1fc2c04a5a226c0848f75926e07399d06f26d08b7070b06631bbd4fe0a9.png)  

>压根没有ipxe这个目录
>

![picture 13](../assets/images/be75b3475455eb0982660a88b60940acd0a14a65d1cddf03625d84994c1a88c0.png)  

>不对啊作者也是它啊，为啥没有这个目录，难道方向不是这个，看看2049
>

![picture 14](../assets/images/d2d7b27bf5b33df6cb8768146c1cada56ab2a5df7ee4b8fd21784683761422aa.png)  
![picture 15](../assets/images/2dece0a638a0fe18224a17bb633a2383b47de16f4af795edc04fd13d92988e0d.png)  
![picture 16](../assets/images/5aa8e814fb184d8162aef3b7d30ce5a6079f02f3eb874c4a7b70150346519d5c.png)  
![picture 17](../assets/images/08fbd3202611b2bb08351c56e3915a1cc7b14e89b006663491030d4965c10da0.png)  
![picture 18](../assets/images/19be01ecc9a0f6a74b1deb1d8cf7160b4b55d12d87651b2fc74518631db4601e.png)  
![picture 19](../assets/images/d1104e1cc02fb743b5d63debda4e22cf9fa26cb707e255661b4df0efdcaaa612.png)  
![picture 20](../assets/images/0ef9dbe05914ad31776785c6670e61026ee32d23897c7b67678254029c718705.png)  

>咋触发呢
>

![picture 21](../assets/images/021d2b01a033cb659cca782565f71902411a5c27e7dd187b262bad811abf32d5.png)  

>这样触发么
>

![picture 22](../assets/images/b8a9dd12725ee700990e391c35cb83d4e16bee20ad6e2f92ca3e3fb9c73e1fc2.png)  

>找了半天了找到这个我以为是干images呢
>

![picture 23](../assets/images/a6272c9b040d352c425255be137ca1f03888aeecfed8061f68960dcc9372ae91.png)  

>ftp么？
>

![picture 24](../assets/images/0585fca207eef8f2d29a57fefbfab70e862af50a5f2fa8a52072d0668b918ae0.png)  

>可以登录我直接创建.ssh
>

![picture 25](../assets/images/fb152108c54cdea5a7e0a26ab72e92fd506bbe1cfe645b7b3f83ac928f5df2b2.png)  

![picture 26](../assets/images/c0b1b52209c908ee2cb3da97bf93350faa40c6a8a093d03c297cd751c69c9ef9.png)  

>不对我傻了，删掉sh就好了
>

![picture 27](../assets/images/547fe9332d938709773c24a0d01d3443100b1aa96e527e08df97dde79080de59.png)  

>作者的恶作剧么，
>

![picture 28](../assets/images/0721f429bd8b8d00e82dba6553b4f774534e0a9f8f3609e992998b78e549dc85.png)  
![picture 29](../assets/images/3766c233bb627ddb91c58a89d71a66378a869687defb7b0f57a18ee7cdd561dc.png)  
![picture 30](../assets/images/813c91a09b6757f5436939f66dace34074ea961a7d3195462505b7d4129296c4.png)  

>文件还真不在，不过我有特殊权限直接创建
>

![picture 31](../assets/images/62f3f4cb92fe777aed4bc96d127cc3fc1e08df19ec65d9ab2170b274c620ae9c.png)  

>好了，可以拿shell了
>

![picture 32](../assets/images/3b9469ba9612221cfc129f297dc1dd61e259eade097fab8338d98dacc218cbab.png)  




## 提权

![picture 33](../assets/images/85aa2a99d1e697b473c7af2cab632172104472107987c3e7822e4ff5807951c1.png)  
![picture 34](../assets/images/fcca9c377113282a9b3b1b44564d56c36bda18ae2bef46759bec399598244707.png)  

![picture 35](../assets/images/8ce672f644281a06051bf73011b7e26829a7991c558c288f7e8fe65290312b8e.png)  

>无
>

![picture 36](../assets/images/c8b89d43232cf1f069dec7d214c9d96ed3bab6b8bb016c1dd29ffc53e25da6d8.png)  

>这也不给登
>

```
estas@zday:~$ /usr/bin/mimeopen --help
Usage:
    mimeopen [options] [-] files

Options:
    -a, --ask
        Do not execute the default application but ask which application to
        run. This does not change the default application.

    -d, --ask-default
        Let the user choose a new default program for given files.

    -n, --no-ask
        Don't ask the user which program to use. Choose the default program
        or the first program known to handle the file mimetype. This does
        not set the default application.

    -M, --magic-only
        Do not check for extensions, globs or inode type, only look at the
        content of the file. This is particularly useful if for some reason
        you don't trust the name or the extension a file has.

    --database=mimedir:mimedir:...
        Force the program to look in these directories for the shared
        mime-info database. The directories specified by the basedir
        specification are ignored.

    -D, --debug
        Print debug information about how the mimetype was determined.

    -h, --help
    -u, --usage
        Print a help message and exits.

    -v, --version
        Print the version of the program and exit.

```

>好像要使用图形化界面
>

![picture 37](../assets/images/f5366608336b2e70bbd064cb2d3452f7e5a6b6ef6cb59de2b3b60577be6fc641.png)  

![picture 38](../assets/images/25c7937143e4d83405b8848a7b0397be31180d9cf885f048e041bd2a6c2587a5.png)  

>这个问题一直没解决，但是我能想象是这个方案，当然有读取方案
>

>我开始怀疑是不是这个用户提权的问题了，为啥这个一直不行，没有xauth么
>

![picture 39](../assets/images/12fd1690030359469daedfe27371c74acfe7ac67d674e855a76568ea5223be26.png)  

>我研究一下为啥登不上的问题
>

![picture 40](../assets/images/9106a725abe23c7a6b72ec22dcbb777c59ab27abd601695c4c7c7e424fb75fcb.png) 
![picture 42](../assets/images/bcd282e9da86631679b983e8341b47c9a34426d18c526107006ad9f19b054450.png)  

![picture 41](../assets/images/c866ed8fd94f206f1291e8506bc16409d0d8225aba6d4fa159c8dcc32c2ac0aa.png)  

>查了一下发现是bash的问题换成sh就好了
>

![picture 43](../assets/images/7c42c48196cd3aa16b2e9b8894d284b0c808ac56b2eb36dc25483e1cfe167f2d.png)  

>又是这个，好想能直接共享root
>

![picture 44](../assets/images/a0c1050b4c6ec8e82a7cf383da75cae728c7e68b06484218bbc7837973f946bd.png)  
![picture 45](../assets/images/7d3bf84a4493524761b5ff477874c267401a2a05ff09b2d5da9ad635f98fd17f.png)  
![picture 46](../assets/images/9f294f057fdef47623954a5ac98eab0579ca38f2d0c6fc7076428c65e720ab80.png)  

>奇怪我无法操作
>

![picture 47](../assets/images/1e187cf9f14ee7977cad87d0335db27e444525c128892bbfe0e846b2a043dc79.png)  

>我说我不能操作呢
>

![picture 48](../assets/images/a59ae70fb0bd52d6cf8b4a66a0b4e94d33c2bddd0eff3c234cf2f9e9fa857fa1.png)  

>好了
>

![picture 49](../assets/images/c479b9b34e64a1ea7dea0ef9fb8504460e1bdc51d7a826b392fe229090e0583d.png)  
![picture 50](../assets/images/6656369c738a35db0bc222e7954939126f03a788de515231d3a546be3e7d8ce0.png)  

>忘了不能bash了
>

![picture 51](../assets/images/760bb30743d27a2baeab56a5bb5532c63369fa298b018a69d4d08050db7b4aa6.png)  

>用这个用户就行了，还行挺有意思,差点搞坏我的终端，重启了
>



>userflag:whereihavebeen
>
>rootflag:ihavebeenherealways
>
