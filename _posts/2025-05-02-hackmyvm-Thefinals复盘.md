---
title: hackmyvm Thefinals靶机复盘
author: LingMj
data: 2025-05-02
categories: [hackmyvm]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l 
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.53	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.138 seconds (119.74 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.53 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-01 22:23 EDT
Nmap scan report for thefinals.hmv.mshome.net (192.168.137.53)
Host is up (0.011s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.9 (protocol 2.0)
| ssh-hostkey: 
|   256 42:a7:04:bb:da:b5:8e:71:7a:89:ff:a4:60:cd:4d:29 (ECDSA)
|_  256 37:32:71:ca:3f:11:41:b4:d7:90:1e:c9:7f:e8:bc:20 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Unix))
|_http-server-header: Apache/2.4.62 (Unix)
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: THE FINALS
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.59 seconds
```

## 获取webshell
![picture 0](../assets/images/6b48b65f71e34ddcc8bdbaa20a5820d3d9b55e282b81f5571b00bed6c7b7c62a.png)  
![picture 1](../assets/images/5b69cae06c685ca483c246f7d5f78f3f066f5ade51bfad6a12b035c390517be3.png)  
![picture 2](../assets/images/d85a701d0571cfabb839221da56b00f88d133e6ab5dc4ab74ecbdfe9227ea485.png)  

>好像存在登录的地方找一下
>

![picture 3](../assets/images/b8cec00363a6b94d063f393757a9e5c6b0d029a50e1a0d122415042f42c5046e.png)  

>密码爆破很慢而且没事收获，sqlmap注入登录没有反应
>

![picture 5](../assets/images/272adf36f66f968b88a491f243d0cf6ff8a8e6a61b864d1422d5bdb44ffd7ac1.png)  

![picture 4](../assets/images/6aa60822a7353601ea3147294dad203bd5e754224cdab27a527ea227861ca5fc.png)  

>目前版本不对
>

![picture 6](../assets/images/54f4ef8a8fd80bf3cc75781c1e890c1f5091b1c7cfd6c41b51be3c1ea2d3f30b.png)  

>我觉得唯一线索是用户名
>

![picture 7](../assets/images/4bffd71c3f428af2ced44b6434988febfbd7ae1c07e24a926ff37190da7e7f6f.png)  

>没有特殊注入点感觉很难做，感觉方向错了
>

![picture 8](../assets/images/165f30f019a8501f5bb0bdb240ec1a8dbe96f70b8212d68780b6e58e7c6cc378.png)  
![picture 9](../assets/images/9c18058c356473296bd45b4a223fdd08f599ea8041ce0a230535b873133db402.png)  
![picture 10](../assets/images/1f2e3969cf75d8e31879eeffcda750f1e5357e512e0a672e902ac28638619c92.png)  
![picture 11](../assets/images/e28e2854aba728853379752a36ab35017b90e532d99f784cd60f524e4db5767e.png)  

>现在需要外带用户密码出来利用xss
>

![picture 12](../assets/images/6f4bcae0c79e90c6784c784c162a33d59ec04ddab0547a69c18887f9aa2d3ca1.png)  
![picture 13](../assets/images/7419e601dbe850e944029563bee40727e7c5d6899217608abf736040e36aa58d.png)  
![picture 14](../assets/images/73b1058080aa5f2502071594d24be96f6d458253b6ff8c61ebb6e77a22f618e1.png)  
![picture 15](../assets/images/63d56b1d40214b087594a847ea18da1050b45ad8eb3bc09e1c8f879eac185c44.png)  
![picture 16](../assets/images/11a956a006367a3e5fe8e9b26c76cd84c47c41a3ae48515c419029aa010caa59.png)  
![picture 17](../assets/images/16797e0f70776ba269ce6f6e976850da202f6e6e305bf307ff1912de80d17825.png)  

>明明已经进去了为啥出现这个情况
>

>找到了地址：https://blog.itpub.net/70041598/viewspace-3051742/，然后命令的话
>

![picture 18](../assets/images/008a3ead02ae4ac825b38edff6ef54654dc6d28a9abff3dc12f0fe302778d695.png)  
![picture 19](../assets/images/498437d903de10f47e8b7f48d401df6e660f8afb657b23f1ba929b2e0d3eea16.png)  
![picture 20](../assets/images/4c08c78e587438d8fc458fe9205d6a2fa0b7e080d0ec6decfbe99202c18f5196.png)  
![picture 21](../assets/images/115ec92233e34bce12f798d16f83f5957881a2db68e52d62ddc16e763655fb5a.png)  

>没有bash只有sh
>

## 提权

![picture 22](../assets/images/b0e9d5eb4f106a022408b93d9c255211ac1b5e37def302288aa2dfc0d9141545.png)  
![picture 23](../assets/images/9aec447148fc5ea07481d1f5d6734460104b59025f667d99848043ad06c80210.png)  
![picture 24](../assets/images/43706c8ca6bf1c0837449b30171537065587a2f58faa25890e5cbc5ee948ec87.png)  
![picture 25](../assets/images/86dab7a0351305641fc39527c9332ad5304c97e4718c8dd847fd7d8b216adce0.png)  
![picture 26](../assets/images/ec64afcee2d97a442192731a52b3d8c48f93ed3a44f49ba68479972ab049a493.png)  
![picture 27](../assets/images/b23c7706cca0fcc98ddbaf186cc426bc464a62d17c6ebfae3ec8f4cf6a47364c.png)  

>直接查不行，我怀疑是定时任务所以现在要解决问题是控制它定时触发我有东西接受这个广播
>

![picture 28](../assets/images/7656b12bca7ac7c3a3f2421c36af36f5e8f43d9f94403edc90dfe53c32ffc95a.png)  
![picture 29](../assets/images/48afdd9bd8a751c4272545157fb96531957fb828ee15967f57748b09e049b76a.png)  

>gtp真是不行，看一眼大佬怎么做的nc -u -lp 1337，原因是我一直tcp去接受，别人端口是udp
>

![picture 30](../assets/images/0a4153b4184141f4b05c2c68d63afafdb0a351526eaab6f61de3790ecc43b119.png)  
![picture 31](../assets/images/35ada2124aba7ee00b24378201b8bc1c9a4dd500e9c482d7689c17ea5818e699.png)  
![picture 32](../assets/images/3f3c8865e4043c6d0f7c9c1c2bfddc0df2124e6007eaf9d578d46ab8baef8665.png)  
![picture 33](../assets/images/8c3b85242a84829c573d92b7fe68a1fc4d30d6e2e3b0d829e410b55c69d6bae4.png)  
![picture 34](../assets/images/8c08c1812eed44ff8b360427c343138fa7449f3326900118af8af34c5ef9a5f6.png)  

>没有多余密码只能进行那个窗口操作，我以前打hmv就是手动脚本的话还不好实现，所以手动一下
>

![picture 35](../assets/images/ceea719a8e376a480598ee7bf39f8832865e3c407a5c31dfbfc34b6d17b4cfe5.png)  

>也不知道为啥巨卡我直接复制出来
>

![picture 36](../assets/images/4a958055884a106ba9b72e675ad9d8d520f7acec5eeabf52a163c58658c86ba9.png)  

>还不直接是密码
>

![picture 37](../assets/images/a3e24e6e342afaef79736d50a31fc4ef27dbdfe642c1ea99716f8761a894c6a7.png)  

>结束
>

>userflag:flag{4b5d61daf3e2e5ba57019f617012ad0919c2a6c29e11912aeadef2820be8f298}`
>
>rootflag:flag{8c5daa407626d218e962041dd8fd8f37913e56e32a6f06725da403175be0b9ff}
>
