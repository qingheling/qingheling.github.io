---
title: hackmyvm Stagiaire靶机复盘
author: LingMj
data: 2025-02-04
categories: [hackmyvm]
tags: [smtp]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo# arp-scan -l     
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.185	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.057 seconds (124.45 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- -sC -sV 192.168.137.185
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-31 04:33 EDT
Nmap scan report for stagiaire.hmv.mshome.net (192.168.137.185)
Host is up (0.011s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   3072 9e:f1:ed:84:cc:41:8c:7e:c6:92:a9:b4:29:57:bf:d1 (RSA)
|   256 9f:f3:93:db:72:ff:cd:4d:5f:09:3e:dc:13:36:49:23 (ECDSA)
|_  256 e7:a3:72:dd:d5:af:e2:b5:77:50:ab:3d:27:12:0f:ea (ED25519)
25/tcp open  smtp    Postfix smtpd
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=debian
| Subject Alternative Name: DNS:debian
| Not valid before: 2021-10-23T15:24:56
|_Not valid after:  2031-10-21T15:24:56
|_smtp-commands: debian.numericable.fr, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
80/tcp open  http    Apache httpd 2.4.51
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Protected area
|_http-server-header: Apache/2.4.51 (Debian)
|_http-title: 401 Unauthorized
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Hosts:  debian.numericable.fr, 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.04 seconds
```

## 获取webshell

![picture 0](../assets/images/8874d430821dd819e8748bb70a23afcf5b434c289a86769c8222295347f92048.png)  

>先认证再扫描吧，看看弱口令
>

![picture 1](../assets/images/1a625c094b0a1898b18e6f69f9c18f6acf30ad8edc56227d7213ac22a749f70a.png)  
![picture 2](../assets/images/abf0843963171dae66c09252b1284fc06f473546c4a9da5903408a829f002f6e.png)  
![picture 3](../assets/images/a098d168b713f3b818cdb44e11dc579db6700f6cda2bf9d5044c5afaf9a3a1dc.png)  

>这样啊那就是没有admin
>

![picture 4](../assets/images/b4c5d3b20dea223c4e8635e11ed24b6544dde3faa41f98f5c6d5ea597e1974e6.png)  

>root也不行
>

![picture 5](../assets/images/3f0d3c9de04129f4c53448a07a081187a5c349e9ec30c28b3dca66ac3a333b23.png)  

>有点摸不着头绪像上一个发送邮件得到php？，能知道有啥邮件获取么
>

![picture 6](../assets/images/be5e44a410ed8aa7f75c2e3ff466520367bc849bbcf29074a8890068c4bf586b.png)  

>不知道怎么操作
>

![picture 7](../assets/images/3aa3ab5268c7a21844a679cd532afe1ad75d0ea3adc026297890713489e40209.png)  
![picture 8](../assets/images/7ecad8630b684beed399d03f3b68c3163df2a668cbf185e6d655c9f37a9c23c9.png)  

>域名突破,没成功
>

![picture 10](../assets/images/06c7367a7754dab37d31e0356404ce7359e4327acb3227139978829ab1865518.png)  
![picture 11](../assets/images/3b46df3c419488a01d08f90bb3eb1e7430a5c8796ae74bafe18e3eb125716957.png)  

>有什么方式找东西呢
>

![picture 12](../assets/images/9bf5591180cce6792fb249834814c30cfafc2923c7e75465af3c8c3447e85177.png)  
![picture 13](../assets/images/503834a0f06087825d1c842b3a6912a3842fc01fc026e7af0bac8134f10d75c9.png)  

>这个是什么原理？
>

![picture 14](../assets/images/6b570c04e4de2e7ed6084797665e1ea97bf029f969d0f81f24816921254aaf70.png)  
![picture 15](../assets/images/1b92031e2586ae112bc92aa7fe07946c9048c96e43bd8d56559ea8e71ef90a5d.png)  
![picture 16](../assets/images/390d615ec194c34ed900203bca161514f343d6772c23e12b1c7f25858e57a814.png)  

>我说呢原来没有拿到图片
>

![picture 17](../assets/images/ed4bb5d3abef391b8ac6d51320a070f2fd13d028a103f5bd86830bcdbf20e515.png)  

>POST才行
>

![picture 19](../assets/images/52ab1ed91c997d749d42465ea68e38f7e760ae67cc145f9db687f18e12e8b4d3.png)  
![picture 18](../assets/images/afcb8bc9e0355f4da0b0faee239e97f68d0857fd67094ba916f5eb74e39f4a9b.png)  

>啥也没有啊更是一头雾水，尝试做密码但是没有东西
>

![picture 20](../assets/images/26a0ded6eb172f2aa3dbdef718b144fcb705d315a776b76640c1752082df7169.png)  
![picture 21](../assets/images/3bc8e6f736144ca720b23e16614f19c37a84bf0f62f5d6ec01bd5d26cba81686.png)  
![picture 22](../assets/images/b0fe52f48ed48b9539dd9a4c3b10fd12c89d83f453e1736e4daa0a041fd8f70f.png)  

>那就不存在爆破目录
>

![picture 23](../assets/images/43e92bdaa8a6e4a3a0f9d0d7fc0b8057ecf8be114d31fa6abf037805fd23d9c8.png)  

>没用户么？
>

![picture 24](../assets/images/e9ff35d41bea3d01fe17f43a8164bd39e5360c149726d30d5bf93e39403ec1b8.png)  
![picture 25](../assets/images/8a26212daddf5610afbdc6d5232dcdac7669f91169eb15ebde27ff3fec9bfbca.png)  
![picture 26](../assets/images/999158944af00b7762fb5fda659b32794b611a25448c373be1c3bbedb328c0df.png)  

>无那就算了没有啥漏洞的提供
>

![picture 27](../assets/images/8a3d5ce861294505d6335c33779111d6e960c762de85605a98a9d7eaba1d8a79.png)  
![picture 28](../assets/images/eba6b474fb0f418106959c1403819896351ddea09be9f9776893bae7fe1c22f8.png)  

>这咋整呢
>

![picture 29](../assets/images/6228a1af3234dcb53956cfa5100166f8f82d12a8bd5c869d974a781ebdc28f0c.png)  
![picture 30](../assets/images/f3f06a0c67a5237fa84ca6e6266e90b99e6aef82625698fa2b141f5d6728e768.png)  

>这也没东西啊，没发送成功
>

![picture 31](../assets/images/4230740a55b979cf93863468a61fcfae4dad70d026ad53655464ba7699aa5f96.png)  

>好像有定时任务
>

![picture 32](../assets/images/e6dce08f2f677c3d8bf39a42ac930293c5d950bba30d7e37bd4a9ed4601b6085.png)  
![picture 33](../assets/images/c0a3e2da03c48a095ad6b19e46851022f1bc177bc8fcbdece9d2a70f4a8150a5.png)  

>不知道咋触发
>

![picture 34](../assets/images/15b8d80001c5d2249c8805c2d505d332f76e6cc7c71762c9dd525df705b9d157.png)  

>但是弹了
>

## 提权

![picture 35](../assets/images/fa8d13a91a1fb99dc5185f0193854651fe84c879c95fe16742571b2c8072ef57.png)  

>原理在这里
>

![picture 36](../assets/images/5a71cab148c8842a3283eeaf9530b7d49c76f70291152a85bc781128c4dd3501.png)  

>要回到www-data的感觉
>

![picture 37](../assets/images/dd908ac5fee608a5095f5d0e76ea17122e205f9b947b3d66d5895996c4bf0aaa.png)  

>不给看
>

![picture 38](../assets/images/ffc20ab602441a0a7e82929b17a3dfae8271e00ed6696c475d9802bab2af7a40.png)  

>这个是里面大概率放什么有趣的东西
>

![picture 39](../assets/images/922899a63d89cd43a0d79cec750fbf829ff086be23a6bbc5ea39b044061c5a7e.png)  
![picture 40](../assets/images/b044f2ceee9f3596a213a6cf449dc5b68738dab1df920143207e90ee068bbaea.png)  
![picture 41](../assets/images/936846c386d017c6615fd3d21b06082d772b093b66a92a80a98e8d8791174962.png)  

>还得是找密码
>

![picture 42](../assets/images/291ef5478fb6787a8c386ffac2ffe611914b7a1819be0edfea31c786d3facff9.png)  
![picture 43](../assets/images/1c6e50ca9a0e0a73c52bfdb9e4c465d746f1120cdb38d6ea78428775720ef409.png)  
![picture 44](../assets/images/d3b3b218b72076db77b55b485da4a3d968e4f6e03270e1a18df2ef93f73336c1.png)  

>我觉得爆破一下好一点
>

![picture 45](../assets/images/5d4276c2c13d1e26b81428f5f930c2aac2ee44e5c9aa961844167538702bcb51.png)  
![picture 46](../assets/images/838de8230c3ea47d78d782d0677756b90e86e6a781b786a8eae73fdeb56a4b98.png)  

>如果是这样的话真就只能等爆破了
>

![picture 47](../assets/images/0cd3b7d3624d338e44bd9f985002e14ba16f273b091e58135aa4618e68434398.png)  

>没有复用可能
>

![picture 48](../assets/images/2fe964bb0568d50425fe1250fba423997e09d422a8725309b281d647feeb40e6.png)  
![picture 49](../assets/images/8e87b7f0ee59a50567498373b97de8342ae8c6161d09fe5cfa681e47002845aa.png)  
![picture 50](../assets/images/fec5438feef9844aafd85568e14c463b2156ec03e5de8ddddc4199f91fb8b7bc.png)  
![picture 51](../assets/images/2764083c543b05a5dc78cfce2368340fe0e52e80da1f18225bd09beae888397b.png)  

>？？？？没有啊那www有多难操作
>

![picture 52](../assets/images/0bf190f53bb838753ea9eb03eccb989590e3c59750596d14e20c7327f09f4573.png)  
![picture 53](../assets/images/6d9c0772cd3b6229fcf308e852d21964d4ca976a00a35fe4f228741ea974610c.png)  

>3306但是没有账号密码，还是得拿www-data但是它又要绕过auth
>

![picture 54](../assets/images/4f9ef964efb7628640f57ba6a0fbd8b73c268ca0e20d1ded7f89b2f810ca0034.png)  

>没有
>

![picture 55](../assets/images/52968bdcd53d9d2cf37c81cef8d5a084bdc3e6f550d1a5face32a7b1c0e97652.png)  

>忘了这个了
>

![picture 56](../assets/images/d10c0109529cdc75825e94e2fcdfb888e4f27a8de73622bd64bb9b14ac086868.png)  
![picture 57](../assets/images/72c45cfe1c20569b499e008497be09f3196c35e5b93c49650aaa4ec6be2c3648.png)  
![picture 58](../assets/images/5c942f8407b48ef3eec06a19a0b9c974f5b46393447986bf22c8f7f1459ea93f.png)  

>还有什么操作呢，好像那个termail能做
>

![picture 59](../assets/images/9fb20f5e42dcd590c5bf3f47413aea3044257adf669892e94c9be12c56dac35b.png)  

>那么问题来了读不了
>

![picture 60](../assets/images/e6c2074fdd45723a7cbc07be977660940aea0152781883fca08165c9b81c1fbf.png)  

>现在问题是如果我整个其他的东西它并不会转用户但是能修改权限如果劫持一下chmod呢
>

![picture 61](../assets/images/0dfc930a174ff932a73c670abf6180cc8c7d1bb724c18005c999e5b55fbd1f5d.png)  

>竟然能移动我很意外啊
>

![picture 62](../assets/images/dee4a7fa870869cf324ec9626f705c0b82f8e45928c0c06be436b7ce65b3cd9b.png)  

>这样就可以读了
>

![picture 63](../assets/images/6a509f68c855c787ed7521b4176aa8fcd4fd9c553dc311e405491d15001475c4.png)  

>可以进入vim模式，那就直接操作
>

![picture 64](../assets/images/05bfe8663c80e398ff657d8adb696579c1abb549461b6d937f668501300e7504.png)  

>刚刚忘sudo了
>

![picture 65](../assets/images/d3f7c70512c4476ee8894d40356c86ff4fb861c761f4aa74e152a5a88a95348e.png)  
![picture 66](../assets/images/9fe3bd772e6a3f7adb21dcb21040729bc1d4198c75dd481bcb1012fa5b174436.png)  
![picture 67](../assets/images/3785ed07d3702772ac44fb4307fedc95c8a16d4af0e394b3e80df79799ad9587.png)  
![picture 68](../assets/images/919f24f7602feb40f99147af39a024a3eafef2a65194364918433a56a2983a6e.png)  

>哈？，我转发出来没目录么
>

![picture 69](../assets/images/ceaee74d9d077aaf93fcc419d287e36c9ed3f53201c755a43fde75731b81f674.png)  

>如果是这样的话那就不要扫描了
>

![picture 70](../assets/images/649071d9d021123f336cbaadbe828b61fed0d894bb598039b66ac347ae41e38c.png)  

>那没事了，哈哈哈
>

![picture 71](../assets/images/3c5a626e0574c2b172dbda19cb8730f326b646006e44c4e2226bbeb41a5fbc53.png)  
![picture 72](../assets/images/be76a6259bfefeb0d82521076599e02ab231c073aa968a842ce3ffe9ee930c81.png)  
![picture 73](../assets/images/6ab48dde16239392a4bc864b757d4cadd32fca1f818dd5086cce9b7d8094a4a2.png)  
![picture 74](../assets/images/d4dba2a8bbef25c3014c349fd1debb4d5f7005fc1be0bd7d97d81e1ac040a0b7.png)  

>结束了
>

![picture 75](../assets/images/cfbb011562f2e010032741e3a6028e976ebe4829c6ecb932df144a02fc468cd1.png)  

>怎么感觉内容再前面不在后面（https://mega.nz/file/drkWySqL#349NDiHdOuYFBkuweE614s5WUKIpgcKGKAzWb6rR_gU）不要点开！！！
>

>userflag:2d82acbaf36bbd1b89b9e3794ba90a91
>
>rootflag:9ed378ce95f7ea505366c55aeaf12bea
>