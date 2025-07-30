---
title: Self-VM Tree复盘
author: LingMj
data: 2025-07-12
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
192.168.137.50	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.187	3e:21:9c:12:bd:a3	(Unknown: locally administered)

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.123 seconds (120.58 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.187
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-12 15:39 EDT
Nmap scan report for Tree.mshome.net (192.168.137.187)
Host is up (0.0074s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.49 seconds
```

## 获取webshell

![picture 0](../assets/images/4b7bf95863b62b05a70adf376e34fa47c6c306932f520f9b1f62f642ac19fffe.png)  
![picture 1](../assets/images/d5e5b63284c336dd2e8b1a661d4386f277a6a24559fb74452cb1db5fa5608188.png)  

>我一开始以为这里是sql注入
>

![picture 2](../assets/images/b3877bcc826069708b0bc5f7505a454c0e5a0849e62b403423639905957551a5.png)  

>然后这里有一个考点，不过我一开始没用直接爆破，这里我一开始用user通常字典没爆破出来，我反其道而行之把fuzz的字典放上去了
>

![picture 3](../assets/images/8465788f03bea0256197e009b43b2bcb4c228a8808d283f7f4e75f9dac9f59bd.png)  
![picture 4](../assets/images/54a0285c1f788900b5e08427d58d0ff807f833ed5ee6f30ec2d061baa4d0c439.png)  
![picture 5](../assets/images/86557e518b63fe33b34c18973b9acad6e5551c2b12abf6fd58281afa2bf88c12.png)  
![picture 6](../assets/images/b876641da62ea3deea6a14eff05542e45c59f8a4d5785b034950aa5be0cd7e62.png)  

>可以看到9万和21万的区别，然后我说一下考点考点是xpath
>
>不会的可以参考路径：https://xz.aliyun.com/news/7386
>

![picture 7](../assets/images/3b97dd00bfd9b461375af94200931cb245ae5cd29322ad16f74a8af2f4f775d9.png)  
![picture 8](../assets/images/c0b9acad6c48c531a8a3e8ae194ce89a483a085bb301c66f8e8cec2dffa2a178.png)  
![picture 9](../assets/images/1e72b62d4f3daba2d3c2d543994c72251b895fa6a2b83ed1f28280ab14a9efe5.png)  
![picture 10](../assets/images/c3ef63258d276ca2bcda7c180df0ddc39966f84c7e2a6968b870913cebd041c8.png)  

>'or'1=1和']|//*|//*['
>

## 提权

![picture 11](../assets/images/a70cc611c44112f4c4d56a1721e5523320b612a7a079e1da9ec1b51c3782ce5e.png)  
![picture 12](../assets/images/5d185f68a7d0deba5f913d7468225c8f6454dc21f1081032683360cb5077f5e3.png)  
![picture 13](../assets/images/7e7d7972b7267dc472ae99b6795f9e4d08e75f60035a3c8bc2c88258f0bb4596.png)  
![picture 14](../assets/images/bbb434d33c20083f3503df30fc7b5270e3ec364f6abe6976c8de2931b5281ad3.png)  

>拿到另外一个用户不过拿不拿无所谓的
>

![picture 15](../assets/images/c1079d4a9b5eff1a8b7db58fbf086195f3920bc5af4c040e1fb59ce60a557e93.png)  

>具有suid权限，这里有3个方案解决奥我会逐一给你们提供，先来最有可能卡住的
>

```
cnext@Tree:~$ /usr/bin/tree --help
usage: tree [-acdfghilnpqrstuvxACDFJQNSUX] [-H baseHREF] [-T title ]
	[-L level [-R]] [-P pattern] [-I pattern] [-o filename] [--version]
	[--help] [--inodes] [--device] [--noreport] [--nolinks] [--dirsfirst]
	[--charset charset] [--filelimit[=]#] [--si] [--timefmt[=]<f>]
	[--sort[=]<name>] [--matchdirs] [--ignore-case] [--fromfile] [--]
	[<directory list>]
  ------- Listing options -------
  -a            All files are listed.
  -d            List directories only.
  -l            Follow symbolic links like directories.
  -f            Print the full path prefix for each file.
  -x            Stay on current filesystem only.
  -L level      Descend only level directories deep.
  -R            Rerun tree when max dir level reached.
  -P pattern    List only those files that match the pattern given.
  -I pattern    Do not list files that match the given pattern.
  --ignore-case Ignore case when pattern matching.
  --matchdirs   Include directory names in -P pattern matching.
  --noreport    Turn off file/directory count at end of tree listing.
  --charset X   Use charset X for terminal/HTML and indentation line output.
  --filelimit # Do not descend dirs with more than # files in them.
  --timefmt <f> Print and format time according to the format <f>.
  -o filename   Output to file instead of stdout.
  ------- File options -------
  -q            Print non-printable characters as '?'.
  -N            Print non-printable characters as is.
  -Q            Quote filenames with double quotes.
  -p            Print the protections for each file.
  -u            Displays file owner or UID number.
  -g            Displays file group owner or GID number.
  -s            Print the size in bytes of each file.
  -h            Print the size in a more human readable way.
  --si          Like -h, but use in SI units (powers of 1000).
  -D            Print the date of last modification or (-c) status change.
  -F            Appends '/', '=', '*', '@', '|' or '>' as per ls -F.
  --inodes      Print inode number of each file.
  --device      Print device ID number to which each file belongs.
  ------- Sorting options -------
  -v            Sort files alphanumerically by version.
  -t            Sort files by last modification time.
  -c            Sort files by last status change time.
  -U            Leave files unsorted.
  -r            Reverse the order of the sort.
  --dirsfirst   List directories before files (-U disables).
  --sort X      Select sort: name,version,size,mtime,ctime.
  ------- Graphics options -------
  -i            Don't print indentation lines.
  -A            Print ANSI lines graphic indentation lines.
  -S            Print with CP437 (console) graphics indentation lines.
  -n            Turn colorization off always (-C overrides).
  -C            Turn colorization on always.
  ------- XML/HTML/JSON options -------
  -X            Prints out an XML representation of the tree.
  -J            Prints out an JSON representation of the tree.
  -H baseHREF   Prints out HTML format with baseHREF as top directory.
  -T string     Replace the default HTML title and H1 header with string.
  --nolinks     Turn off hyperlinks in HTML output.
  ------- Input options -------
  --fromfile    Reads paths from files (.=stdin)
  ------- Miscellaneous options -------
  --version     Print version and exit.
  --help        Print usage and this help message and exit.
  --            Options processing terminator.
```

>阅读手册也是很重要的
>

![picture 16](../assets/images/1794a7c8055d330a39ce870941819fd8c3620174ed97c71695fea8cf3106157f.png)  

>可以读flag
>

![picture 17](../assets/images/d4eb1acd4ef5b3c14e6f225454e8b60c46a91828e2adabba4c6e06b5c51620f8.png)  

>声明读flag不是方案，这个是留给新手的小孩模式，所以我们要做的是获取root shell
>

>第一方案是之前做过的/tmp/pe.so方案
>

![picture 18](../assets/images/d4d0c6fa4c0f81f207d82aad38613772f5cb2548a7cbae599723040bc01015de.png)  
![picture 19](../assets/images/5efea919717c028bbcab45a999a9c5b7090aa81658f59151155bb1c7a7641ebb.png)  
![picture 20](../assets/images/2153eb5b56593bff72cd8b4b360cf31cf420c19efba442d7dab86b1ce60fce17.png)  
![picture 21](../assets/images/97737a6ce7caf5a86833c8d62306c0e68b7253d9c084ddcda3ef98ed2c2712c8.png)  

>报错了
>

![picture 22](../assets/images/5f09f9b7743f377cbc47040c6b99a64dea7a593272a90cfbb674a504b908b65a.png)  

>有点玄学奥先下一个操作
>

![picture 24](../assets/images/f8c482e2ae3074cdf9076611da94eb4b2307d61b2ac6c68ab33ff874cd43ff18.png)  

>研究出来了是+x的问题去掉就可以了
>

![picture 25](../assets/images/2b961384d702f3e72e6a2b1651e44182186d5d87c2b82d8fa72460c2987af8a4.png)  

>不过说我没权限是什么问题
>

![picture 26](../assets/images/d4a9f40658a22332ea3f934ec1f7f37fa1993967b19fe2e9e7a777ca5720a343.png)  

>退出重新登录就好了
>

>下一个操作是sudoers的写入，因为是覆盖所以只有一次机会
>

![picture 23](../assets/images/14091997cd9455cd611e2b5a9e9ac59effabc4269a166fce895d7a1c6cf3b893.png)  

>不过很简单所以小心一点就行
>
![picture 33](../assets/images/81ad3360d4894b74ad482491822d024fd9d3931f22685ef9f987b7b0cc319150.png)  

>mkdir可以去掉空格问题
>

>进去把之前错误的删掉重新写入
>

![picture 27](../assets/images/f3a3cb8473355efcf3811518a44efc747a1cc6f589d92ae011404e8c3c411217.png)  

>继续变回来奥，最后一个方案是找密码的
>

![picture 28](../assets/images/ba70ca88a1ed5f108286f6eef9e9801cf23449850ecbc7c19274d3047a60135d.png)  
![picture 29](../assets/images/fa3cdfec44cb5fd613beb95f52e329c8611defaaa6efeafa8521db36aa54b8ce.png)  
![picture 30](../assets/images/94c25e1a4cb9d0f82e3ce971413bddab01023c909b4212629a4cc8a2b96d1b7b.png)  
![picture 31](../assets/images/dbb03f6c7de78621fcc9ef0c867d0339a6348fc58158db2261dca95f79d43244.png)  
![picture 32](../assets/images/c254b918f294f89470b8843aa43cc047072ef48777aa93753cd7b7375a70341f.png)  

>多加几个方案
>
>写私钥
>

![picture 34](../assets/images/1677b21e240825c5d06ee96cee7c321d29af4c6598f9025980434945385c6bcd.png)  
![picture 35](../assets/images/9de73ae63d0352f38543e7f13465334ad06d68cb8a05bbeb3647c01bc13f4e8c.png)  

>为啥没成功
>

![picture 36](../assets/images/f1060e96f29cd661f63c2d6e263e1c93f0c3dbee1aa6613b4d905800b635790a.png)  

>多了点东西
>

![picture 37](../assets/images/915b4bdc1fc54973f0cb8197b25c3f24c14aa56cf382603f6754fcfc721ce579.png)  

>颜色问题加个-n即可
>


>不知道什么时候跑完，一时半会应该不会，然后我总结一下，你可以看到提权的方案很多，别只顾着获取flag，因为提权的主要目的就是为了让你去了解和学习这个命令，发挥脑洞吧
>

>userflag:
>
>rootflag:
>