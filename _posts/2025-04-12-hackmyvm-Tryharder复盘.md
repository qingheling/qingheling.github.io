---
title: hackmyvm Tryharder靶机复盘
author: LingMj
data: 2025-04-12
categories: [hackmyvm]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.159	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.078 seconds (123.20 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.159          
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-12 04:21 EDT
Nmap scan report for Tryharder.mshome.net (192.168.137.159)
Host is up (0.0074s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 93:a4:92:55:72:2b:9b:4a:52:66:5c:af:a9:83:3c:fd (RSA)
|   256 1e:a7:44:0b:2c:1b:0d:77:83:df:1d:9f:0e:30:08:4d (ECDSA)
|_  256 d0:fa:9d:76:77:42:6f:91:d3:bd:b5:44:72:a7:c9:71 (ED25519)
80/tcp open  http    Apache httpd 2.4.59 ((Debian))
|_http-server-header: Apache/2.4.59 (Debian)
|_http-title: \xE8\xA5\xBF\xE6\xBA\xAA\xE6\xB9\x96\xE7\xA7\x91\xE6\x8A\x80 - \xE4\xBC\x81\xE4\xB8\x9A\xE9\x97\xA8\xE6\x88\xB7\xE7\xBD\x91\xE7\xAB\x99
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.70 seconds
```

## 获取webshell
![picture 0](../assets/images/e344eaa0e60fcb5d1665d0cac0b1d8281637808929b663239498ad09b9e12dfa.png)  

>正常扫一下目录
>

![picture 2](../assets/images/dbe892cc0244708f6e8576376d504ea35c96a5a07f271c0910a20e4c4b9003f6.png)  
![picture 3](../assets/images/d3854bb8d2dda104113826f11b90ad696622eb6b5d4c065b1d9ee1dcaabe3f9c.png)  

![picture 1](../assets/images/074990c6880f5df9f1e1a102f1ef1a03519c5459846c50acecfa712b277dc4a3.png)  
![picture 4](../assets/images/a1223f133ea812b08fb977b4bed022dd4e2fd236fb77be3ae8e2a2a794c774dd.png)  
![picture 5](../assets/images/44ca02c4902f127ad0d41d5bbad1247a97b051f40e7ca4481bd8669de7483c6f.png)  

>无密码少了还是什么
>

![picture 6](../assets/images/4c28819c6f81896148b99c2ddb4d6c379b153ab2b7daf1e0f0f1f2100823d075.png)  

>用户名的问题？
>

![picture 7](../assets/images/a23095d62fd415a005a7c1843356dd18fa0d87368d43158262fbb704ccd18e1b.png)  

>换成74211也不对，直接全爆破先
>

![picture 9](../assets/images/ba44b3c0b610d546f176b57f5939f8fdb477a6279f9a01be975645ebb3b430e0.png)  

![picture 11](../assets/images/8dad45ba9deb39ce82c9fc6b64c242ea353a82365473cc3b2e33d5f62c584d8a.png)  

![picture 8](../assets/images/1142fb098e3a3e0ac44d357e0dd20121b57ff3bae25f8b2bf1788c9ed9324ae7.png)  

![picture 10](../assets/images/f2779bf00a1f555d723df53ff063dcb331724ca202764a0004707f623040633f.png)  
![picture 12](../assets/images/250bcbac107faad47e380f18143e09be50d4357befc99ddc4ebc226808dfdd57.png)  

>方向错了，目前看这直接
>

![picture 13](../assets/images/a93a5ffc669ea4f6452f1729a4045d9d2438d6e9caee64f5338d85f781c31b4b.png)  

>需要读文件么，还是扫描有问题缺少东西
>

![picture 14](../assets/images/826d692e6f10e919cfaaee45e4a6635242b678f7da8b4511b33aeef189155e4a.png)  

>无
>

![picture 15](../assets/images/97db7637dce14e72f327660c9977641ed93575dddc3e44ea16e7fb01adcc452c.png)  

![picture 16](../assets/images/07ceca077a4dd3ab3bba03085cc3b81692467a7cd9a9d4b9075aa089f327ea88.png)  

>整一一个用户名密码全爆破
>

![picture 17](../assets/images/58a5558549f5fadc8d01834408e1bee68748650c5ad0d815bfc1d79042592929.png)  
![picture 18](../assets/images/9de1032a95c9b355bab401c895eb6e1da7565f24e2dda50ae302eb682802543b.png)  

![picture 19](../assets/images/6d18580ca227fc450965c72dca3a97825b517dbe726ce9d19be08e7bcc104f79.png)  
![picture 20](../assets/images/42bc2d380e9bb1ab3afd0caf8311d328a689a43be182edef9c0e4a1d8369d640.png)  

>改成admin
>

![picture 21](../assets/images/e53b5b3c7078004be72ca05c942736a307f8ed400b39fda5e7faa7aed9b0947c.png)  
![picture 22](../assets/images/f67d8677bd961b8b35072faf9cd2469b72ff87e0eb42bc4018a44717c826d16e.png)  

>改半天了这个jwt难改，上次的love也是这个毛病，头疼看看有没有好的修改jwt的方案
>

![picture 23](../assets/images/bc08b030d0d70b56734159cd08e40502cae03a00b5c319c6fe3a12841c6e0c1f.png)  

>找了一下有密码，我查了一下可以利用jwt改现在问题是网页改不了jwt成admin
>

![picture 25](../assets/images/2f15b276f8e5abce36137207bb4049102d3fa39a7d42a36aaa16766903844ae6.png)  

![picture 24](../assets/images/1b27f2efb237483974dac486233d5823d635a0ab74ac8d0d108a89bbf41b0d95.png)  


>好吧一直没找出密码所以导致没成功
>

![picture 26](../assets/images/eae09ad95a05a322987392771d54bdf91417e00cbb2e0fb2f7e26ecd8e6bc3df.png)  
![picture 27](../assets/images/326a048639f88799f94cf11f55765587341d76c453a0ff87929304a4261ef90c.png)  

>如果是白名单可以测试.htacess
>

![picture 28](../assets/images/84295c0b1e91f50eef0ba9711535fbe9c102014d58f9c548c72a7560067db549.png)  

![picture 29](../assets/images/49e42011850fb101a8acfe82c0c7ebb391a81b77848788062e424e7c4828e4db.png)  
![picture 30](../assets/images/1d283a2471f2f71b47a5b3b9f083680f23bd1a8879e24a47c0625704f41ae479.png)  
![picture 31](../assets/images/d86246ac8e72ccb9d0ce2df2d8a8d25520b519f343db1a9b978b5d9137d99e89.png)  

>还行就是jwt和爆破用户密码费时间
>

![picture 32](../assets/images/534ac2f1586176c7f890cce3a8fdee0a974ecafd309cbdaa102af5e0996abafe.png)  

>直接busybox
>

## 提权

![picture 33](../assets/images/d3fd3a5ce87bb23a5a1f68d3953ad226558d6a2cd1c168365d658551d916a680.png)  
![picture 34](../assets/images/f8878cb43975b5ab2b80bca637e8e1d194b3892bcf1200db6b24350fccbe8e55.png)  
![picture 35](../assets/images/ba8002a123778994f61feba629007082d1e7084f97b89a9aae0d610e968596be.png)  

>留有工具直接用，哈哈哈
>

![picture 36](../assets/images/48f94880c196988e69423737288806a819640e02e667bbb930b0dea121abf281.png)  
![picture 37](../assets/images/f1d47493caa81f035f94da0abd77c498e3b109a6f072679d5c34b97481669d1c.png)  

>这是什么
>

![picture 38](../assets/images/e50ebde1cc27b2fb77fb16689dbb1574147237e9a642cbbe44cbd7c9476d1319.png)  

>扔给gtp半天没相应，算了看看直接爆破密码了
>

![picture 39](../assets/images/2f8682983fb303b0e88e3c748b1dde43e7a3e991d29926986bf4934cc466f8ae.png)  

>这个明显看是文字类，看看其他提示，对密码学没啥研究没看懂双城记，我看wp了，明显一点也不了解这个东西猜谜除非整成cupp字典里
>

![picture 40](../assets/images/fb950ebf82b3dda63c598355ac376a0430fa57addad2b6969aca09b2e0a3d3e6.png)  

>密码也无
>

![picture 41](../assets/images/3d1fa5c2131b401ff983c72221335e4eaf8df9a5111f61695afa11ea85a31f92.png)  

>看巨魔wp这是方案
>

![picture 42](../assets/images/a0e2618c9206d265717675ba7c29dc072c097ab3e6456454eb7803fca66ca0e3.png)  

>用2个明文的差值进行字节转换会得到二进制的b'\x0f'形式，不懂没研究过，直接跳过，一伙看ll104567大佬视频去补知识点，先打后面部分
>

![picture 43](../assets/images/745046acd2353fa7b45687a9235b235e448b47d182f89ec04fb5cc1bb61f02a8.png)  

>大佬写的代码为就不直接复制出来如果想要的话直接：https://tryhackmyoffsecbox.github.io/Target-Machines-WriteUp/docs/HackMyVM/Machines/Tryharder/ 自取
>

![picture 46](../assets/images/7fc3b0cffeee72d5fd9f740c21c888e631ff4b4f62e790b1ecbd4d37e305150d.png)  

![picture 44](../assets/images/ced9b9d5892771e47e3b65378338410a1f829c90bdae468ccf222acfc2dd00aa.png)  
![picture 45](../assets/images/019ab3ddf954d06c3fe4be38d191a90862e93e50612b767647c5f8eb550c71f4.png)  

>这后门脚本之间利用
>

![picture 48](../assets/images/3d79f2e34f14530607813a1def4d40d9d3a3f9b8c4b569871ec563bce32fe640.png)  

![picture 47](../assets/images/d686b46d665bc11e338b21850e5fef6b2b5fb3c47d40f800d697a5aace4e4d78.png)  


![picture 49](../assets/images/260277ec78f31a689f1b0ce45df278342086f8dbfceda02fef7b931a343ec334.png)  

>密码是啥，哈哈哈
>

![picture 50](../assets/images/18306650b55d39415ccee319fb10d5733856f39fc3a3f720485711ebc0960033.png)  

>又是这个？
>

![picture 51](../assets/images/3e13036db7a1877cfb48b971fa506544da6751febbc0a80ae0317458bac26f15.png)  

>为啥不能改呢我很好奇明明我们是文件权限员
>

![picture 52](../assets/images/caf73dcd75c5a628c6be88714a5b12cc4880ae13162844a590fc9892195e923f.png)  

>很好完全不能控制算了我直接猜数了，看看能无限爆破不
>

![picture 53](../assets/images/ea9d622d93db5a9d172e132c66d46ae87eec1407dbd227152644b3d1115383a3.png)  

>这咋猜呢
>


![picture 54](../assets/images/98421294a50c24a7ab759d44e73317cbc7b1745744d6c9f41bb32a7a9235bdf9.png)  
![picture 55](../assets/images/80cb8ff2fcb5d514c5d96c1989b153bf5e13094777bd6422f189fed32dfcf90c.png)  

>猜到了，哈哈哈这个方式屡试不爽，
>

![picture 56](../assets/images/79e774b3f7cc4e491695b20e60db2362bbc0faf10bca80788472350383d2e54b.png)  

>还是没有root密码还得提权
>

![picture 57](../assets/images/0b7c1f4fa3d751ea4b54a3c42fbf05ad8289db150d9768bf42f1c5624d66116a.png)  

![picture 58](../assets/images/ec543440f6fcd6eddde3d4f437d1dd871d487971ce55a38cc4825e5065d3c8a3.png)  

![picture 59](../assets/images/59aa6583b65b7e60eef31fc06f8d20efd8924f692b49a1ed41460e615b62e06d.png)  

>ok结束了，这个密码猜谜是真难
>

![picture 60](../assets/images/1ea1c16d2adb4e17176060179c7076538f8bd230a5e57951a6f614c105f655da.png)  

>我就知道加了这个哈哈哈
>

>补充一下那个猜谜的知识点
>

![picture 61](../assets/images/257d4f1caac44928f4470abb40911b36258f48da44ca135498fd9a71a9106447.png)  
![picture 62](../assets/images/7bceb46af3947769469872ee7edabc871229ca9ee62952c26034c4f26cc3c34d.png)  

>back的密码不存在爆破了
>
>OK看了wp我懂了，自己写一个简单脚步加好了
>

![picture 63](../assets/images/ba39a8a4a1ca410b0b6aebb3cb088557f29a8516f6193b2dbdfc21b9c52a4d41.png)  

>主要是根据是否相同的说法可以得到一个二进制当然观察字符也可以
>

![picture 64](../assets/images/f956a407132ee7000ec9fb37c8426fd7871b81c80ddbb5c9fc8b5050d6c6286f.png)  

>接着拿出我的acsii
>

![picture 65](../assets/images/695b61fd5b3aad4d2cb5ac5b81bb89420e706f434842b3491c155a890b83a7de.png)  

>很明显a和b不同字符相差1，当然直接不同等于1也行是大佬的做法
>

![picture 66](../assets/images/cef617ad4c627e9d56c99c43fd77175a4ce6404c57244f28bd1e961d3159a925.png)  

![picture 67](../assets/images/9880e01bbb240e03ef0e22b983d08450c2e62711189768afd3dee2af80f2d916.png)  


```
a = "Itwasthebestoftimes!itwastheworstoftimes@itwastheageofwisdom#itwastheageoffoolishness$itwastheepochofbelief,itwastheepochofincredulity,&itwastheseasonofLight..."

b = "Iuwbtthfbetuoftimfs\"iuwbsuhfxpsttoguinet@jtwbttieahfogwiseon#iuxatthfageofgpoljthoess%itwbsuiffqocipfbemieg-iuxbsuhffqpdhogjocredvljtz,'iuwasuhesfasooofLjgiu../"

c = ''
for i in range(len(a)):
    c += str(ord(b[i]) - ord(a[i]))

c = [c[i:i+8] for i in range(0, len(c), 8)]

ascii_str = ''.join([chr(int(byte, 2)) for byte in c])

print(ascii_str)
```

>ok简单写完，懂之后就简单多了，不过都是马后炮的事，笑笑就好了
>


>userflag:Flag{c4f9375f9834b4e7f0a528cc65c055702bf5f24a}
>
>rootflag:Flag{7ca62df5c884cd9a5e5e9602fe01b39f9ebd8c6f}
>

