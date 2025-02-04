---
title: hackmyvm p4l4nc4靶机复盘
author: LingMj
data: 2025-02-03
categories: [hackmyvm]
tags: [sed,passwd,LFI]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:/home/lingmj/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:fd:61:55       PCS Systemtechnik GmbH
192.168.56.129  08:00:27:6a:2f:23       PCS Systemtechnik GmbH

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.389 seconds (107.16 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj/xxoo# nmap -p- -sC -sV 192.168.56.129           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-03 05:09 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.129
Host is up (0.0081s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 21:a5:80:4d:e9:b6:f0:db:71:4d:30:a0:69:3a:c5:0e (ECDSA)
|_  256 40:90:68:70:66:eb:f2:6c:f4:ca:f5:be:36:82:d0:72 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 08:00:27:6A:2F:23 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.93 seconds
```

## 获取webshell
![图 0](../assets/images/2e9335fe11edb7f0f8d4fb943058c25b2f42f665ceb4465b1a689e0d18c83313.png)  
![图 1](../assets/images/e745132beda9369dcdc4012c2b9c3f0a1880769dc05c247df9152f778641be6d.png)  
![图 2](../assets/images/3e0f7c7edcd8bee8561938e09e0d18cf8c8020a064a498f4de5b1e1ce858fbfe.png)  
![图 3](../assets/images/ddd30917c904097daa286ac45f4062951570753a10df63c2e870776ce93f4755.png)  

```
A palanca-negra-gigante Ã© uma subespÃ©cie de palanca-negra. De todas as subespÃ©cies, esta destaca-se pelo grande tamanho, sendo um dos ungulados africanos mais raros. Esta subespÃ©cie Ã© endÃ©mica de Angola, apenas existindo em dois locais, o Parque Nacional de Cangandala e a Reserva Natural Integral de Luando. Em 2002, apÃ³s a Guerra Civil Angolana, pouco se conhecia sobre a sobrevivÃªncia de mÃºltiplas espÃ©cies em Angola e, de facto, receava-se que a Palanca Negra Gigante tivesse desaparecido. Em janeiro de 2004, um grupo do Centro de Estudos e InvestigaÃ§Ã£o CientÃ­fica da Universidade CatÃ³lica de Angola, liderado pelo Dr. Pedro vaz Pinto, obteve as primeiras evidÃªncias fotogrÃ¡ficas do Ãºnico rebanho que restava no Parque Nacional de Cangandala, ao sul de Malanje, confirmando-se assim a persistÃªncia da populaÃ§Ã£o apÃ³s um duro perÃ­odo de guerra.
Atualmente, a Palanca Negra Gigante Ã© considerada como o sÃ­mbolo nacional de Angola, sendo motivo de orgulho para o povo angolano. Como prova disso, a seleÃ§Ã£o de futebol angolana Ã© denominada de palancas-negras e a companhia aÃ©rea angolana, TAAG, tem este antÃ­lope como sÃ­mbolo. Palanca Ã© tambÃ©m o nome de uma das subdivisÃµes da cidade de Luanda, capital de Angola. Na mitologia africana, assim como outros antÃ­lopes, eles simbolizam vivacidade, velocidade, beleza e nitidez visual
```
```
cat dir|sed 's/i/1/g'|sed 's/l/1/g'|sed 's/a/4/g'|sed 's/e/3/g' > dir1
cat dir1|sort|uniq|tr 'A-Z' 'a-z'|sed 's/-/\n/g' > dir2
```


![图 4](../assets/images/4267a4cb9c024e2db19d55a473220a008b9ec8a552fa5ff4ce4a3d0f34e463b5.png)  
![图 5](../assets/images/e5c098b8d7cc771baf1aaf0860c76172ffa6490cea658e0b1dfc0a680471e586.png)  
![图 6](../assets/images/db9617d9899a8ed5df8e71f13fe23744dd89747a581be8ab3e397bcaec412a30.png)  

>大题过滤就是这样，是一个500的php，wfuzz一下吧
>

![图 7](../assets/images/051ed35c3d75f72204ae6a63a85008e185bdeefd7ae93f5f4c0430f72f04b896.png)  

>没成功怀疑不是LFI，感觉是直接命令执行
>
![图 8](../assets/images/489dfc25fc5be514f0d16fc9747a0c893d19c0cd64605ab8ec753e9852b397d0.png)  

![图 9](../assets/images/0fa723e9d4c67bb2220082f641b9399294562a9a405d6fe5c73803f77f195014.png)  

>去掉目录穿越就出来
>
![图 10](../assets/images/02f6bc616bf1d7d218ae9e174967bc44a8c3ded87fd7caf16d4373c6a6bd3da0.png)  

>只能说字典太大的问题
>
![图 11](../assets/images/ec8e2a848c7aa77d876d29c07aeda7eca25e39e669bc630bd4653d1b0a317151.png)  
![图 12](../assets/images/185831c6fb95ce981b6d5d7421261076fe99a7c5914dc39393eb3e8ced04cf7d.png)  
![图 13](../assets/images/07f9a56864497ce8e3914d160bca504c07513a28150a7eb64f9316c649cae60c.png)  
![图 14](../assets/images/05bd8bf3f81b31b732a2fd9818c8a914e367085129839c1b08ef3fc36fbadc4b.png)  


## 提权
![图 15](../assets/images/c29c61b8809635400ff215f78275ffd52d52ff0f522edb726c2f402ee98026ba.png)  
![图 16](../assets/images/c81b474d152d6a11af605da183fb298e1b8449db6fc99eeda8afcab8bd3589c7.png)  

>结束

>userflag:HMV{4c3b9d0468240fbd4a9148c8559600fe2f9ad727}
>
>rootflag:HMV{4c3b9d0468240fbd4a9148c8559600fe2f9ad727}
>