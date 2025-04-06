---
title: VulnVM Interceptor 2靶机复盘
author: LingMj
data: 2025-03-29
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
192.168.137.41	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.172	62:2f:e8:e4:77:5d	(Unknown: locally administered)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.028 seconds (126.23 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.41
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-28 20:42 EDT
Nmap scan report for debian.mshome.net (192.168.137.41)
Host is up (0.020s latency).
Not shown: 65531 closed tcp ports (reset)
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         vsftpd 3.0.3
80/tcp  open  http        Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.62 (Debian)
139/tcp open  netbios-ssn Samba smbd 4
445/tcp open  netbios-ssn Samba smbd 4
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Unix

Host script results:
| smb2-time: 
|   date: 2025-03-29T00:42:44
|_  start_date: N/A
|_nbstat: NetBIOS name: DEBIAN, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.52 seconds
```

## 获取webshell
![picture 0](../assets/images/ad8bde13206d9c124a1f3580246554ed9c2f45d1d51d9d202b560775ef45ad67.png)  

>没有东西，扫目录了，不过里面有smb，ftp应该要爆破但是不知道用户
>

![picture 1](../assets/images/6b9fa7b26c3f2ef2fe8f35e554aedb4918444944c5137189e04f13b8e1d07d32.png)  

>新线索
>

![picture 2](../assets/images/f79168acde142b32ed21e54f22a390bf3f0a09928e7f8241cdbba7d8466a6057.png)  

>和1是一样的？ 我试试1的方式能不能提权
>

![picture 3](../assets/images/eaa457b8b149fa6df3b7a20fbbac5c44ae5bd6e5fb14487aa1a79afe428b4ca0.png)  

>应该不一样因为1有ping的php文件这个好像没有
>

![picture 4](../assets/images/c78a4df1e69a048e01ad90c23f3282f6622b3d9d5b85a5a40feb332c80b48bb2.png)  
![picture 5](../assets/images/6718382c768ad01d5d7dbad9b84430576c7d7d65b7f473acbac8ce96e3faee0a.png)  

>看看能不能匿名登录，不然爆破一手
>

![picture 6](../assets/images/4d876ecddca16bb683ec9d4d903a87c11ce5bbde147711807c01a7e2ac03d711.png)  
![picture 7](../assets/images/67b031e279893cf7c1388cf98c44d6ef8eec8f43b8deee1d7b597822f536f8c1.png)  

>第一个用户smb没有，可以考虑ftp了
>

![picture 8](../assets/images/2b203008bb46e461486ccda6f735e208f80ff50aa5cf1aac539897a145afbe2c.png)  

>小拿一手命令，不然我感觉我的hydra跑得很慢奥，等着了
>

>跑半天了，会不会是cra的问题之前就有案例得用msf跑
>

![picture 9](../assets/images/eac7649d2cc19dfecb361bcac06c871f0384c445f4f0c635bd56b2c59fe10ea5.png)  
![picture 10](../assets/images/d700203db7f8c33250bf7264668b59705a509bc6bbafc76885147dc3d484adea.png)  

>验证了一下确实只有中间要跑密码，先跑半个小时不对就换了，总不能这个靶机跑密码硬控1个小时，好了得到提示，跑密码跑到明年都跑不出直接转站wordpress
>

![picture 11](../assets/images/4539d954c7dd09a251367effdd195c065f52d14a8f5903b1aacf68eb4ed553f9.png)  

>搁置了，没有思路
>

>好了拿到了一个脚本
>

```
import requests
import hashlib
import time

url = "http://192.168.3.181/wordpress/wp-json/wp/v2/archive/"
timestamp = str(int(time.time()))
nonce = "mahalkita"
secret = "supersecret"

# 生成签名
signature = hashlib.sha256(f"{timestamp}{nonce}".encode() + secret.encode()).hexdigest()

headers = {
    "timestamp": timestamp,
    "nonce": nonce,
    "signature": signature
}

response = requests.get(url, headers=headers)
print(response.status_code)
print(response.text)
```

>这里肯定需要改一下东西
>

![picture 12](../assets/images/5077df36eafa8b77bdc3b47d8217fdea4ea1c7700401238acff1391191561365.png)  
![picture 13](../assets/images/ea7f378da11b1bd139077914de7dd301f05b469ea708ff0ba89566190d3b31a2.png)  

>目前都没思路，看大佬非预期解。
>

![picture 14](../assets/images/b838b864384eaf9c09b398fc134442307b1bf25cefabf2a2c85fe8bc4b995d75.png)  

![picture 15](../assets/images/de1ab469bc238be40580a945f5fe2e99e95d7bdba74b7e8b7dd5472248b25c8e.png)  

>闪电侠的精简wp对我来说确实很有看的难度，我可以看到它先打过一遍再写的wp，有些地方比如字典我就没找到,压根打不了
>

>我看了wp，看到一个牛马的东西，就是wordpress原来啥也没改，我直接看上个靶机用就好了，如果可以我评价这个包是垃圾靶机
>

![picture 16](../assets/images/d6a49422fa3e73e99c195f8693bdcedc73f871db4aa3df03f9f14deb56d7c1b5.png)  

>这里看我自己博客的截图
>
![picture 17](../assets/images/184960121af7c6b1d576162cbc9b98a4ea1fec39cabfe0a663b1bf2f028c2416.png)  

>没成功哈哈哈哈，确实不行看下一个，可以感觉到明显不一样，算了我感觉继续打下去跟开盒似的，不打了啥时候放官方wp再议了，我还是很难理解闪电侠的wp的，哈哈哈哈
>

![picture 18](../assets/images/8d9d8affba8fe12463c2bd196e5b1ad2e25fe337046c129dce1b0f8d0b5145a1.png)  

>我看到了明确的密码爆破成功，所以其实上面操作可以不用
>

![picture 19](../assets/images/16dc4777cb4733c02e1c8e54bc6378109a8cb3ef1007e0185017b126858d8b59.png)  

>啊真的假的这个靶机能正常打？
>

![picture 20](../assets/images/840fa06372049c795e473d2bceb9efd028c25e727c1e01add4c7f4685663588e.png)  

>没有密码，算了不挣扎了，不打了
>


>又能继续了
>

```
import requests
import hmac
import time

url = "http://192.168.137.131/wordpress/wp-json/wp/v2/archive/"
timestamp = str(int(time.time()))
nonce = "mahalkita"
secret = "supersecret".encode('utf-8')  # 密钥编码为字节

# 生成 HMAC-SHA256 签名
message = f"{timestamp}{nonce}".encode('utf-8')
signature = hmac.new(secret, message, digestmod='sha256').hexdigest()

headers = {
    "timestamp": timestamp,
    "nonce": nonce,
    "signature": signature
}

response = requests.get(url, headers=headers)
print("HTTP Status Code:", response.status_code)
print("Response Body:", response.text)
```

>这是脚本
>

![picture 21](../assets/images/b328a4e159ef100a783a63fa7713cb57ec31fc5ce3251924e8d3d8d116beb415.png)  
![picture 22](../assets/images/8cc6a86f5a7f1c0be1dd6123ed9f59c357804292e4b0cb779287af1e10bf5b22.png)  

>写一个过滤很丑陋，不过肯定有优雅方案
>

![picture 23](../assets/images/acf5e3875e8f16a349da7f19489703fd7e650626cbda619122646dbe2fb14264.png)  
![picture 24](../assets/images/035c3ce5d801a7f1a2b6f213b2c5788f3af082c637b777351d169f08f3fa91eb.png)  
![picture 25](../assets/images/d005bb203ae79fa733f424ca226f1125aa9bb09334b329ad4499b66a83e6ed27.png) 
![picture 27](../assets/images/f808f8335d603d883f66c167c2bb5d2aa3aac6b52327ecd2d0b56203d7aae6e6.png)  

![picture 26](../assets/images/4551d18e73b5266cd3efeac84013707d8f8c20c4f469636e2fa06dece368b800.png)  

![picture 28](../assets/images/5ad90c793d5ddf84fa79b2ceb40fec28de7d3c993bb38df87654a75b1228eee4.png)  

>后面方案是大佬的比我写的优雅，放出来研究
>

![picture 29](../assets/images/af22f38a2120b065d230b90f593450a8bb6d43c595a024b85f3079457bf5b245.png)  

>上传个sh大概了，这是我目前知道对应smb的操作
>

![picture 30](../assets/images/7503641b45d33415c743e554656463dbee94c8268890be2e5e29b997273a94e0.png)  

>上传个文件能拿到www-data的用户
>

![picture 31](../assets/images/6ab3c7f9d13edbccaf7cd27ad6cbdad664ed44ecc1f347619bace2b518dbb311.png)  
![picture 33](../assets/images/2a2047069d925c99f0e0e700d7bc4e07197f982dc23160e69e01b5522ef2b0c8.png)  

![picture 32](../assets/images/e0047492cfa04b20612680d4ac9f3efae7a5133b9804d5cdcb3741dedd0fc919.png)  
![picture 34](../assets/images/ad73b0a3000aaddf60d432a5827d971e410c20f76f13ad4b0d429967f3b2e61a.png)  

>有点奇怪好牛马,这个格式没调出来，一开始我以为是我电脑问题，重新开端口也不行
>

![picture 35](../assets/images/ca6ebdd6654623f30581aebd3856fbd22dd430daf187a9a5a4d7394e90ba1661.png)  

>我还是没找到对应的wordpress密码
>

## 提权
![picture 36](../assets/images/83c721f441ddcc179a2678ee0636457eecb522ac73667e38551d8223c7847e92.png)  
![picture 37](../assets/images/ee7e56dc948d3dca120ff0b4d0ab787e771966020463a476c046569ff4c0c929.png)  
![picture 38](../assets/images/c3d54b9bdb2d47e24197e0ff32836be31474461c914b1dfb82127fbda74e3000.png)  
![picture 39](../assets/images/89fdf8721f1c47e48bb5926a6236bd652e3ea4d3df40d140ec50c6a3ad4d489a.png)  
![picture 40](../assets/images/3316fb4008af37b468b06018fe5de07585393d4e44ae8784eb5428b4743b47ce.png)  
![picture 41](../assets/images/6b2e08edd10a4e8fff7b5168e71f59ad7a2c58f9969d277184326c83f867151a.png)  

>终于调好终端，不过呢先不直接提权因为现在直接提权没意思了，我看看咋拿wordpress的账户密码
>

![picture 42](../assets/images/e833b3c9cdae4d92b04e0898f34278f631d651e3c700ad42dda3773563451a62.png)  
![picture 43](../assets/images/9757959a0ede1653ec3f06a07696c027d272e6aff10ca95b4895a4807b8ccd59.png)  
![picture 44](../assets/images/416d93bb7b4c85745bc88b0e0c14ee6d0feba77e4bdbba22972b6f095823bdaa.png)  
![picture 45](../assets/images/ea97381be029bba507f0e0bc2ba5092fc0191babb1d7a3e5c2ebbffc7253d83a.png)  

>直接写一个就好了
>

![picture 46](../assets/images/3b7fbdab8a1a76ba568d8bd485daff8924b2abc4fe879260597af65a93fec87a.png)  

>算了就这样了这个靶机其实也就前面的地方有难度
>



>userflag:647cb9f7681a16e4d624292af30ac0cf
>
>rootflag:ece9596c8b1cff09b05ececadc0bc5b4
>