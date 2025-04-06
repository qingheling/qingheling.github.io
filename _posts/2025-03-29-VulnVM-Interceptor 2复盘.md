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



## 提权



>userflag:
>
>rootflag:
>