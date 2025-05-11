---
title: VulNyx Carlam靶机复盘
author: LingMj
data: 2025-05-10
categories: [VulNyx]
tags: [upload]
description: 难度-Easy
---


## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.106	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.129	3e:21:9c:12:bd:a3	(Unknown: locally administered)

10 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.156 seconds (118.74 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-09 23:11 EDT
Nmap scan report for carlam.mshome.net (192.168.137.129)
Host is up (0.071s latency).
Not shown: 65525 closed tcp ports (reset)
PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 7.7 (protocol 2.0)
| ssh-hostkey: 
|   2048 48:a5:6a:7a:bf:c3:8a:60:be:f8:0d:4f:44:bd:2f:e4 (RSA)
|   256 e5:6c:a7:94:25:09:75:2d:d0:55:78:b8:d6:c3:26:f2 (ECDSA)
|_  256 36:a2:cc:18:ff:01:62:e0:be:df:dc:35:3a:b9:e9:ee (ED25519)
111/tcp   open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      33241/tcp6  mountd
|   100005  1,2,3      39067/tcp   mountd
|   100005  1,2,3      41101/udp   mountd
|_  100005  1,2,3      57820/udp6  mountd
139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: FINE_ARTS)
445/tcp   open  netbios-ssn Samba smbd 4.8.12 (workgroup: FINE_ARTS)
2049/tcp  open  nfs         3-4 (RPC #100003)
35135/tcp open  nlockmgr    1-4 (RPC #100021)
39067/tcp open  mountd      1-3 (RPC #100005)
40543/tcp open  mountd      1-3 (RPC #100005)
53257/tcp open  status      1 (RPC #100024)
54581/tcp open  mountd      1-3 (RPC #100005)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Host: CARLAM

Host script results:
| smb2-time: 
|   date: 2025-05-10T03:12:31
|_  start_date: N/A
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_clock-skew: mean: -40m00s, deviation: 1h09m16s, median: 0s
|_nbstat: NetBIOS name: CARLAM, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.8.12)
|   Computer name: carlam
|   NetBIOS computer name: CARLAM\x00
|   Domain name: my.domain
|   FQDN: carlam.my.domain
|_  System time: 2025-05-10T05:12:31+02:00

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 43.23 seconds
```

## 获取webshell
![picture 0](../assets/images/c4b5ff42b384b340b7b5d815c20f5369f58aca914bdda1d320b243e8bef2fac3.png)  
![picture 1](../assets/images/2f5bff0e5e0ac7c706d052ad9307ff986b214b89c16c583cb52c32772fb238f9.png)  

>三个用户
>

```
S-1-22-1-1000 Unix User\carlampio (Local User)
S-1-22-1-1001 Unix User\xiroi (Local User)
S-1-22-1-1002 Unix User\aitana (Local User)
```

![picture 2](../assets/images/4e9cac6aa7b86c0302aa37484879009b5b9e741dd95c0c7bd7ce7a17fab9877b.png)  

>那就不是这爆破了，看看挂载怎么玩
>

![picture 3](../assets/images/d770720162128df2355cdd396193864a11a972b3a8b9126a9877d20e1960e99e.png)  
![picture 4](../assets/images/24b3acc113c4f66c5c51338d8e6580040a406b506e004d4959f6d1985d8b6ac2.png)  
![picture 5](../assets/images/2edc0659703cad50d503cfcefc033c39f18d3e8be40e921ba4a3e60d27c20692.png)  
![picture 6](../assets/images/5d1a8356d88b817d9a3a4b3df66a9367d8317e1eeac5a7b95aa6cc1f75626881.png)  
![picture 7](../assets/images/33c5e821d8fd035c6de4e65f9eaed9fb6b4353753788f5d6ce1ac66b96c4f4c9.png)  
![picture 8](../assets/images/f3e9de9f51e45fc6d7341f8672f2e51bc371de4acdfb18cc6a9b085e70b5c0c0.png)  

>又是个猜谜，acre可能是用户名
>

![picture 9](../assets/images/6ff6b198c375b63399e65d9e6fd437ecfdd7b9b28de80e3e6f500aedce55b6ed.png)  
![picture 10](../assets/images/9f739d8491d83efeb9fcc23938a63fe718f21097a84ac9b9b4b6c538b314dd52.png)  

>还真没啥好想法，爆破用户了
>

![picture 11](../assets/images/a5b1907c90e327534fe491c5ff3b5466f36ce201a7b9c1976edd61669603ff92.png)  

>这个还有一个提示
>

![picture 12](../assets/images/1a3d4d77caecf1cf7977cd01dbb0200fb20b28b63b1043e22a9095f74d63e626.png)  
![picture 13](../assets/images/f614ce29bf9e3ff7164c82edf6931e3d6c27ba2bb6f4c2e51500444e78acea48.png)  

>所有用户都做一下
>

![picture 14](../assets/images/9d89064e1d4e68c7706dd96241d84bab681a48424e2da76ab9bc8862fd43ea1a.png)  

>跑完没出
>

![picture 15](../assets/images/34a3846e4c61a057d7c844ef192384d1d0c3376693090561e937d990e9a51f3a.png)  

>查一下什么不是日期的问题
>

![picture 16](../assets/images/5823a68819b9089d7446455fcaf46f6c472d0b3d45025d800ed9544f8185ac38.png)  

>还是没出
>

![picture 17](../assets/images/ea08b825e6d5353d8bb63683200756cd9ba206c26652f15e58b6678f3d84344e.png)  

>把cupp所以情况的试一下找到密码
>

## 提权
![picture 18](../assets/images/8c453a3518af07bfb68ee2a3d4517ed1ed466ed21fa3f658c446928d076d53c5.png)  

>还有奇怪东西
>

![picture 19](../assets/images/80e15c26b8a33ca337cc39ac5a9ac68012d96fd98f878339458fe460b7ceac4f.png)  
![picture 20](../assets/images/abb7c665923ccceed12366b53370b408cf7400a06094999e44bebed37d86effd.png)  
![picture 21](../assets/images/322f0ae0815c83a488ef6aef80799bef53172576e00dc1db15d3359e006e5883.png)  
![picture 22](../assets/images/74b9a270565437dfe0abbae5df4d08f18ae4ce5056ddb597ef63bd65f391a3cd.png)  
![picture 23](../assets/images/eb26f79928ee27348d0d5d080e2eecfa0beffd2eff72bbaa2896cd3ca5a596e2.png)  

>这是什么应该怎么利用
>

![picture 24](../assets/images/bf913263a0eff8f1c780ca41edf641e4cd72bc1f24aab86dee65cbae9867b88a.png)  

![picture 25](../assets/images/6c53b429aca5c04a3b877099bbcf2fcbf8df7fabde37f8830f64b416dcf5aef3.png)  

>除了知道不是定时任务找半天没看见有用方案,看wp去了
>

![picture 26](../assets/images/874fb302488e71e780c01973e2505531f54f10ace15f635c01968238f50239f2.png)  
![picture 27](../assets/images/ae9d749d6735049c34ab16e8803621b740f4fa0919cf6175cf8aa9004d75f623.png)  
![picture 28](../assets/images/a467390aa6d911e23a3060aad1d37842d5071d30bc0f6ca43d88818e2402bcda.png)  
![picture 29](../assets/images/8386e404db217c30e223a9b6670793cd0e244ad6b1fdfa885592bc3c05f43606.png)  
![picture 30](../assets/images/a4cc4378f7d249634e809272ef0f98199938aa055e5029ac11c32abcda4e72dc.png)  

>在conf存在base64
>

![picture 31](../assets/images/5e8aac90290f3f215626b73a11a9ba6b144ef56221f42d1738454cfe56a1b736.png)  
![picture 32](../assets/images/1e89551a2d23c7d3c189b1eb6eda54f2e4d8ff4580e6adc486a0c4821f7a744e.png)  
![picture 33](../assets/images/862d047dcffd83e791028aa9e06128fafd4f950b95e45e28bd23b0e83a6b3225.png)  
![picture 34](../assets/images/def1ade59845c88ea791e9e33fc97a2938efd76a82fae9a1c0d08bb4102c9b8f.png)  
![picture 35](../assets/images/69b9d991bfa41ea32c6138a211de7eb281f2a2f32b44d42dad36d3c96a94da63.png)  

>没成功为啥呢，应该是怎么干
>

![picture 36](../assets/images/d0abe3a8be3869eb0f7224cf20070f61a0f25f601c0ed11503631b7409a7b8a6.png)  
![picture 37](../assets/images/4e53875e336570a938bedd29a22ba7bb9c87a1eb56aca1c91bb20776b19f1d9b.png)  
![picture 38](../assets/images/0cd9e2495dd326087000d7047f7162fc8872ab80fb45796e69869c74f4a2ed54.png)  

>还好能执行命令
>

![picture 39](../assets/images/0de50fe3d843b735cc7e212e91c21677a7006bcd48b570114b32c13a9398c095.png)  

>结束，感觉难的地方可能是socat部分和leet字典部分
>


>userflag:23bdb9bfae27f13a9e216fa72fcdf9c5
>
>rootflag:9755cbb374f1a6b47d52160a452b7084
>