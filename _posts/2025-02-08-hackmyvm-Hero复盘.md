---
title: hackmyvm Hero靶机复盘
author: LingMj
data: 2025-02-08
categories: [hackmyvm]
tags: [n8n,banner,chisel]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:20:f0:a0       (Unknown)
192.168.56.144  08:00:27:c9:9b:73       (Unknown)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.942 seconds (131.82 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.144
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-07 19:53 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.144
Host is up (0.0030s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
80/tcp   open  http    nginx
|_http-title: Site doesn't have a title (text/html).
5678/tcp open  rrac?
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Accept-Ranges: bytes
|     Cache-Control: public, max-age=86400
|     Last-Modified: Sat, 08 Feb 2025 00:46:25 GMT
|     ETag: W/"7b7-194e305a674"
|     Content-Type: text/html; charset=UTF-8
|     Content-Length: 1975
|     Vary: Accept-Encoding
|     Date: Sat, 08 Feb 2025 00:54:55 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <script type="module" crossorigin src="/assets/polyfills-DfOJfMlf.js"></script>
|     <meta charset="utf-8" />
|     <meta http-equiv="X-UA-Compatible" content="IE=edge" />
|     <meta name="viewport" content="width=device-width,initial-scale=1.0" />
|     <link rel="icon" href="/favicon.ico" />
|     <style>@media (prefers-color-scheme: dark) { body { background-color: rgb(45, 46, 46) } }</style>
|     <script type="text/javascript">
|     window.BASE_PATH = '/';
|     window.REST_ENDPOINT = 'rest';
|     </script>
|     <script src="/rest/sentry.js"></script>
|     <script>!function(t,e){var o,n,
|   HTTPOptions, RTSPRequest: 
|     HTTP/1.1 404 Not Found
|     Content-Security-Policy: default-src 'none'
|     X-Content-Type-Options: nosniff
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 143
|     Vary: Accept-Encoding
|     Date: Sat, 08 Feb 2025 00:54:56 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error</title>
|     </head>
|     <body>
|     <pre>Cannot OPTIONS /</pre>
|     </body>
|_    </html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port5678-TCP:V=7.94SVN%I=7%D=2/7%Time=67A6AB64%P=x86_64-pc-linux-gnu%r(
SF:GetRequest,8DC,"HTTP/1\.1\x20200\x20OK\r\nAccept-Ranges:\x20bytes\r\nCa
SF:che-Control:\x20public,\x20max-age=86400\r\nLast-Modified:\x20Sat,\x200
SF:8\x20Feb\x202025\x2000:46:25\x20GMT\r\nETag:\x20W/\"7b7-194e305a674\"\r
SF:\nContent-Type:\x20text/html;\x20charset=UTF-8\r\nContent-Length:\x2019
SF:75\r\nVary:\x20Accept-Encoding\r\nDate:\x20Sat,\x2008\x20Feb\x202025\x2
SF:000:54:55\x20GMT\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<ht
SF:ml\x20lang=\"en\">\n\t<head>\n\t\t<script\x20type=\"module\"\x20crossor
SF:igin\x20src=\"/assets/polyfills-DfOJfMlf\.js\"></script>\n\n\t\t<meta\x
SF:20charset=\"utf-8\"\x20/>\n\t\t<meta\x20http-equiv=\"X-UA-Compatible\"\
SF:x20content=\"IE=edge\"\x20/>\n\t\t<meta\x20name=\"viewport\"\x20content
SF:=\"width=device-width,initial-scale=1\.0\"\x20/>\n\t\t<link\x20rel=\"ic
SF:on\"\x20href=\"/favicon\.ico\"\x20/>\n\t\t<style>@media\x20\(prefers-co
SF:lor-scheme:\x20dark\)\x20{\x20body\x20{\x20background-color:\x20rgb\(45
SF:,\x2046,\x2046\)\x20}\x20}</style>\n\t\t<script\x20type=\"text/javascri
SF:pt\">\n\t\t\twindow\.BASE_PATH\x20=\x20'/';\n\t\t\twindow\.REST_ENDPOIN
SF:T\x20=\x20'rest';\n\t\t</script>\n\t\t<script\x20src=\"/rest/sentry\.js
SF:\"></script>\n\t\t<script>!function\(t,e\){var\x20o,n,")%r(HTTPOptions,
SF:183,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Security-Policy:\x20de
SF:fault-src\x20'none'\r\nX-Content-Type-Options:\x20nosniff\r\nContent-Ty
SF:pe:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x20143\r\nVary:\x
SF:20Accept-Encoding\r\nDate:\x20Sat,\x2008\x20Feb\x202025\x2000:54:56\x20
SF:GMT\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"
SF:en\">\n<head>\n<meta\x20charset=\"utf-8\">\n<title>Error</title>\n</hea
SF:d>\n<body>\n<pre>Cannot\x20OPTIONS\x20/</pre>\n</body>\n</html>\n")%r(R
SF:TSPRequest,183,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Security-Po
SF:licy:\x20default-src\x20'none'\r\nX-Content-Type-Options:\x20nosniff\r\
SF:nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:\x20143
SF:\r\nVary:\x20Accept-Encoding\r\nDate:\x20Sat,\x2008\x20Feb\x202025\x200
SF:0:54:56\x20GMT\r\nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html
SF:\x20lang=\"en\">\n<head>\n<meta\x20charset=\"utf-8\">\n<title>Error</ti
SF:tle>\n</head>\n<body>\n<pre>Cannot\x20OPTIONS\x20/</pre>\n</body>\n</ht
SF:ml>\n");
MAC Address: 08:00:27:C9:9B:73 (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 89.29 seconds
```

## 获取webshell
![图 2](../assets/images/bb129c7c3ca3d11fa8bef285ecb866c62d2f30fecec8e2bb7607f9fe4bdbd3fa.png)  

![图 0](../assets/images/c9ca93531cf2deb83efd7eb94116c14aba4657faa0c26bac810ac53feae82c9e.png)  

![图 3](../assets/images/77ff632fb09966d884188b0b5d12e10cb47f1a885e190481bc8978166d5c7cbd.png)  
![图 4](../assets/images/cf95ea231bec2811e80cfbf71c0978bbe1a1d5c8da03ddb0303c04ceb2aa1d37.png)  

>随便填
>
![图 5](../assets/images/1da28cd58aba6a97794a55f93f7a87751ce0f91358a6f835e1e012eaba78398b.png)  
![图 6](../assets/images/c0e33d63bcb63faad3c8c9c2aaf19e4a65a8e1fda5d33de5a373c712407658bb.png)  
![图 7](../assets/images/e71eac19155125fc81ca8b3543155b75cf4c7e4d0af59624da4fac85f5018f82.png)  
![图 8](../assets/images/711aff12fb428ad7474578290aff3fd9f4cb7842a0166833c785161b62d08875.png)  
![图 9](../assets/images/dfed9e78c68abdcaec59cf66c47a8f10a3d2e7051c5d8a3464e4681b4795a15c.png)  
![图 10](../assets/images/d8a764fc0b36e8bb8b0f633016384ebf291bf34b46991d35a907cfeae041b055.png)  
![图 11](../assets/images/e25b05d795d9a7c383ad1e7798da88fa7d805ef976d2525505510e5f692c3504.png)  
![图 12](../assets/images/3aa618c9c14a188c60fcf7b73065a8249c9acd7e5d28fc953c0e0e0933fe9ba1.png)  
![图 13](../assets/images/647cc43ed013fe0ebd714882e90ca130782101606a47cc482142d2203e32374a.png)  

>只有busybox
>
![图 14](../assets/images/da3ab4adc53def1d1db0626ecf150b751bdc9c1593997e67d20c88600350b98a.png)  
![图 15](../assets/images/27495982c794e13fd4e9d68690dcebef4e138fa829d236c604ca59e7e6e224b3.png)  

>没有script 我在思考如何稳定这个shell，因为直接利用socat和chisel转发出来会没有东西
>
![图 16](../assets/images/d5fd3332cd7420d1d04362446e468e4e4d25e4fa9b01d1df7cb214ab6f3a7094.png)  
![图 17](../assets/images/ce5f9ef706d88bab2ce9362a958feedf1c7f6b2af0d21bb11bc59e85109133dd.png)  
![图 18](../assets/images/e4eb0f660bc8aea15f20a07a4c176b291b7b0b8e71b89b565cf9d731e770a019.png)  

>可以看到我的socat没成功，主要是因为终端不稳定
>
![图 19](../assets/images/fb370edf8891803c575a5135347465887111564d93a6e185f83c5e8ffc21b17f.png)  
![图 20](../assets/images/ee1c2e66dca0313c50446698090609ca0072ece08f596a74ac13e248ce43106c.png)  

>这里看到我的chisel打洞回来了
>

```
./chisel server -p 2022 --reverse
./chisel client 192.168.56.110:2022 R:2222:172.17.0.1:22 &
```

![图 21](../assets/images/11686d0d4d829694fe3ea9bedb1e728d85a522baf091a4edf1ef39d4fbd35b01.png)  
![图 22](../assets/images/828901561ec14aa5f700fc72204058f6718cc668184f8359b65e510d125c72fb.png)




![图 23](../assets/images/9f5f82771d6da7eb0218a5d7edb89ac7b8e1a1b5831fa8cfa2c91d87706e5e72.png)  

>想半天了，发现是旧密钥条问题，没有重新加载到id
>




## 提权
![图 24](../assets/images/9d58077e328ca9f517b7a517d0d6c136237077e7a609e2401cfe5fce76271441.png)  

>这个用和有uid的权限
>

![图 25](../assets/images/6894434e5b01e69f48d29ad10e0f68e2dcbee55d3e8a24ff6c36d6439a079f27.png)  
![图 26](../assets/images/82af24b9a0356615ea7b36105e028d29e13093d2e16c7a9347bac071ce2b8cde.png)  
![图 27](../assets/images/004e4b83bad7c49901a10df1a363c1464a488418bc521419a7cced8ed0809205.png)  
![图 28](../assets/images/61a0e3f988eba6fff0ec653c9443df8fc6755fa42ef5f0d313713a34a8938d4e.png)  
![图 29](../assets/images/a1b304d4c66b71304e2d9a53f2a64010ec7e85a0bb9b7378b92307adb52a7e00.png)  

>看来是sshd的问题，利用一下就吧ssh带出来了，之前的靶机就有这个好像是jan吧
>
![图 30](../assets/images/1bebee95ca29533cf1219ede7f385f6da8481682016c3ac1085df9e24ff4e8ac.png)  
![图 31](../assets/images/6e2ae04544855cd6b573d0a7caaafc41fa951d5d2fd88a1bc2d72bf1ba4ed6c6.png)  
![图 32](../assets/images/23e49798ab0c65394512ee8087f6d267fc1e3267c74b71007b2dd0ab872edbda.png)  
![图 33](../assets/images/302037c415edacf5f0e05277f4b943117819d797cdff64026aead0887ea51d39.png)  
![图 34](../assets/images/7d7125851229f2d064102475ada2cedee9ed13eb936262f638b030735ecb7d52.png)  

>当然这里可以写一个脚本去做,更panghu的靶机一个原理
>
![图 35](../assets/images/26ed7dbf23b76cc93d03c89535314dfa206a2386db134114945ca5b9f774510f.png)  

>试一下拿shell，不行就算了
>
![图 36](../assets/images/4f7c12dca96224634db9ee7d413d9f6018c257c3e2d67dd1022cfe07f74a5759.png)  

>ok 结束 root密码：Imthepassthaty0uwant!
>

>userflag:HMVOHIMNOTREAL
>
>rootflag:HMVNOTINPRODLOL
>