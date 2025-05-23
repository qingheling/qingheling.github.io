---
title: hackmyvm Keys靶机复盘
author: LingMj
data: 2025-05-23
categories: [hackmyvm]
tags: [gpg]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.34	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.267 seconds (112.92 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-23 00:08 EDT
Nmap scan report for keys.mshome.net (192.168.137.34)
Host is up (0.0076s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   3072 6e:b1:d1:09:f5:dc:01:29:ed:9d:4f:8e:a7:7a:a0:a6 (RSA)
|   256 35:f4:29:df:64:6a:be:7f:9f:0a:9f:ee:07:e4:19:07 (ECDSA)
|_  256 4e:0f:f7:32:cc:c7:91:57:07:d9:50:0a:38:c9:e5:11 (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-server-header: nginx/1.18.0
|_http-title: The World of Keys
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.54 seconds
```

## 获取webshell

![picture 0](../assets/images/f8cd930140298a805518d0daf32cdedfb00a0210541fa8ffe5e7708244568e45.png)  
![picture 1](../assets/images/40301cdd495687e7b373b9e8dd856f2827d337cd78915e16bc50c53d831be0e7.png)  
![picture 2](../assets/images/4ba6fab9242359efad5a5dc950d5c2d354ab9dbeee9568558511c0e29ea620ee.png)  

>好了查看礼物先
>

![picture 3](../assets/images/a3e2717078dc69d3ed87ed6b980f96349299477ef976558dc2bcd786cf58406f.png)  
![picture 4](../assets/images/c07209690c43950bd1dc95c6a5331e3478bb6217aa75f6c3e1e4fb4afebb41ef.png)  

>给一个字典
>

![picture 5](../assets/images/dcfbed505bf6de2027bf70ef559e107c70936ce0b1d35df7bdfc04f763ac1ced.png)  

>有点多
>

![picture 6](../assets/images/359792a3e9e330051de50a925e2904fe088cd07f0d8941c40d1df94d5ba3990a.png)  

>感觉像一个故事，但是太多了翻译都很麻烦
>

![picture 7](../assets/images/2ecc6e56f2f1f1fa44f3c3d0ef9c34fb849f4bdb41ac2790f5c9f21f2701941f.png)  
![picture 8](../assets/images/ce811f94e6b762557c6a69576c76cd287044bb3873b0ec3cd8de28971020dfaf.png)  
![picture 9](../assets/images/88e131de23a3f580890ed3d8689bc749754ee8927ba308c7cb45174fff7439df.png)  
![picture 10](../assets/images/c8dbd86242f5a91d79d7e788dde23dc77cb6cce4c25423569670b50f71debe33.png)  
![picture 11](../assets/images/88bc86f2c54d0078cb59f82a279f1134594285f643cc24b66d93d40b2d20f6d5.png)  

>有东西了
>

![picture 12](../assets/images/47160266840f9f9b53dd145e37d0ce567c8a73e990f26f7ee51f123d846f7f58.png)  
![picture 13](../assets/images/a2127ef58f8b4997b1b45b136d903799889f60b022b197b53ef27a48fa7e1c99.png)  
![picture 14](../assets/images/6816d69d3d10953891e0ed89dd8582d47b95430093d5d3fe7e2cee09cc68791c.png)  
![picture 15](../assets/images/f272e53d9297f0d4c187e17415c9af0e9fed05a13024fe2d1cb903f88aaf58a4.png)  

>好了
>

## 提权

![picture 16](../assets/images/8bdf4397f1be2b1b5227abf8ef264d748a24932bd5569843e5a548c5c91f49cf.png)  
![picture 17](../assets/images/8b92fba12089dd86fe1568f3e81e7218704dafc3b3189dec25580e016a1dd4b7.png)  
![picture 18](../assets/images/4ad41df3e1f0065d692fc853ec41b7972e11168e58131bc5ce0dbf371d121976.png)  
![picture 19](../assets/images/51942f88afb16a9d310ed40ab1ecd809cfc2b97992448aef4829eace31d679e6.png)  
![picture 20](../assets/images/9c90fc2306560ab33ac753189dd7c26016f5cd4d0ead476fcc03a0368c86fdf2.png)  

>序号是这个4695
>

>里面压缩包都是私钥
>

![picture 21](../assets/images/d5dcd23fa8eb8322b80083535b377eee3a9365a5c9c5ad38f97a620befceef1c.png)  
![picture 22](../assets/images/3bcf042afcec0b24bff9305da4c57a5002a6153e8326be4f605e6c5c35173123.png)  
![picture 23](../assets/images/077259d23f26b7b228f70bc262c0ed49f3801fcd7dcc9a5c5b1c6ca820752abc.png)  

>只有steve是不知道可以尝试私钥登录的
>

![picture 24](../assets/images/cb6d4fdc258ccaddae1838b240df4ef4e253fae6c8cbc8f3d8e65cce3eda34b2.png)  

>没找到私钥么
>

![picture 25](../assets/images/e1d07e507313617b211b3d472970dd5b3d4ab0e836b7e732135f89a7f8ad7261.png)  

>刚刚目录不对，不断按空格就行了
>

![picture 26](../assets/images/960924e8dd049fde9ebbb992b2a1f7f8eb7e7e2c87e4f2eb957c7095a239a373.png)  
![picture 27](../assets/images/e46f7491473295a2bb3643725a729a5fa3fd8a7cf6efe542df7286043aa72221.png)  
![picture 28](../assets/images/c53c014382e7a5bb52602b8f154d5377db9e5a9361568817d8bb5cde3abe7754.png)  
![picture 29](../assets/images/abe9a32e4d59b64edde39fb349d2e49e7b1d632ee46839c20945fb3ec31780b9.png)  

>这里是私钥的提示啊，那没啥用
>

![picture 30](../assets/images/46d80bd7a76e902f505bc3922bbf4dc85ca5a0e15c23653c01e63d3f3377288e.png)  
![picture 31](../assets/images/817ec57f5c8fbc5f20eaffad245dd297ffd1e886c405ad7be8d68d07d648838b.png) 
![picture 33](../assets/images/94719f35a95fa8cc5291dccd054239ad0977b4ba7ec47bf8228d670d3c02b2fd.png)  
 
![picture 32](../assets/images/d58729a1c5c9dd3e4878bb7cc39b0576270f912a7e407a081374a642f077e15e.png)  

>那就是有密码
>

![picture 34](../assets/images/2e012db1dd7bf8899f81fa52a710f430315fbbc85e2c1e4e8a363db56dad314a.png)  
![picture 35](../assets/images/7377ee86ccabf97db64653db96bb76d7677384505a46dd6247090e1b7bea7a9a.png)  

>然后是什么
>

![picture 36](../assets/images/806661b7a3cac4bbcf229b54a2a8a490f2fbf6bd364ca5905a7fb7901bd6902e.png)  

>看wp是什么内容应该怎么利用
>

![picture 37](../assets/images/92e3ed20d4e7044f5de611b89339fe0e5bee9718cb1f58eee8496ee413c94db0.png)  
![picture 38](../assets/images/ccac20b1fa9b3b019eadf6dc6ab98975a6bb52e3fcce3ef73b7b6d0536aa7b42.png)  

>结束
>

>userflag:4vJkfrYnYT7Q6PwVDll6
>
>rootflag:AeQgWYpsNcuL4BzXH2p1
>
