---
title: VulnVM Search靶机复盘
author: LingMj
data: 2025-04-06
categories: [VulnVM]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.131	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.037 seconds (125.68 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.131
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-05 23:14 EDT
Nmap scan report for debian.mshome.net (192.168.137.131)
Host is up (0.038s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 39:0d:70:e0:55:cb:20:de:ad:f7:10:d8:1f:76:4d:9d (ECDSA)
|_  256 df:e2:94:52:e9:3d:eb:69:2d:b4:a5:a9:2c:3e:63:46 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## 获取webshell
![picture 0](../assets/images/47705665ea5a2b09a70b9bf7f452f28b5b67f1a51b5fa08b59a904ecf6954923.png)  
![picture 1](../assets/images/867e6d7ad98add27df69c1c7ca56136636a8e3006c516456a38505c58cc5f131.png)  

>没啥好说的
>


## 提权

![picture 2](../assets/images/0640fba8c42c7929b336febb9b2d26444f4390f60388a754177d374c0ba076a5.png)  
![picture 3](../assets/images/d78be3b22e319d38a6365e6454ae1d0805c826abe17bfcdee59999ffc0c6de84.png)  

>一个下载的东西，算了直接看看程序
>

![picture 6](../assets/images/252e63bd2d1c5d278e3c3227ec054cfd3dadd3931251b93b57c8707b5386621a.png)  

![picture 5](../assets/images/7d636f2a9990abbc5f750ab1aba1cb7c8de5ecc861c754b1f6c1e9c053d25144.png)  

![picture 4](../assets/images/2c2ccc847ccb635d229cf79dd515b5100588749044cdd5034f68ecb40558825e.png)  

>构造恶意的deb包
>

![picture 7](../assets/images/d35a4e32a536eeb7aa1bca8ffd1cd1a1322a92ef07b7effdcdaa598bff0262f0.png)  

>算了头疼搁置了反正就这个思路构建不成功，主要是我好像没打过构建deb的靶机差不到具体流程
>

![picture 11](../assets/images/8e0b8e4c88b10f03835e6c7c30a857eca87a87de6e6d874e6f98f9eec2ad1e33.png)  



![picture 8](../assets/images/ce48fdaece73b8160cc47c00d78821761ccfcc46d8142b361cafc4e8631c6ec2.png)  

>需要https端口，具体操作找gtp
>

```
import http.server
import ssl

# 配置地址、端口、证书路径
bind_address = '0.0.0.0'
port = 443
certfile = 'cert.pem'
keyfile = 'key.pem'

# 创建 HTTP 服务器
httpd = http.server.HTTPServer((bind_address, port), http.server.SimpleHTTPRequestHandler)

# 创建 SSL 上下文并加载证书
context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain(certfile=certfile, keyfile=keyfile)

# 用 SSL 上下文包装 socket
httpd.socket = context.wrap_socket(httpd.socket, server_side=True)

print(f"Serving HTTPS on {bind_address} port {port}...")
httpd.serve_forever()
```

![picture 10](../assets/images/d82d740d64d9a4849c25dd2186dbb3184c55a3c168f8d4cdfab90031c0396b12.png)  


![picture 9](../assets/images/52da404d44e224a02d819226bfa9b9f742c1cb39ad236a687d32cc697db7ea3c.png)  

>来自大佬提示完成
>

>userflag:0c289d650057a4b2399192d6c3386226
>
>rootflag:42b8499e0709ef45c5e9ede616271e53
>