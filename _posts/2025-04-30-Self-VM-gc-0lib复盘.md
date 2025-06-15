---
title: Self-VM gc-0lib靶机复盘
author: LingMj
data: 2025-04-30
categories: [Self-VM]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.83	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.033 seconds (125.92 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-29 21:47 EDT
Nmap scan report for gc.mshome.net (192.168.137.83)
Host is up (0.091s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
8080/tcp open  http    Apache httpd 2.4.57 ((Debian))
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache/2.4.57 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.19 seconds
```

## 获取webshell
![picture 0](../assets/images/3c591c14123a48166aaaf95ac6772ed5f706f11f2efe81095888ac9078a11bbc.png)  
![picture 1](../assets/images/663eedb7b3f7eb764fb39a1c69c85538fc339ab7da132abd334c207d972b4b3e.png)  
![picture 2](../assets/images/9c7485d1a36e2ba6cab24f183e71c3f60eecfaae8a2da417f37718471f91a770.png)  

>是存在文件包含的，但是当时我就做不出原因没走cve，刚看一眼cve进行操作
>

![picture 3](../assets/images/43e646295d7b0de1e8a94b800b5b2a933f21cff38296a923e6b7e48f790435d7.png)  
![picture 4](../assets/images/2d3782bfbdd8d145d619ead76da29e62ab34b92e98447685fed90012b1ea224a.png)  
![picture 5](../assets/images/2a52b04f762f566fd5bf6f3af28c78b178d41faaaf2a924357e18e8d9a59d21c.png)  
![picture 6](../assets/images/b2f09b407c85da56c56dd7ba48bd01f3025845512b56dfbc698964ccaf208c6d.png)  

>报错了，主要原因是get和post问题
>
![picture 7](../assets/images/a830f4d1cb88862844a55471e5050c29c382cf88e163ef89a72817f0b0e316dd.png)  
![picture 8](../assets/images/2525d55cf7e9294d36b9b5f16d31b57a4318e7db0e50fd68e35091de256536fc.png)  
![picture 9](../assets/images/5200bd5243a4994c89198ec4ee539eddc6383bd1803d7211b9fa75888be723ba.png)  
![picture 10](../assets/images/1f040d5e80b63d68da14f0884548d080b291b650bc8928376433c158bf728766.png)  

>除了这个啥也没有先提权了
>

![picture 11](../assets/images/991b407688b0922253e43073df1da83bfbf7898a05628f721af804a95e2db609.png)  
![picture 12](../assets/images/140aa0f7f48d0a54a8e43336046aa95b987e868892dadd23f3ea29be31083b35.png)  
![picture 13](../assets/images/de5573730c4e79df015069e73073067fa9fcdd2d89cecf30c383f10bf733335b.png)  
![picture 14](../assets/images/6e7cc57835fd1fec5fae29e6a291b0920cb8deee6bbc3ebbb4158db0cc7c6ada.png)  

>完了咋找出密码
>

![picture 15](../assets/images/584869f7cbc306b6a2960c8e6f959af3fc83e204db876c796c6a6aec685d3c17.png)  

>又是自己
>

![picture 16](../assets/images/8a4a193be94b54cd4793d35069e55efeeb54eda01dab51b94b27a86289e9dbed.png)  
![picture 17](../assets/images/13115f186a6ad8569e37e0f1d8eb467ffbd9698724429a9a22616922c26a3bb5.png)  

## 提权

![picture 18](../assets/images/e457604adf083698b1658ec94c9b751766a73977867fcc52b84227000cfac2f8.png)  


```
welcome@gc:~$ cat /think/Task_Scheduler.sh
#!/bin/bash

echo -e "\n+ Task Scheduler +\n"
echo -n "Please enter the task priority (1-10): "
read priority

echo -n "Please enter the estimated CPU usage (in percentage, 0-100): "
read cpu_usage

echo -n "Please enter the estimated memory usage (in MB): "
read memory_usage

# 用输入的值进行计算，根据优先级调整 CPU 和内存消耗
adjusted_cpu=$(( cpu_usage + priority * 2 ))   # 优先级越高，CPU 使用率越高
adjusted_memory=$(( memory_usage + priority * 10 ))  # 优先级越高，内存使用量越高

# 计算总资源消耗
total_resources=$(( adjusted_cpu + adjusted_memory ))

echo -e "\nTask Resource Requirements:"
echo -e "Adjusted CPU Usage: $adjusted_cpu%"
echo -e "Adjusted Memory Usage: $adjusted_memory MB"
echo -e "Total Resource Consumption: $total_resources"
```

![picture 19](../assets/images/dd798f67ff4564b1b91f70c94634c944062e2c46c1ba828f20080e3ac3aa6ac0.png)  
![picture 20](../assets/images/257ffc2c9d0dd7f160d45034d1af3589ea5f427c5b1e2f1318a42ad188d1d104.png)  

>内容是数字注入，这个形式是之前大佬的
>

![picture 21](../assets/images/25a1f3b34f66ed2efb04e7815f2daa9f867c3b93dde7a0aed34863da77b7934e.png)  

>这个提权的话找半天没啥想法,看了wp
>

![picture 22](../assets/images/14a4e1049fb002942e9f67e39a00743929d87f298415607c0326e8f7403f26f1.png)  
![picture 23](../assets/images/2b037d61a8d603cfcf49127106ac4afd7537346cf197877ac3a20bfb26e7488c.png)  

>好了对我难度挺大，哈哈哈
>

>userflag:
>
>rootflag:
>