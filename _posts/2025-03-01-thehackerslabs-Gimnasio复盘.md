---
title: thehackerslabs Gimnasio靶机复盘
author: LingMj
data: 2025-03-01
categories: [thehackerslabs]
tags: [xxe,sqlmap,hash,sh,perl]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.9	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.92	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.253	a0:78:17:62:e5:0a	Apple, Inc.

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.047 seconds (125.06 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.9 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-01 07:44 EST
Nmap scan report for neogym.mshome.net (192.168.137.9)
Host is up (0.028s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
| ssh-hostkey: 
|   256 b3:83:ba:3f:2b:09:07:47:c4:30:37:d8:d2:66:bc:d7 (ECDSA)
|_  256 01:77:26:20:16:a2:f6:5e:4d:22:4f:cc:ab:dd:ae:b0 (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Neogym
|_http-server-header: Apache/2.4.62 (Debian)
3000/tcp open  http    Golang net/http server
|_http-title: Gitea: Git with a cup of tea
| fingerprint-strings: 
|   GenericLines, Help, RTSPRequest: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain; charset=utf-8
|     Connection: close
|     Request
|   GetRequest: 
|     HTTP/1.0 200 OK
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Content-Type: text/html; charset=utf-8
|     Set-Cookie: i_like_gitea=7b09539e3136df63; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=TQHxBXfdHO0F0Ca5gA_vsUfavvQ6MTc0MDgzMzEyMTg4MzU4MTAzNQ; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Sat, 01 Mar 2025 12:45:21 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" data-theme="gitea-auto">
|     <head>
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <title>Gitea: Git with a cup of tea</title>
|     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR2l0ZWE6IEdpdCB3aXRoIGEgY3VwIG9mIHRlYSIsInNob3J0X25hbWUiOiJHaXRlYTogR2l0IHdpdGggYSBjdXAgb2YgdGVhIiwic3RhcnRfdXJsIjoiaHR0cDovL25lb2d5bS50aGw6MzAwMC8iLCJpY29ucyI6W3sic3JjIjoiaHR0cDovL25lb2d5bS50aGw6MzAwMC9hc3NldHMvaW1nL2xvZ28ucG5nIiwidHlwZSI6ImltYWdlL3BuZyIsInNpem
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Allow: HEAD
|     Allow: GET
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Set-Cookie: i_like_gitea=d3344e1ac4e6cc0a; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=LAX3wVADPcj2Fb_5gix_fhMvKFs6MTc0MDgzMzEyMTkyNzQ3NTU5NA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Sat, 01 Mar 2025 12:45:21 GMT
|_    Content-Length: 0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.95%I=7%D=3/1%Time=67C30161%P=aarch64-unknown-linux-gnu
SF:%r(GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:
SF:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20
SF:Bad\x20Request")%r(GetRequest,1B50,"HTTP/1\.0\x20200\x20OK\r\nCache-Con
SF:trol:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\n
SF:Content-Type:\x20text/html;\x20charset=utf-8\r\nSet-Cookie:\x20i_like_g
SF:itea=7b09539e3136df63;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSet-C
SF:ookie:\x20_csrf=TQHxBXfdHO0F0Ca5gA_vsUfavvQ6MTc0MDgzMzEyMTg4MzU4MTAzNQ;
SF:\x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Frame-O
SF:ptions:\x20SAMEORIGIN\r\nDate:\x20Sat,\x2001\x20Mar\x202025\x2012:45:21
SF:\x20GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20data-them
SF:e=\"gitea-auto\">\n<head>\n\t<meta\x20name=\"viewport\"\x20content=\"wi
SF:dth=device-width,\x20initial-scale=1\">\n\t<title>Gitea:\x20Git\x20with
SF:\x20a\x20cup\x20of\x20tea</title>\n\t<link\x20rel=\"manifest\"\x20href=
SF:\"data:application/json;base64,eyJuYW1lIjoiR2l0ZWE6IEdpdCB3aXRoIGEgY3Vw
SF:IG9mIHRlYSIsInNob3J0X25hbWUiOiJHaXRlYTogR2l0IHdpdGggYSBjdXAgb2YgdGVhIiw
SF:ic3RhcnRfdXJsIjoiaHR0cDovL25lb2d5bS50aGw6MzAwMC8iLCJpY29ucyI6W3sic3JjIj
SF:oiaHR0cDovL25lb2d5bS50aGw6MzAwMC9hc3NldHMvaW1nL2xvZ28ucG5nIiwidHlwZSI6I
SF:mltYWdlL3BuZyIsInNpem")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r
SF:\nContent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close
SF:\r\n\r\n400\x20Bad\x20Request")%r(HTTPOptions,197,"HTTP/1\.0\x20405\x20
SF:Method\x20Not\x20Allowed\r\nAllow:\x20HEAD\r\nAllow:\x20GET\r\nCache-Co
SF:ntrol:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\
SF:nSet-Cookie:\x20i_like_gitea=d3344e1ac4e6cc0a;\x20Path=/;\x20HttpOnly;\
SF:x20SameSite=Lax\r\nSet-Cookie:\x20_csrf=LAX3wVADPcj2Fb_5gix_fhMvKFs6MTc
SF:0MDgzMzEyMTkyNzQ3NTU5NA;\x20Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20S
SF:ameSite=Lax\r\nX-Frame-Options:\x20SAMEORIGIN\r\nDate:\x20Sat,\x2001\x2
SF:0Mar\x202025\x2012:45:21\x20GMT\r\nContent-Length:\x200\r\n\r\n")%r(RTS
SF:PRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x20tex
SF:t/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad\x20
SF:Request");
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.00 seconds
```

## 获取webshell

![picture 0](../assets/images/b5cd1ebf0c2754826159e54cb1b08acdbca5b90a83eabef999813d333e69fea0.png)  
![picture 1](../assets/images/d7fd473db816ef7183ec57327d977a635b66515f6d99160ad5724d887c02ede9.png)  
![picture 2](../assets/images/cb7d8847cc0bf6c19ea72c8aabcd470f410bfc4923f660d5f7710d1d1dc575cb.png)  

>看见这个消息不是xss就是xxe
>

![picture 3](../assets/images/e51b0bb6690aec3d160b24b40a4f0ee4e032a7384967ed7f44dea7adbe5595e4.png)  

>看来xxe，直接上poc：https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection
>

![picture 4](../assets/images/03bff0b11abecc07c42e000257bac3b893f8fe76a55a804b5476dfc5b58a4895.png)  
![picture 5](../assets/images/af677e7004347456d89826f963b529eae97212c9dfc94069cf998ea2dcf7e087.png)  
![picture 6](../assets/images/1ea2ce46a922848401a8fc9b603c007317258bb075ea9ae9df0d2c6499a932ff.png)  

>有报错证明里面有东西
>

![picture 7](../assets/images/b2b3f34730a1229d3c0d18757c987301419f09abc6d195db757705f3c31bee0f.png)  
![picture 8](../assets/images/600ba054f46437ccb806afe32430af66c9d6d67aff84d0e7c689d1885538cef2.png)  

>这里有账号密码但是不是ssh的我们去找子域名
>

![picture 9](../assets/images/7acd5d4b851cba0dce49d8ba23d540804764087c77d426fa92a6bdb34fb36762.png)  

![picture 10](../assets/images/e8c877fb406c45d253bc08bc3cdab5c5344fc2402124de5b0b5c26cfeea9ba86.png)  

![picture 11](../assets/images/a787451090cd9848be27ff93797d63b6dc4b62df13e4c2200370982fc44fbdc1.png)  

>好了登录进来了，可以继续进行操作，这里有一个提交框但是必须得输入正确不能乱输
>

![picture 12](../assets/images/fd64f13f2e8b142d5999fcf02dfc234190abbb40330d67232ddf4e29b163b60c.png)  
![picture 13](../assets/images/eaf2f6f32a26195f126828b078498ad3f3f9b262a4eb95d39e05ce53ea49d4ac.png)  
![picture 14](../assets/images/15da9b6d16a60838ce19442bdca42b769161f8d26d0897720001bf806a55c0fa.png)  

![picture 15](../assets/images/cf379550f1a13638c9bff8dc6e5122358aa2000753531033cf73fc500d70d31a.png)  
![picture 16](../assets/images/59f0af3c6147eeb294832dbb7da04e002b17c2d9814315148ded9c10dcd40a78.png)  
![picture 17](../assets/images/3c41390680787a13636f4c68ca691137c400f4b3b3be34beb23f11b30218b88d.png)  


>拿到账号密码了
>

## 提权

![picture 19](../assets/images/f70610cdf7cf9d8b990371a5eb5f54ff965347b4ae79f4000d6ae1e6c44cbc7d.png)  
![picture 21](../assets/images/d2c1415c306d9b65eeb1645cbdceb1c822a9588be820f3cfe80d3291fe9269df.png)  

![picture 20](../assets/images/ab0fd9c7b243fb525a1dde4d5a29d988d1f5005a0a89e5f0df4c768c4342daff.png)  

![picture 22](../assets/images/b0c7ca5d7778e58df917a6d0c3e0390eecc4992ff33eee59d576cde16a7aa090.png)  


>这里python服务像docker一样，这里如果你很强可以直接盲注入获取shell，但是我当时没有怎么干我去获取了gitea和源码，我改了gitea数据密码
>

```
kyle@neogym:~$ sudo /usr/bin/python3 /opt/scripts/systemcheck.py --help


Ayuda:

container-inspect: Inspecciona un cierto contenedor de Docker
container-images:  Lista las imagenes
container-ps:      Lista los contenedores que estan en ejecución
full-check:        Realiza un chequeo del sistema

Ejemplo de uso:

/usr/bin/python3 /opt/scripts/system-checkup.py container-ps
/usr/bin/python3 /opt/scripts/system-checkup.py container-images
/usr/bin/python3 /opt/scripts/system-checkup.py container-inspect <container_name>
/usr/bin/python3 /opt/scripts/system-checkup.py full-check


```

![picture 24](../assets/images/62ef9c1573989eeda888dde742594804b32306ec7c19071b94f64c8fb903562b.png)  

![picture 23](../assets/images/a30589477ab7d3b7c21e5629f452161e551d551cbba2e0fc49dfebd7f3a6937a.png)  

>这里利用fscan完成端口扫描
>

![picture 25](../assets/images/264e7d21cb0eed9b4d2d3db51703aa20ac1af0851d8ce4cadb66817cca465f9b.png)  
![picture 26](../assets/images/2b721a1fad22589b74da30de5e98fdbbccde5a97ae5a2cc1473bc182e650b55c.png)  

>好了利用mysql进行账号登录
>

![picture 27](../assets/images/f0d901a4559951847e0ee1c6e71a02e88159b2f10fbcbf50c18e516a1912d35f.png)  

![picture 28](../assets/images/0ac5ac66828faf9a2f008482f773266357bd6c37245a9f3313821fbdad14232b.png)  

>这里改mysql密码方案很多我选择python设计登录填充方案，你们也可以看ll104567大佬的更改加密形式
>

```
import hashlib
import binascii
from hashlib import pbkdf2_hmac

# 参数
password = "123456"  # 需要加密的密码
salt_hex = "f439c46d381ae6790b9e6ce0a44101d5"  # 给定的盐值
iterations = 50000  # 迭代次数
hash_length = 50  # 哈希值长度

# 将十六进制盐值转换为字节
salt = binascii.unhexlify(salt_hex)

# 生成 PBKDF2 哈希值
hash_value = pbkdf2_hmac('sha256', password.encode('utf-8'), salt, iterations, hash_length)

# 转换为十六进制字符串
hash_hex = binascii.hexlify(hash_value).decode('utf-8')

# 输出完整的 PBKDF2 哈希格式
pbkdf2_hash = f"pbkdf2$sha256${iterations}${hash_length}${salt_hex}${hash_hex}"

print("完整的 PBKDF2 哈希值：")
print(pbkdf2_hash)
```

![picture 29](../assets/images/c7d8e67baa8f796d5dcbca6051d33f29c1c9b17b64d24f2daa1312db292d926e.png)  
![picture 30](../assets/images/de7ccb78900f7389d63c497764b5bafac9ad8f206c7cb3fe70132418dabf96fa.png)  
![picture 31](../assets/images/524efb1c4e53c1a982a13d755ef29b228e9a2f4880c0d53a4badb8648db8c581.png)  
![picture 32](../assets/images/da528a43ce64cc7601c3b929a30f6c046327435b41bad655896c1694f2f026b1.png)  
![picture 33](../assets/images/32c41a4b3f0fc79801bffb8919fa2d23c42f27f0b8804f482a9eb3c3a387cf69.png)  

>这里是注入点它会直接传递进去
>

![picture 34](../assets/images/73cd809bacda7f2240fc051582e08bc930ae0327e3902e6db7fe534e277630f6.png)  
![picture 35](../assets/images/c664f508d6f343981a8d0f2716625bb6d686eab08db6d1dc993eb4e467262ed0.png)  

>结束这个靶场我已经打了4遍了已经滚瓜烂熟了这些知识点
>

>userflag:9551bfd5501e3cb80b264056b0f66986
>
>rootflag:89db0121a107d46b719f52eac2f03567
>