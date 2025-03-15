---
title: VulNyx Zerotrace靶机复盘
author: LingMj
data: 2025-03-15
categories: [VulNyx]
tags: [LFI,lsattr,ethereum]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.93	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.140	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.028 seconds (126.23 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.140
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-15 04:50 EDT
Nmap scan report for zerotrace.mshome.net (192.168.137.140)
Host is up (0.045s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp   open  http    nginx 1.22.1
|_http-server-header: nginx/1.22.1
|_http-title: Massively by HTML5 UP
8000/tcp open  ftp     pyftpdlib 1.5.7
| ftp-syst: 
|   STAT: 
| FTP server status:
|  Connected to: 192.168.137.140:8000
|  Waiting for username.
|  TYPE: ASCII; STRUcture: File; MODE: Stream
|  Data connection closed.
|_End of status.
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
```

## 获取webshell
![picture 8](../assets/images/e4cf674d970afd23fe8a0a5e40ddabf6b669e4708b5e87d3e2abf0ce7098388e.png)  

![picture 9](../assets/images/a41dd22dfd7ff105d379d68e99bab1133c2e901b2dddf5f8dc982847b81d91c2.png)  
![picture 10](../assets/images/6f703ae6c5fb7239a3b32957448a2639a15e99b1bea68b3d6c28945897529202.png)  
![picture 11](../assets/images/08893268c1636e78b65b6d7232369a1743047e01b1467cc8a00c76d407fae8d8.png)  

>到这里我要了一个提示是关于LFI的进程是一个非常重要的东西所以
>

![picture 12](../assets/images/71e018d204b20fff7d1576f0894602d146ff3e116bb0a7a2e15829a98b4bd94e.png)  
![picture 14](../assets/images/0554832085d1ae60a0f9dc4021a30315d22f9d78e15652a4c66440fb290b08ed.png)  

![picture 13](../assets/images/cd55e937e9907ea577bf6b787247321b00937a7f743cfb2e09f67064254029f5.png)  

![picture 15](../assets/images/10b6d43d9f2c631a7a127f44ddbf7a21c0463a59e2b415cff20393070304e881.png)  
![picture 16](../assets/images/6fd25af4f085f51c8dc158dc43638929785c81b4f082d4a805a4df081f0f7911.png)  
![picture 17](../assets/images/0911d367f87d623615617b12a79cc357a1c43e26579a21256a30b95f5d05d49b.png)  
![picture 18](../assets/images/1287363d04af6813c8dddc4c097150c676cdd2db96dd4edb1e37228efe0f2f59.png)  

>可写看看是否存在定时任务
>

![picture 19](../assets/images/997b9b5780f1148693c46d52af7f9cca45af89c7bf3298a69ef3f77c30366b42.png)  
![picture 20](../assets/images/ad9380ef947024c9d5748699a952ebcbfe268891cb175b596730426448360862.png)  
![picture 21](../assets/images/4325f938c30191d833eb7a93d75e025a7af5d669cfcbb1c82ff9dcb86509ecf8.png)  

>这里很明显我们遇到的问题是可以看到权限是可写，但是无法写入，尝试创建文件成功，不启动，我觉得跟什么设计有关，查gtp肯定不如大佬对linux要更透彻，我小问一下给了一个提示符lsattr
>

![picture 22](../assets/images/374c1ca73b1ce7597b281d9c0eb3e9102f4baf96f6d581a32966e860e70d3362.png)  
![picture 23](../assets/images/fc5b3565e41ee56126938842fade13fd3daa0979b1fb0913662180a37b2bdc7a.png)  
![picture 24](../assets/images/49cd3ff839a5b29a45f173c6bc8a0f2858c6bf170c09169468fa6b141ca06d97.png)  

>具有不可修改情况是一个特别意外的
>
![picture 25](../assets/images/7a3392280315facf6f7f3138ab74421a9ea69f2d3c574db43a4779fa1f873523.png)  
![picture 26](../assets/images/5700a9fbc161fdfba2a8c5ccd74e7756d1eb65a87ce321c1c361833c2fbe78a5.png)  

>我觉得挺神奇的，给我对linux加多了一些理解
>
![picture 27](../assets/images/6172f99eb0165c67f2a9af0c57fdccc163501730c50a4a48e8a50d2036837537.png)  
![picture 28](../assets/images/6ccb1ea6e41867c471b6b1a54275a4f54b0e6fed17bed9a5f2007b8d8c213025.png)  

>这里最好写个公钥
>
![picture 29](../assets/images/c43a92bbe2489cc332bba5a594d1b7d9715e8cf0b067cdce34755a3ec91574fc.png)  

>方便之后搁置
>

## 提权

![picture 30](../assets/images/5b748cea3702958220fea7d27150254a858ce0853d73fca78e85c198d682dc83.png)  

>查了图片以为私钥在里面看来不是，只能解密这个secret
>

>来自ll104567大佬提供的爆破形式和提示，主要我查了gtp压根没见着这个形式所以爆破失败
>

![picture 1](../assets/images/00fe1f3b902889b2c3d431bce7bdc6913bf3dfce189896ef21d8d74258ead390.png)  

![picture 0](../assets/images/3b3ff8ee664cb66ef1bab0cc8128a8da301ffab46d1222560db0ec0f23e1eab6.png)  

```
secret2:$ethereum$s*262144*8*1*abb71ccb91d0ec97831d49694bd80ce925c0204772fa6268ace1f73df97e3d71*fee023fd8fcd5b242b0ad4900de2d4614fa4be48887efbd6208a9beb65923df7*4ed5177b17ad85eafafd3dedc40a3c85914d18611c2cca079871a28487055892
```

![picture 4](../assets/images/f357da2e2534b497f2acad88104b3f80035d864b0b6d3b57af02f5da38b5c370.png)  

![picture 3](../assets/images/70cc7b53093cbee1569d28ae2cdbfa1ce98b3fac839ea0b1289f122f83c7ca96.png)  

![picture 2](../assets/images/b16c40023c0bfb38262d81341c4e3ad4b30ebe6a970c79cc03792469b8d3f456.png)  

![picture 5](../assets/images/4fdf8c5ff56faf71499aaa8f361be13362e79c5ec822b33570d56b3b0e6feaa3.png)  

![picture 6](../assets/images/8cc27e252855da2934ba771ac01cb17d85d4c067789ae5f197885c84aea042ba.png)  

>最后王炸了，没啥可说的
>

![picture 7](../assets/images/cff77a148b755df0ba5fc278455702744290c2e482d51c797bc2a95a08642157.png)  

>补充一下预期解不是王炸，需要做的是密码爆破，我之前写过一个脚步忘了放那了现在重新写一份留出来
>

```
import subprocess

all_num = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

prefix = ""

def check_password(passwd):
    
    try:
        result = subprocess.run(["sudo", "/bin/bash", "/home/ll104567/guessme"], input=passwd, text=True, capture_output=True)
        return result.returncode == 0

    except Exception as e:
        print(f"error: {e}")
        return False
    
while True:
    found = False
    for char in all_num:
        attempt = prefix + char
        print(f"\rTrying: {attempt}*",end="")

        if check_password(attempt + "*"):
            prefix += char
            found = True
            break
```

>当他不在输出时前一个就是爆破出的密码
>

![picture 31](../assets/images/483262f675dbd49f3ad13ba14634b5f9d2df3c306db5382c2952076d9d1f533b.png)  

>他不会自动停止脚步，需要手动停但是你能完整看到密码，改进的话再议
>


>userflag:yLFsSkfsLjQQKm49HCkwBtiY60ESXH3s
>
>rootflag:0IB3gKtQ82ZBpyvwDo1Gp55snCElXC7U
>