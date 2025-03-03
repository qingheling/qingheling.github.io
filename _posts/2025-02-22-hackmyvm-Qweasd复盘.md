---
title: hackmyvm Qweasd靶机复盘
author: LingMj
data: 2025-02-22
categories: [hackmyvm]
tags: [jenkins,pwn,Capabilities]
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
192.168.56.100  08:00:27:d9:dc:78       (Unknown)
192.168.56.160  08:00:27:23:4e:2d       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.864 seconds (137.34 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.160
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-22 01:54 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.160
Host is up (0.00062s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 fa:b1:dc:5b:9e:54:8c:bd:24:4c:43:0c:25:fd:4d:d8 (ECDSA)
|_  256 29:71:69:ca:bc:74:48:26:45:34:77:69:29:a5:d2:fc (ED25519)
8080/tcp open  http    Jetty 10.0.18
|_http-title: Dashboard [Jenkins]
| http-robots.txt: 1 disallowed entry 
|_/
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Jetty(10.0.18)
MAC Address: 08:00:27:23:4E:2D (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.95 seconds
```

## 获取webshell
![图 0](../assets/images/836463bc829960030cef4a5613246b272cfc637794a7e7fcf3171a936b0ee4a2.png)  
![图 1](../assets/images/ef33554da81c878813edcd228b68f35e6f790251b075179e23fb335ebe52c7d7.png)  
![图 2](../assets/images/30cb8d93d48cea50d6b20666e9b35da4b68f481a15001176fe33bbd8a83d51cc.png)  
![图 3](../assets/images/f68d168b8e73ca6c6975d5ffc0d01073ecf590f14374915ad335e983b274aec7.png)  

>先爆破密码进行一下测试
>

![图 4](../assets/images/5726473667ee61f280ec2a9e54c208c1208845bd66ee127b9cb679316c10b806.png)  

>这个靶机很卡不知道为啥，打得非常费劲
>

![图 5](../assets/images/43e24c80c7550d2d3cd710470c061a63ddd2c1fe0e9fbb4c896e495836ec9958.png)  

>看看cve吧，受不了直接爆破是不行的
>
![图 6](../assets/images/c72e23d183fc74cc11d79b307d343e305d45563d825fc11f72a23ac88334fa41.png)  

>第一个就是
>
![图 7](../assets/images/15a10f12d2296d305db7fbfb14df3ae6987c6a453050dfb356b03e3fd9a4f005.png)  
![图 8](../assets/images/032625821cd0b787bb84a4770bff141feedb5e100b5cc556cfa88f79883a842d.png)  
![图 9](../assets/images/0902285ca2040a1ac0952693291620410366d0736334a9d075869aa77b33bbff.png)  
![图 10](../assets/images/c0c90a8006076c563c3f3e8c20fa7e8b29dc97ad9d7df7bccb0d652a1f668c0d.png)  
![图 11](../assets/images/51e35da684d430a363a0013a00a04fe6f650922341db1b85e6a276660236e7fd.png)  
![图 12](../assets/images/080f65ffeac2e37805a7c8b8139364721defa041dd99b92b7283c2ace78180dd.png)  
![图 13](../assets/images/de3513826cc8af374583f1da3c07eaaae1a2ca77d3340a4132edd04a588bb922.png)  
![图 14](../assets/images/f2d0612ab09bf4d5a40c65be831046c5f1c19b3eaf5ca344f0a0651863557a5e.png)  
![图 15](../assets/images/0ea3352bf8829ae7f7da994a1c5e0082fd90901c1d8d8952d3b7106275cef9f2.png)  

>现在需要找到对应的东西
>

![图 16](../assets/images/b71ce0cffc01f5e45ecf65a98653c0485010cea0a6209b6b6624452665998afc.png)  

![图 17](../assets/images/8ec0aea4f41010759aca17915216ce9fb5df27e2a2d3abff0bf214fd69b6f438.png)  
![图 18](../assets/images/72237c29913460001734e1634c54e436fb59f44db6ad218ca9c9a7050b6fc6b1.png)  

>这是bug么怎么直接通过了
>

![图 19](../assets/images/1b7bdb47eb16d6c9bf90cfe0b053a756290bfe3ff08b3d455381aff9610f542d.png)  

>docker是干啥的我开始有点懵了
>

>上面是bug了直接成功了，这里我直接退出来走一下常规的路线，原因我们有LFI需要去找到对应Jenkins配置文件
>

![图 20](../assets/images/c807e7d1273772be5c657bec431c6ccaf343741f728333c826eacd187beb7b1f.png)  
![图 21](../assets/images/b8da1a15675b2b163ea0785466e0c593692aa5511c21124c0aa029097d87fc76.png)  

>问题是我不知道路径在那我现在进行,kali用户找一下这个配置文件路径
>

![图 22](../assets/images/4f5dcfa9bd63ee8bc96a38cce11fd335c3ccea6466b47ba490db560488a0ed30.png)  
![图 23](../assets/images/6d5a0773aec4bcb88000d3461b0107ea65b963912714bfd5e27066cb485adc1f.png)  
![图 24](../assets/images/da8375f330b56e0a9c82a36f949a23295b9468a39ca7cd3b38659738d171e95e.png)  

>hashpasswd:#jbcrypt:$2a$10$3Ms0ektq3Nt8FBV8WeISb.Y.Xh81/VsOhZAhn5xhXzTZEFlsmGm76
>
![图 25](../assets/images/0aa84b4353e19d59ae3165f9ed0331c7f728dad63f304094fae6e57a8e0798f1.png)  

>爆破一下这个passwd
>
![图 26](../assets/images/ca1ff54125b944a9582fddbcd234ce1b5c2309b0c3d220729e3bbe3582d6492e.png)  
![图 27](../assets/images/fa4253b36de1b994b5769ec6f6e65e3f130b01de208993ea9732776d6c79b554.png)  

>密码不对，应该不是这个密码
>

![图 28](../assets/images/2d7771299674008013d4ececf44f0bd3d98d657aca6d69d4808d0ec073cdc784.png)  
![图 29](../assets/images/4ab5771c608697ce407db41acf9fea4940410d67342d338e99607132b2eedbde.png)  

>试有一伙了是这个东西奥，拿webshell还是我的一个记忆点，简单操作一手
>
![图 30](../assets/images/b810c2f532749e21db2ed6bdbb72d5a04a07b37806fcb1f82d9b92909f792c6a.png)  
![图 31](../assets/images/576948ecc97303c368aecf87396da5eb96aa73d342125ea4260780f7257a480d.png)  
![图 32](../assets/images/7eec992cf115a54aac762ef66badf0eb2f6339d7f7d63c644bc2fd1335d5e6fc.png)  
![图 33](../assets/images/1de998614f471369d7ab9b4e9eb0d3d75e847a18aa6d3e1911f015ed93fb01eb.png)  


## 提权
![图 34](../assets/images/da1a5094047b4090c155b5edb8b6b0af4acd7f44c8857b6db6660e1e05fb8c8c.png)  
![图 35](../assets/images/d8bdd887d9617854f1bded7548436aa747a437945ffb1803d6ae02df74b31e10.png)  

>感觉像pwn能直接干掉，我反编译一下
>

![图 36](../assets/images/870d4b7b0844436b926727a85a3862cdfe0f0a30c369d58c2c293a96df513e19.png)  

>是溢出么？
>

![图 37](../assets/images/23930456166f916b7c86914c2fb62623c9c6e078362e21a4ae2f46854703bda5.png)  

>跑一下工具，不然直接pwn的话又得进行socat转发，这是我唯一想到的方案了
>

![图 38](../assets/images/de7b88ec291aa51b3d5af1a51a41e47d4248234452a56394deea4e11d462ab1b.png)  

>这个是另一个方案奥
>

![图 39](../assets/images/c41a5792127ee275deadd8f489367d552df648621709883f8458f0964aa24998.png)  
![图 40](../assets/images/7ab3ac2fdb012de57120c5efa2111d572ee25b06ed0764c8d9de35a89bd90002.png)  

>还有一个是pwn了，不过呢我打算一伙再做，先挺在这里先
>

>我有一个专门打pwn的环境，所以我就不在kali上操作，操作的原理什么都是一样，所以我直接本地整成功就行
>

![图 41](../assets/images/eb8e3af87b2388d09ba1fb3977db6032dd8ca871da96e6e314aea8d6954d0ea2.png)  
![图 42](../assets/images/7a08684e930f70bfc39114677ccbe28cd657611175e12ea5ccdda807fe35d858.png)  

>本地环境运行状况是这样的
>

![图 43](../assets/images/cf0de79dd9b67833e29cf54bd4a3f9562de6f804dc9e771398bd9fdc8ea356fb.png)  
![图 44](../assets/images/0de85cc6b6d180966e6a5dad212895168007cde75355732016ba6e73a8fa4da0.png)  
![图 45](../assets/images/8bba7444b62c1811f0cc9c552049a0639885779718672e5f9f2d6879bb877fb0.png)  

>这里可以发现程序已经完成炸了第一步
>

![图 46](../assets/images/35cc81235783e2ba06cd27307a4d815c493007343e01939de62698a21cc3a32b.png)  
![图 47](../assets/images/d8736be6133e602b684088ba782442c06ecec9f85ac1b6365f298733168e7fc6.png)  
![图 48](../assets/images/ddbe8d7814eb3a09a0cc523f172ab0f18f4027ed0cb6c6ab04c57f95a63e2b51.png)  
![图 49](../assets/images/b9926d76202ee8663cf5036d069daebdbd6638b58fced0bf531eb218d757fed8.png)  

>偏移量71
>

![图 50](../assets/images/4e0f5fe9581b3b1b9af1a1555477de0c0789e6383bff9206a159eb4ff5347598.png)  
![图 51](../assets/images/3327a687bdeda308e2cc543490e23aba2d7196cc45439bd9ad5cc6a788bac648.png)  

>可以看到我们已经控制住了这个地方接下来就是找system和执行命令的位置就好了，对了声明一个地方是他的函数库是libc.so.6，我从靶机扒拉的保障环境没有任何问题
>

>我利用的是pop rdi 的ret2libc的方案
>

![图 52](../assets/images/7a856a2ca5bc34a49cfa64eed013c2eb88e9392c9041f5a8190d833776f2113b.png)  

![图 53](../assets/images/0d8da30731af9195cd0d1c87dbe7fc2b65f0de93fac64ee3472fc4369aac6609.png)  
![图 54](../assets/images/332fee461997d91fb32bb806da5a0fc9a698bf33b145ec30097a990d6413cb6c.png)  
![图 55](../assets/images/529325c8a2da3f68e547efa0333b62d6809f2ab1bcf04c11f635010f26cc9166.png)  

>ok目前到这里我就已经不会了，先搁置了等那个靶机有类似再研究
>

![图 56](../assets/images/d4999bb8952b679419c273a4c6679161951117925edb9975753d3c0185bdf4a1.png)  
![图 57](../assets/images/4ba1eca4883e183b82876aeb05be26d902e5f6bcddce51d9c9fa8c0b72b02430.png)  



>userflag:flag{Whynotjoinsomehackercommunicationgroups_}
>
>rootflag:flag{Hackercommunicationgroup660930334iswaitingforyoutojoin_}
>
