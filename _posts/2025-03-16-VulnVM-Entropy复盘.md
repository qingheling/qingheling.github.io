---
title: VulnVM Entropy靶机复盘
author: LingMj
data: 2025-03-16
categories: [VulnVM]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.28	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.73	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.93	a0:78:17:62:e5:0a	Apple, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.122 seconds (120.64 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.28 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-15 20:56 EDT
Nmap scan report for 192.168.137.28
Host is up (0.018s latency).
Not shown: 65532 closed tcp ports (reset)
PORT      STATE    SERVICE    VERSION
49152/tcp open     tcpwrapped
62078/tcp open     tcpwrapped
63641/tcp filtered unknown
MAC Address: 62:2F:E8:E4:77:5D (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.93 seconds

root@LingMj:~# nmap -p- -sV -sC 192.168.137.73                          
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-15 21:13 EDT
Nmap scan report for debian.mshome.net (192.168.137.73)
Host is up (0.034s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 cc:05:ab:8c:ea:28:eb:b1:9d:da:8c:ce:65:ee:63:43 (ECDSA)
|_  256 3f:9f:0a:7d:61:f8:6f:4b:46:01:c4:db:74:b2:b6:a7 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Apache2 Debian Default Page: It works
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 24.20 seconds
```

## 获取webshell
![picture 0](../assets/images/698b13e25855a66ca473743f9bfb977a8a7c3dd7fc824e766ccca13743ae279f.png)  

>没有端口可以访问，好像被拦截或者它得127.0.0.1
>

![picture 1](../assets/images/c35b194b461ea2d50b2aae73638eb7550fdc47f0ba24059bf7313ee0737701ed.png)  

>双网卡扫描出错
>
![picture 2](../assets/images/65f0385df34f77986acd7b52fef2eb025aa48d2f64654dc9943c42323533472e.png)  

>没隐藏信息，直接扫目录
>

![picture 3](../assets/images/f70d1750972fe0634bb76b598a951c66d0ef3f152baa917b1985c81a5234943a.png)  
![picture 4](../assets/images/9eb6d7ee89be545b222b48e8bad11acea33bfa6682b57f4a910a906b630c65fe.png)  


![picture 5](../assets/images/85e45f932e29754768960809ee59e1cb68245d962856f1a7dd7c2fde3e209198.png)  
![picture 6](../assets/images/b45e4c59518888a7b505e4e3f6e442d4b76c96f595154cbc4cd792807b34110b.png)  

>这玩意扫目录,压根没扫到有点超出预期了
>

![picture 7](../assets/images/0a95b02d84c92e91efaca3bdab4725c9690791ab32a7d5f0cfa306f8be56a21b.png)  
![picture 8](../assets/images/d543150de9b533adba5080a5482789e7536e17ff9ab9f98fac44e08a88b086ae.png)  
![picture 9](../assets/images/b7cea5e434bfbdac4e828e4c7a31d4c0b673c8b139c99d3f2c536b3e564b6891.png)  
![picture 10](../assets/images/c9cc39d0c72ace24a826e6fa0bf5266b2f29a0ebf8cbb609ee39c8fd88287bd3.png)  
![picture 11](../assets/images/ef877f5f05741de74874cbe38f00937527a002d9d58ef78dd106dce182c20a49.png)  
![picture 12](../assets/images/81ba0431a2600ab6082c5e0d433640e78c79161b29e43ba75ab01e00c23389b3.png)  

>那个cve有点问题是要吧代码改一下是python3代码
>

![picture 13](../assets/images/9207356d097ad655f92b2c80227c81fa9b9df833789c1133b45d44cee523d810.png)  
![picture 14](../assets/images/82a6a856a635b6a4348ce0c50d319349a1defbf267932362241ce8783c2a85ee.png)  
![picture 15](../assets/images/ceb6e7c4b5e8092aecd50ab5ec6a5664a72ac78b949377a088964ad85c5edc38.png)  

>错的是什么鬼,把salt用上
>

![picture 16](../assets/images/b36ab71f19b32fa76baac2b65219680ace6673d967ceaeff0d0c089b36d80303.png)  

>不过如果是爆破密码的话其实也可以在外面burp爆破一个道理
>

![picture 17](../assets/images/8d0135e43c81b0afc40fe1acb468231c3cf1fc60138e7170bdf034ef8b1b3740.png)  

>找了个油管的博主做这个，当然方法不是唯一可以去ll104567的视频看如果做，只不过我想完整体验靶机没有去看找一下这个爆破方案地址：https://www.youtube.com/watch?v=VwtHv4B3f6s
>

![picture 18](../assets/images/8805c3f006f97a4a6a3f378b9349c394cc60152a8509349a61813ede9e699ad2.png)

>不行太卡了，跑得我电脑都动不了
>

![picture 19](../assets/images/0d324af83e3e6b413cb1c9502de2f0c2fff812d6b21129e94d6501543c74cab6.png)  

>去找hackmyvm看看存在的方案
>

![picture 20](../assets/images/dbbf66bb87d72a81d10814ba2a91f422882912358340ae2782c835b7296d31ea.png)  

>找到的方案地址：http://162.14.82.114/index.php/507/04/05/2024/ ，不过上面说到这玩意看命
>

![picture 21](../assets/images/b654494ee759882db4707f58fc68104c7b66a784980e0cad772fe3deb5c86fdc.png)  

>这玩意没跑出来
>

![picture 22](../assets/images/f6b7a513d61d30b72a811ae6a2a94d864358b426d41701064c6ace833f8c4a10.png)  

>是我运气不够好么跑几次都不出
>

![picture 23](../assets/images/922dbaced7a40c11ef9cc63250be0907131528feaecc5d070cf61d2f5c0431a0.png)  

>把能想的方案都试一下不行就看wp拿密码了，好了看了wp发现密码不在rockyou里面
>

![picture 24](../assets/images/6aa7dd65c28118ba49a810c700f8c3d518385e337862fd228f726f95fec09eb8.png)  
![picture 25](../assets/images/fe349168652bb32af80d52b084492a25dd2ce44e6451a272880acb4dfafa6d79.png)  

>不能直接上传文件，看看什么形式先
>

![picture 26](../assets/images/ce2ba0626d65d5546c48aa27183bb975278da768a69cac6c2a90ff6ccaa60e0d.png)  

>没看出是成功还是失败了
>

![picture 27](../assets/images/7379ed494d43060ef31c86b7862695ac3743700e68ad1874384d7aa033dbc9f8.png)  

>直接继续工具进行
>

![picture 28](../assets/images/cfa1de4ca24a9f4c486f840438deff24420ff8c089af4b1446cd5c55b0abc1d2.png)  
![picture 29](../assets/images/fe4ceb4ee0fb77db99efbe8a049be870812efca4cf618daec4720958d7bfc2a7.png)  



## 提权

![picture 30](../assets/images/b21f3b4f787f8c75d1468547bbb02f118153d9394ba2d3ee32e4df6077e22ec8.png)  
![picture 31](../assets/images/06e88f55dd8a73d48e67ab5b04b78aadd251982a750f88bd1ff1cec2966cd5bb.png)  
![picture 32](../assets/images/1b443fb4cecfbc181be13abcfa2619f1d6f47f7742cab98f99de9e46ead0cc0f.png)  
![picture 33](../assets/images/3d2fb7390abfb0e03138e12b08605eedabf0bf9f5b61fd02b255f6819302b42c.png)  
![picture 34](../assets/images/9de64e5fafad90129b5369856e8bd51d7d473cae8618660e30a653dd24fec461.png)  
![picture 35](../assets/images/014d8c91d5e169272c918bb503ec15e0062aafd86306f5bca401502fa375b3e2.png)  


>数据库没啥剩的，密码也不是这个
>

![picture 36](../assets/images/5205cff37803542db2e59bd5db7c493d597f8c0612244b9eaa94bb7e04513247.png)  
![picture 37](../assets/images/ea338e4fae18919cfe1ddabc6a28cffb46c33ed074ccfaf3ef5910d274150dd5.png)  
![picture 38](../assets/images/0237b023d4417b5f7331d175ea883ce856021e30ac6c095f8daa234dc9baf5a3.png)  

>有点累了，我直接用工具打吧
>

![picture 39](../assets/images/f71e33f06542e649d6105cf3d4452dddae199fe3265dca6cbefe71dd8aab69ec.png)  
![picture 40](../assets/images/997c6698accd4723a53ce891df0ab2eda2a7940f817f07152f72ac1aef964396.png)  
![picture 41](../assets/images/676d65dae979317dbbaac7f55bbfd44dc51e796541b75ccf70d9f26639aa05e4.png)  
![picture 42](../assets/images/d5d294da5e7d24866ac7039da2ec39c16ea27cb1298cf9a6363c3e5d7c9ab369.png)  

>密码是啥玩意呢，试一下弱口令
>

![picture 43](../assets/images/cbf46cbd9d08df962a6b834ac9cce84e3a5ade447a4db23e9e80168adaa70553.png)  
![picture 44](../assets/images/31e291659eae86a3fc930089ae0d17678c27ea3ae718515df406a85271c45522.png)  
![picture 45](../assets/images/edd5953e16152278130dc13777130224add45e4ed75b8a75f79a0e4f95b78cc8.png)  

>出来了
>

![picture 46](../assets/images/08afb6624f6757fc30f9f18dd6cdcbbf4dee7d8bd6a900c817cade273093c583.png)  
![picture 47](../assets/images/63c44b2970c49bcee3909d395e0e1877eb335a6cce7d92b0b52a73c44e25a959.png)  
![picture 48](../assets/images/acdc193440a6ff07341c28058bc44a900dd1d65a3e02c64c069b71ab99b2bac2.png)  
![picture 49](../assets/images/cdc4bad312842c6cbcaf8af132a91da44996ea32a5a3eebb6bc18635791f6027.png)  
![picture 50](../assets/images/39570e885404fd97b9f6dd209737eaa4641038fbd49dfe213e1961dfc50bf153.png)  

![picture 51](../assets/images/0784cbba8615975ffb86c333ea35038a3887e3f0bce4a030f350fb9dc492dd6a.png)  

![picture 52](../assets/images/3b387cd417206f15660882145ac1d9467927bffad4d7c477c3bbd9a709a21160.png)  

>出生年份啊，直接for制作字典爆破了
>

![picture 53](../assets/images/212e518ccd25d40ea96b24f2d628573aa7f55f31c929366df6c2ba1aee42bfde.png)  
![picture 54](../assets/images/92b7423fff067bd1d58097c76300638f49d8ac66a719c235b29c0891b4b77d84.png)  

>等着了，主要我不知道出生年份
>
![picture 55](../assets/images/563e1420dd7621fe63c44a7844b1bca5430f2af9033fb4e3aefafc1f10c323d3.png)  
![picture 56](../assets/images/218dc4c3a3a9570ca8fffe26fa5ffeaa88510f40177c37eaac5f120ecf0831e3.png)  
![picture 57](../assets/images/8202020eca3561ea70d3be87fcb852160c8cf62d7c21ed440435e5f0e99a9609.png)  

>结束，太困了打得很疲惫还好打完了
>

>userflag:4080c01a18779f38d244109af1cdc20f
>
>rootflag:387fe09da641693684be5d92603eb35b
>