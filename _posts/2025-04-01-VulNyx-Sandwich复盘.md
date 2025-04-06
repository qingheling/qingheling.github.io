---
title: VulNyx Sandwich靶机复盘
author: LingMj
data: 2025-04-01
categories: [VulNyx]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.164	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.145 seconds (119.35 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.164 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-31 19:48 EDT
Nmap scan report for sandwich.mshome.net (192.168.137.164)
Host is up (0.039s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 4d:30:db:f3:d0:b5:b2:65:8d:3b:08:dc:56:2b:28:b9 (ECDSA)
|_  256 16:9f:f2:7f:ca:5a:a2:03:65:9e:f1:09:ae:15:f7:8b (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Sandwich.nyx | Your Favorite Sandwiches!
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.54 seconds
```

## 获取webshell

![picture 0](../assets/images/7cbf14acc41e786c05cb4935b37a94303c3eb73281dfec4010df245ea2f6c724.png)  

>存在域名就加一下了
>

![picture 2](../assets/images/81451b1d6aeda4044730a67358d1fd799712a5cca35c3b25fb35e0b89d361e70.png)  

![picture 1](../assets/images/e295bef90a172bfdf719e0f36db907ab47423fe309e688ad126be571b2019571.png)  

>好像没有域名一定控制的样子
>

![picture 3](../assets/images/39b0036d7e576540014dbbb1953c0681a1d1d93e8d480ff5ff6c31a8765ccdb4.png)  

>目前看必须登录，我看看咋拿到用户和密码
>

![picture 4](../assets/images/b0de28f0ef45a67a469ff9af314af9469d5d4d7d63a1b3ee13d8b2c5bbd52862.png)  

>先注册
>
![picture 5](../assets/images/1da5f502ba17d02c6724ec9e6aa3e913235d95fb95effcb0f17817547ffa55e6.png)  

>得用域名
>

![picture 6](../assets/images/1a9f586caa8d3a3db069b5fb53d56d8638646a2f781530c88f2cbc2d14e6396b.png)  

>要一个user,我先怀疑sql注入
>

![picture 7](../assets/images/a74c92522c062d908419b38ced6a4db25f2cdb5810b9e3fd61cbde05a1507c69.png)  

>无
>

![picture 8](../assets/images/6ea41bb2995bee0de3cc55d0fe9c742799bb203afb31d5afdd323a09b63d2844.png)  
![picture 9](../assets/images/b74b1b8b2cb005e1a8e723bb647e101539051ea3fd435c46dbde229a998dc83e.png)  
![picture 10](../assets/images/3b74563163d74c08d3ed6169b2cfed73042e8355d5a5a41722fb5094afabfd2a.png)  

>目前看把目录扫了看了能看的地方没有收获，主要是无法注册，看看子域名了
>

![picture 11](../assets/images/4f0dc2350a773f5cd4209bb620928ea4eeed4509aa064ab1c22c59c45c4ac673.png)  
![picture 12](../assets/images/18c922fde8dff01c73c4e1519f1381a8dba5ba1a9942f4f4a4076b1fc8a8d694.png)  
![picture 13](../assets/images/c68c22c0949330d6df24044897022450198843d1e47f4036d7fff82897834f90.png)  
![picture 14](../assets/images/6f64f840b157015b586cc3c2a7eb8e792e2b82f00c2de515e974410fa5e03dc3.png)  
![picture 15](../assets/images/47574dac00ff0d5829eaa717f22d08127ac8226ab64a828ed3aeb4c52d39d389.png)  

>验证了admin账户
>

![picture 16](../assets/images/741529859df1249ccd7b5aa6ce96c0c83d3dfb0b4897fb9b38713fae703df49b.png)  

>可以随意创建
>

![picture 17](../assets/images/2f33c02c36060fe443fff6b37938d12a70e7478ca2e67bce334ec265e4033a61.png)  
![picture 18](../assets/images/423b64517d07a9f498739ab5e41cbff0724c2756fab6ac3e304fd631dd2264f3.png)  

>我还是怀疑是xss，这个作者老喜欢这个了,爆破密码先，不行就xss了,爆破没用啊
>

![picture 19](../assets/images/01cb981b4d0e52fed8ac2ecf7c36607e84f9047773e5cb2ba5ed43137bdbae9d.png)  

>2边不互通
>

![picture 20](../assets/images/020115f2dfbbf29d0bd4fa181e89929ceb8e3e3b242f42ee5ca895024a3234e5.png)  

>诶有点意思,登录没啥用啊
>
![picture 21](../assets/images/49810178000cb928313d9608186e12437e2400f2563f67578a630b14a7b90248.png)  

>现在感觉只有xss的路线了
>
![picture 22](../assets/images/f1f9d095451ea02867692366e46cfb65b4d7f2f759bb5da35e920046c1c4710f.png)  

>没有么，方向又错了，还是我打错了
>

![picture 23](../assets/images/3fbe1125560fe650073361b27fabb018e05cf185186e4a1c9b0de35213ff6d3b.png)  
![picture 24](../assets/images/c4141fdbd57df6f57803c270cb14581eadd9709604f911ba102fab5abfb9ed32.png)  

>没啥想法了，重新扫目录吧，没准有东西
>

![picture 25](../assets/images/eb31a03be8b8c9900be6589e4ea021ddb43fc1896abeb43c3f998a31d7f8a88a.png)  
![picture 26](../assets/images/86c86c2f2711813e9342b0243c11d438bbab9c3c46af20f3b1b422794374bfb8.png)  
![picture 27](../assets/images/850c7b1dda02418e38a70e00bfae397dc5795dcde51c8e7ae2f4f5d584c4f4d8.png)  

>找php的方案了，我感觉我的思路极限了
>

![picture 28](../assets/images/6506588583d5fb37536b4e761a29f09257355489be54078b4da25627153f67aa.png)  

>是什么呢
>

![picture 29](../assets/images/6d46b5bb73b79ca7ece1d480366a7839feb97007f879995763ecdaf897e3defa.png)  

>搁置了，等大佬下班先哈哈哈哈,好了大佬们打完了我看了大佬方案由he110wor1d大佬提供一个2个方案
>

>我先用常规的那个得写一个脚本去完成,首先我注册登录过了所以现在直接进行
>

![picture 30](../assets/images/39ee2f629790aa4abecffee5078ec0c7501060363ff0642e7398a432605f5ff1.png)  

>我们第一步的操作
>

![picture 31](../assets/images/f6aabdcf5a3a40650554ea4af943ffc0e8e7fcbbd253b6f19c5d297aa9bbb7dc.png)  
![picture 32](../assets/images/5e762be25e886e3da6e1f99302c3ec86709ebf35e8180912f7e1a697fd28f590.png)  

>这里可以看到token出现进行密码重置，这里token是根据时间戳完成的就第一部分需要变动
>

![picture 33](../assets/images/6906e56c5eb694a6e01dea5ce77eba0131619d17a2d2b41300346eb5f137fe0d.png)  

>具体需要写脚步完成的操作就是发一个我的，发一个admin的，发一个我的，当然如果脚本不会情况我想了一下其实可以利用bp去做，我先试一试
>

![picture 34](../assets/images/4ad4362487e3e27deb5bffddd4a4751b0ae3c0e880bbd0e2ee403a22c65aed0e.png)  

>如果能调bp的速度这个方式也是很好的
>

![picture 35](../assets/images/1cbc65be18ab8ee03d59bfc6814200df0c57e6a4d16c4a2b2f9b87cc7af40700.png)  

>现在需要算在178922fa到178425f2的时间戳了
>

![picture 36](../assets/images/e866aec2d2b84d8f39fec3edea492b3bba490e6e225f644d4efcfbabe22df37a.png)  
![picture 37](../assets/images/b440235fe44129d5225fa253bc82d64c512a5983ca01fe9c7738c1e5c3e0e9c0.png)  

>接着需要使用wuzz批量对密码进行操作
>

![picture 38](../assets/images/43b97a58b7fbb6a4f867784a0902e414b4aa2adbf8d2e80e8f0a19cd63d3d167.png)  
![picture 39](../assets/images/8ad546b898a258f8385b903f537ebd29152580a805a66725f83c4e261dba9f0f.png)  

![picture 40](../assets/images/44c70d24ef5760fb7d70e01ad190d4107d57479c6c816380930d7eb1c15b5d57.png)  

>看啥时候成功了，写脚本也行其实但是我呢比较反人类，哈哈哈哈跟别人wp区分一下方法
>

>不保证成功性，我进行一下别对操作就是方法2
>

![picture 41](../assets/images/fa09ad9f419c4f69ac4cadb41ff8e0590b46138348188a1a89c9b6e680421ede.png)  
![picture 42](../assets/images/fca434922688053e1db31d092dc6021dbf84eddc3eb8c9f08089d7fd29f39386.png)  
![picture 43](../assets/images/5147cc03fe4da3b2eaced0e9545899142eb1b4237d835787091c59569f348fa2.png)  
![picture 44](../assets/images/eff788c633a57a13a106f48f7db2e905aeb3f9135fb04beb4bd2b7e91c8ad702.png)  
![picture 45](../assets/images/698de8ddc45de0779502233b943e7cbfa9c74811d7b71f9a99c81a3c7ce7ed4e.png)  
![picture 46](../assets/images/a62e6366b2e28b0789e9d86f60e096c5c9ce3ec02191e5f86500b785256d3ef9.png)  

>就对身份简单绕过，你刷新呢也是会重置的
>

![picture 47](../assets/images/047c0595cacf95fd34f8cae6cc500adb6cd62d7148bce968fb20129c6d6711da.png)  
![picture 48](../assets/images/896733d0b7253d6e986c282a7a5d4dfb6a1107d8f84ce0710ff147e205280b31.png)  

>接下来爆破密码找对应邮箱
>

![picture 49](../assets/images/c3f9d4fc49ae95410ca3699f1cc55068f37f1f6a1aa0cf4e71302f02b777b5e2.png)  
![picture 50](../assets/images/0cc951593e98a112d10977275394e2bd6cbfe38d02a91bd378756616b6bd91d1.png)  

>挺慢的不知道为啥
>

![picture 51](../assets/images/f13922a72eaed6731457964ee4c1a01a691895c19b15b266e87da1d7bf069892.png)  

>奇怪这个wfuzz不显示爆破让我很难看，由于bp太卡不知道原因我打算用hydra去跑了
>

>虽然我在跑但是感觉不一定出因为没有域名啊
>

![picture 52](../assets/images/a195fc0973542e75e751788d50cd1f3adfdfa44aee8351184ecfe1d3138b9b47.png)  

>果然没域名的情况是不可能出的
>

![picture 53](../assets/images/53d165a6e812015983b09da7624fa6d6c769c30909c38457bce97b0679f16545.png)  
![picture 54](../assets/images/a5c3b9caf28edbb5affdecd406daba8af2b0c00f723c8fff4e12fab1623778c9.png)  

>密码改成功了，差一个步骤了
>

![picture 55](../assets/images/f914e0ff9b135634781ce378ac475b9f109d967d848c53217aa51c5964028f9f.png)  
![picture 56](../assets/images/03e6a70b660a59759e48650673f9edf54504cee52d3a56f4e1e817961c3a0de0.png)  

>OK下一步，有点困了，感后面不是很难
>

## 提权

![picture 57](../assets/images/9f5b2f6a5b538b103488dc13516cfc27b606e0b871115ba1aa7523f147c13d5f.png)  
![picture 58](../assets/images/f65ccd2fbbddaf793582ef46d0b6d768b9e8d5443345171cb27ff7ec48575ca1.png)  
![picture 59](../assets/images/e63b27d578da6d29214f43ead6ab33737ff9ee8168c3e877424ba97ebff3e5ce.png)  

>不一定爆破出来,跑工具
>

![picture 60](../assets/images/e944e9b4f5ad411d659b6c1357e4f01480663d561ca992ccc5fd87e515dfa7ee.png)  
![picture 61](../assets/images/5d68aa97fc678dd4524ff1ef96bf879655c644c7cb01dff04402c03e0408087a.png)  

>不知道，看wp一下，发现这个chvt是个新东西
>

![picture 63](../assets/images/97a81880fa7d5924b5cd30c30088629fb5578cc577965386499715ca594f877a.png)  

![picture 62](../assets/images/26829f548dfd8188b93b9974fcc1d6629c3a12f6fe429da560f9cd267fd19f67.png)  

>给自己加权限就好了，写个私钥进去
>

![picture 64](../assets/images/14b6f1eac1edcf9e74663575d5eb209ff1e96f642380a526cf5af402622d0e9f.png)  

![picture 65](../assets/images/4516efe8b6d942aaf37619aa2f916fa2dd5b35592ba97ce9974c90de79e8c048.png)  

>又是一个脚本shell，直接gtp分析操作了，太长了看得难受哈哈哈哈
>

![picture 66](../assets/images/e889d4f473a128a6a0b26d9bb55663f835b01432ea2bd1a398eea73793bd3083.png)  

>一个猜数字我感觉我运气比较好哈哈哈哈
>

![picture 67](../assets/images/e7b972a2481e22327f397c88519665d9ab14a5c05b24ae6f6db651b459158137.png)  

>好了结束了挺有意思的，不过又把我能设计的点缩小了很好的靶机又多了2个知识点
>

>写一下脚步处理一下吧不然不够优雅这个获取方式
>

![picture 68](../assets/images/54912dd84f1b05d1076bd0e3eaa517592db21e3540bda077a55165e43e742e35.png)  
![picture 69](../assets/images/fea89ce27357c27df6d9be2412a2a9560aa3b01ec6a77a57ac794c69956884ee.png)  

>小问题不太会用查查资料
>

![picture 70](../assets/images/a46f7629e190e9961088a4ff6f563f64bb1ae72548e201783d76f0ac2a83a0b8.png)  
![picture 71](../assets/images/ec1bcbfa83948ef4b78ed34eae2e93cec0d6a93abdb70df370aff2206cf0ac4a.png)  
![picture 72](../assets/images/cebae83ca2960a2d1e6a46dc9a738c29bf9e2b289b02c9c28f43320e2759dd88.png)  

>好奇怪一连就断,等一下好像我的pwn库坏了
>



>userflag:c158efefab9bfd356fa8e9ec3c440da1
>
>rootflag:a4e728e6ffc502beea7570a75348af44
>
