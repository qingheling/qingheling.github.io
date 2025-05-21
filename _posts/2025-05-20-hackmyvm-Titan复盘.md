---
title: hackmyvm Titan靶机复盘
author: LingMj
data: 2025-05-20
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.82	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.147 seconds (119.24 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.82
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-19 21:25 EDT
Nmap scan report for titan.mshome.net (192.168.137.82)
Host is up (0.0075s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 37:fa:d2:9f:20:25:cf:c5:96:7a:dc:f3:ff:2c:7a:22 (RSA)
|   256 11:ad:fa:95:71:c5:f9:d4:97:da:42:03:2b:0f:55:bb (ECDSA)
|_  256 fa:fb:04:13:93:90:a5:01:53:ba:6c:e9:bf:dc:bf:7e (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: nginx/1.14.2
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.68 seconds
```

## 获取webshell

![picture 0](../assets/images/13a2de925478bcbd18ebdac41dce7284830795136149904f8e83a95a57cf3f20.png)  

>这个图片好熟悉
>

![picture 1](../assets/images/927b144d1474b0228d4c59ab9abf4c74ad43ce30ea131a66c0582761e3d27036.png)  
![picture 2](../assets/images/e9599a73db1ef9460f78a047791911536ded3073e152e8792cff431a8a236ba9.png)  
![picture 3](../assets/images/9d460da36e3a5db892812ba862edfffcf1dd547b493bc29ec46beaaf403bb2bf.png)  
![picture 4](../assets/images/97c6643266691ec4980b946f97c3e6423746111a598654fdd508439c758438c0.png)  

>没收获目前
>

```
 /3/*2'*+*
**************************************************
```

>不知道有没有用放一下
>

![picture 5](../assets/images/434ab223a379827f989236109222b885d7d1ee902a8aa09fab83c9633509733b.png)  
![picture 6](../assets/images/b0d04745871c529e5643b05edb1df3bb77e8e6dad430a6f8d17208fc383c92c2.png)  

>纯文字
>

![picture 7](../assets/images/d5428e01588e6d816f6f5027f7783f6708720354b82b70213c2c2ddddaffd5ed.png)  

>隐藏东西了
>

```
泰坦！献给谁不朽的眼睛
死亡的痛苦，
在他们悲惨的现实中，
他们不像神所藐视的；     	       	      	    	   
你的怜悯得到了什么回报？      	 	  	     	       	      
无声的痛苦，强烈的；       		       	      	   	       
岩石、秃鹫和链子，
骄傲的人所能感受到的痛苦，
他们没有表现出的痛苦，
令人窒息的悲痛感，
它只在孤独中说话，
然后嫉妒天空
应该有一个倾听者，也不会叹气
直到它的声音没有回声。
泰坦！这场争斗是给你的
在痛苦和意志之间，
哪种折磨是他们无法杀死的；
无情的天堂，
以及命运的聋哑暴政，
仇恨的统治原则，
为了它的快乐而创造
它可能消灭的东西，
甚至拒绝你去死：
可怜的礼物永恒
是你的，你承受得很好。
雷霆从你身上夺走的一切
那不过是一种突然袭来的威胁
你的痛苦折磨着他；
你早已预见到的命运，
但不愿安抚他说；
在你的沉默中，有他的句子，
在他的灵魂中，有一种徒劳的忏悔，
邪恶的恐惧掩饰得如此糟糕，
闪电在他手里颤抖。
你的上帝般的罪行是善良，
少用你的训词
人类苦难的总和，
并用自己的心坚固人；
但你虽从高处惊惶，
仍然在你耐心的精力中，
在忍耐和拒绝中
你那无法穿透的灵魂，
天地不能震动，
我们继承了一个深刻的教训：
你是一个象征和符号
向凡人讲述他们的命运和力量；
像你一样，人也有一部分是神圣的，
来自纯净源头的湍急溪流；
人在某些方面可以预见
他自己的葬礼命运；
他的不幸和反抗，
他那可悲的不真实的存在：
他的灵可能反对
它本身——与所有的苦难同等重要，
坚定的意志和深刻的意识，
即使在酷刑中也能看到
它自以为得到了回报，
在敢于挑战的地方取得胜利，
让死亡成为胜利。
```

>cupp -w创建字典爆破用户一下
>

![picture 8](../assets/images/bf75bd20bc1575ada9a808b4af2cfc95bbb4bf7d89d67d59d180822aa57a08c0.png)  

>挺大
>

![picture 9](../assets/images/f48ffd7509b6aca667e4118bd6c582eb84719ece55b4159b4880566fe1e61230.png)  

>这爆破也纯扯，按里面字还行
>

![picture 10](../assets/images/a13abc2f803ffbe5f3a04cd934f22f745a0eff5ace3d85a62f5c198af9e3987a.png)  
![picture 11](../assets/images/cd1298f4c0c5742bb48a88c3fe15407b61a34e71b58c380b703ce30f1ab06968.png)  
![picture 12](../assets/images/829bf2bbc619b6c58c53b60ff3d203cad90ef882ca08614af76e2e3dfd92faad.png)  

>有收获了
>

## 提权

![picture 13](../assets/images/913b52a10860357bc916f63375241f3fe1d7c96396039978ae6facff24e90ae0.png)  
![picture 14](../assets/images/d50193afa8762abead3cdca09df3feaa92e2a43c9783ff7c90ddd5f080ebaf09.png)  
![picture 15](../assets/images/adae3bfd5f63d5f8704f243f806ad26f80f8cfc572be16a6af957527f31ba890.png)  
![picture 16](../assets/images/4263e4a23627b77712f96147f9c9d2819c66d6596bb67adea95baf501ff090c6.png)  

![picture 18](../assets/images/9a99f8582a1f1c850609b59e91d855f7699e119cfd9c855d28b87c25c32bbdf3.png)  

![picture 17](../assets/images/01d177c5909c1e00d06361516d4ad78fdfc7c36c7b3fabd28c3210b739561630.png)  

>输入beef即可
>

![picture 19](../assets/images/485478dc3682bda29d65e714a76b5e7f8e831bca4d82cfcfc1acd79c41f16a88.png)  
![picture 20](../assets/images/b6b30b420670a0353ca7db66cfd1fa28c91efbff89e9acba96526228c4662c54.png)  
![picture 21](../assets/images/4f68c6842ba38b5e61a71b527528f7b57580c1e5f42b28031d9503f25761abb0.png)  
![picture 22](../assets/images/a19f77f93776a7ecefb848a768a4f514405d35735852bad526013872ccbd3579.png)  
![picture 23](../assets/images/c63eb70fa9b3feb8899f36456318e071f17c6d33581fd60963c1d71443a84cfa.png)  

>好难看直接用之前那个了
>

![picture 24](../assets/images/754503436f885623bc46cff4a701666b3a125c6802f6f9c3aab4643abf6b00d3.png)  

>这个好匹配
>

![picture 25](../assets/images/7b18b6ddba7ea04e0481ebb4d8eb961e9e021969bb4f4458a9941c66d121c34c.png)  

>还是有点难，手动一下
>

![picture 26](../assets/images/b327b17e6db4cc26706874e184c0f932c6798a3f464409e22a7dadcddeaa77f9.png)  

>好了
>

![picture 27](../assets/images/9114874c489919fd3ea26a82b47deea7d2db43b654bad046c70a9acb431a6a3c.png)  
![picture 28](../assets/images/5bbd5c18ed527244c3ad2fe58aa8a1ffd04e9607d149639cd43db9834655a8ce.png)  

>有密码的话就sudo了不如还得找方案
>

![picture 29](../assets/images/497b70a954ca09fb97608d48a8a12e72ae2430b6d23300f8d67739399b9da7b9.png)  
![picture 30](../assets/images/9435eb5d0e2c7123a01eb81df7f87aecb431ebe41a6514ba5b0b11373cf3f413.png)  
![picture 31](../assets/images/eda35365da3a02e4bf9f06fdd44697dcc50d928ec27b2a57e2f816a7e2329d31.png)  
![picture 32](../assets/images/3b211120e7f7ceb83adfc06d53f9fc44de8406b230d9e65ae6b28d79922b4702.png)  
![picture 33](../assets/images/6e89515a98879b49ef591db198ed1b37d5d6f13ee46d5226e0ec76c5f2660a1f.png)  

>能截获？
>

![picture 34](../assets/images/42d9814081fc3e74338df59acdf0ab0aec80252141dad1d76846b25134db05e4.png)  

>无定时任务，如果不是root密码的话就内核
>

![picture 35](../assets/images/686272223bf42dabc5e41bc318c0915a7bddb5f68425c915a8780838a8ed5380.png)  

>尝试一下不过不行
>

![picture 36](../assets/images/c79b57e5a9fb731c806b1ed9092a6901e262711def112dc23cf69c0e31516887.png)  
![picture 37](../assets/images/f08d772e90a96e927b8122d3d8d59140c00b792a29276266fdcd347405e8d28d.png)  
![picture 38](../assets/images/62248663414d4307e6571252c91c3f56ec274d0c56a096057f6cae17f4b950f4.png)  

>没有印象有没有内核的，看wp要范回去用那个uid的东西
>

![picture 39](../assets/images/b3582e7b8ee55a626f7289753146bead8422d20e40a93609a34891a9cd923f21.png)  

>在这里不过要利用栈溢出找到这个地址调用执行，现在主要找到溢出点
>

![picture 40](../assets/images/51788ffeafecff14881289002ccf17085d57841d18b4ea692ba40498ae4c5ed9.png)  

>貌似环境不支持
>

![picture 41](../assets/images/8e6e8e17599bf418bd95678b4895755950667061300b44fae1b6aaf48a3055a6.png)  

>破案了没有so打不开，环境短时间是做不了了，可以尝试看一下那个函数地址
>

![picture 42](../assets/images/f6a3c4bf0ef09744ace646f8f95c15ec24650741781190ee999f484f3ba993dd.png)  

>这地址看起来和wp的不一样
>

>算了用了才知道主要不想打开另一台设备
>

![picture 43](../assets/images/23aa192eb2a65ba5585cdcc900091d91fbaa8f44a2a6b2b868e674ed4b288eae.png)  
![picture 44](../assets/images/df7fb0ac8afc07a210ed8d9414d9b2bb0cb0ba6cd6859071f9474db6cf319290.png)  

>不行
>

![picture 45](../assets/images/a9b5339ca7bcaf1a0833823bb6288909eeb5161bc2796fa40a2c77545006be91.png)  

>他就给我这个地址
>

![picture 46](../assets/images/cf4d871475d76b7d01fe776732f866fde5891d53815e57ade3f55ec0fc5022fe.png)  

>用一下wp的
>

![picture 47](../assets/images/e9d5cb2a53dcac5944b9a6620dda9d57e98ab9f8abd43f8e2f57ebac99801b51.png)  

![picture 49](../assets/images/d8f5a824a283d06a45190b0b1438a86288976cba40a7e71c64b1d8daf30770c3.png)  

![picture 48](../assets/images/4de2d2b4af8649d808dc232aa9e95ee5c075cb58eda7f331ace948f99606fb3a.png)  

>现在问题是地址和溢出的wp的我得搞个自己方式的
>

>先搁置一下我等今晚换设备再弄
>

>userflag:HMVolympiangods
>
>rootflag:HMVgodslovesyou
>
