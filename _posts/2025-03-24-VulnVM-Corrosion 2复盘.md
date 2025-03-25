---
title: VulnVM Corrosion 2靶机复盘
author: LingMj
data: 2025-03-24
categories: [VulnVM]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l    
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.71	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.215	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.110	62:2f:e8:e4:77:5d	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.133 seconds (120.02 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.215
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 03:50 EDT
Nmap scan report for corrosion.mshome.net (192.168.137.215)
Host is up (0.0099s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 6a:d8:44:60:80:39:7e:f0:2d:08:2f:e5:83:63:f0:70 (RSA)
|   256 f2:a6:62:d7:e7:6a:94:be:7b:6b:a5:12:69:2e:fe:d7 (ECDSA)
|_  256 28:e1:0d:04:80:19:be:44:a6:48:73:aa:e8:6a:65:44 (ED25519)
80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
8080/tcp open  http    Apache Tomcat 9.0.53
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/9.0.53
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## 获取webshell
![picture 0](../assets/images/dce570771396fd8da5ab087ed85e24eab474b4186c19f94e91537f1f149f2f69.png)  
![picture 1](../assets/images/54e867cbb3702b9dfbd36660a39344a6caebadab4286e110183c7fae6eb243f1.png)  
![picture 2](../assets/images/3013b5c59664f4fa508dba97171ab273bd8fe4fef18808f61b60f27f72057b63.png)  

>无直接利用的漏洞看到登录进行拿shell了
>

![picture 3](../assets/images/2d725335a1ca066d3ca2aeacb4c0ff2cfe8aeada9b2de5234123889ea5116723.png)  
![picture 4](../assets/images/fd8b918f2e8c88178fa7a12b9498a390a44c993a6673b77426f5bb4d1589733a.png)  
![picture 5](../assets/images/044f3d9fa69a5017e8160423d69c2abb8f928af487d2694ab1f2d3eb97880d56.png)  
![picture 6](../assets/images/9b57b82903d41cdf95bbb6c16ba9171b629805cd5cacdc5fd6ccc2df3f48e595.png)  

>爆破一手
>

![picture 7](../assets/images/012fb815f8920ed244fe649e0b132e1b0a499f139c80e18b2ea56d4650f1a08a.png)  

>很卡的前提我先等着了
>

![picture 8](../assets/images/b6c84b5fc8e4bb6e9ebca3dcddf8486010c016cde443d761b20a510504dc7a79.png)  
![picture 9](../assets/images/8b99526d82c1c058e4819161a956a6e11d9d532d1cda5aede7e87f987d642558.png)  

>目前来看2个方案好像是ssh登录或者tomcat爆破登录先试ssh，因为那边在跑，网站里面有一个文件
>

![picture 10](../assets/images/e38280df7a36c76d69da99daac8d9785c6e8c58334a98935ca536c1cc9e30cc1.png)  
![picture 11](../assets/images/9ce4edd03f7e62572feb284c91eca48d867bec3e700c78ad547d4d85c054ace0.png)  

>换一下工具跑起来就是快但是好像不是这个方向。
>

```
Hey randy! It's your System Administrator. I left you a file on the server, I'm sure nobody will find it.
Also remember to use that password I gave you.
```

>难到这句话不是跑密码登录么，现在ssh和tomcat都没见成功,所以它是需要目录全扫找文件么？
>

![picture 12](../assets/images/bef3cd8b5e04edfb739e8d1508ddd696743520f6464328a2a9c025ce913635a0.png)  
![picture 13](../assets/images/37652a393716b921bda45ab2c3598718e450d01ca8a07f0b2dfb5ae13d736948.png)  
![picture 14](../assets/images/9aa62d54d514acef0c15575b309043aa2373c93005bd7d78547f5a44da9446fd.png)  

```
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
<!--
  By default, no user is included in the "manager-gui" role required
  to operate the "/manager/html" web application.  If you wish to use this app,
  you must define such a user - the username and password are arbitrary.

  Built-in Tomcat manager roles:
    - manager-gui    - allows access to the HTML GUI and the status pages
    - manager-script - allows access to the HTTP API and the status pages
    - manager-jmx    - allows access to the JMX proxy and the status pages
    - manager-status - allows access to the status pages only

  The users below are wrapped in a comment and are therefore ignored. If you
  wish to configure one or more of these users for use with the manager web
  application, do not forget to remove the <!.. ..> that surrounds them. You
  will also need to set the passwords to something appropriate.
-->
<!--
  <user username="admin" password="<must-be-changed>" roles="manager-gui"/>
  <user username="robot" password="<must-be-changed>" roles="manager-script"/>
-->
<!--
  The sample user and role entries below are intended for use with the
  examples web application. They are wrapped in a comment and thus are ignored
  when reading this file. If you wish to configure these users for use with the
  examples web application, do not forget to remove the <!.. ..> that surrounds
  them. You will also need to set the passwords to something appropriate.
-->
<!--
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
  <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
  <user username="role1" password="<must-be-changed>" roles="role1"/>

-->

<role rolename="manager-gui"/>
<user username="manager" password="melehifokivai" roles="manager-gui"/>

<role rolename="admin-gui"/>
<user username="admin" password="melehifokivai" roles="admin-gui, manager-gui"/>
</tomcat-users>
```

![picture 15](../assets/images/139f86a25e6ce5e4d1180cada36039daa910b7a63558cdaf770e8cdbc0a8bf08.png)  

>无语，算了也算能拿shell了
>

![picture 16](../assets/images/91860a35e9610c4f27d87c63fc3bbb749fad4bb311fb1ac005733ea56e9283f7.png)  



## 提权
![picture 17](../assets/images/3c09b23e5eb7f70efe3999c2f08898be20b284bbdc8a67ab00a4486bcd45dd0e.png)  

```
tomcat@corrosion:/home/randy$ cat .bash_history 
tomcat@corrosion:/home/randy$ cat note.txt 
Hey randy this is your system administrator, hope your having a great day! I just wanted to let you know
that I changed your permissions for your home directory. You won't be able to remove or add files for now.

I will change these permissions later on.

See you next Monday randy!
tomcat@corrosion:/home/randy$ 
```

>又是猜谜哈哈哈
>

![picture 18](../assets/images/af80aec3e8e7a408bde6af33f0bf9ad2298cf0505c9491650a0d19587d590b9d.png)  
![picture 19](../assets/images/edb21283a2ae6a3bfb986bd3ae0deec4189db79ab5d908a251fb889c8a9a8a84.png)  
![picture 20](../assets/images/b3ef9823754ea610c8f97d5fa1fbbe0f682b5e9c73fd24a03584832667a16429.png)  
![picture 21](../assets/images/7e8eafcdebe55155e1cff12fd7918ad9339bd182cce21b048977b9ff35ec8287.png)  

>不像是有密码的样子
>

![picture 22](../assets/images/68630a928095daa77c4ed630e2480221489dda7dcd9ca755489d59d3c84c51d5.png)  

>跑一下工具，不想看,根据原来的提示是找密码但是现在压根找不到爆破不现实
>

>它密码就是tomcat那个，melehifokivai，不过我一直试randy肯定是不对的
>

![picture 23](../assets/images/2937379dde224aa0cbdaad9e4c3dec6625268735101c3134e7899b8604b76f33.png)  
![picture 24](../assets/images/60991be35d8ac03e22ea3c055df793f088827e17939ade602a99f850ff9636b7.png)  
![picture 25](../assets/images/bec3233fbec59e573d705263760388a331ff4bb3a00a154dea623f942ba24756.png)  

>正常要是能改什么的就直接结束了
>

![picture 26](../assets/images/e0c554c6c1be7b0ea015b1d7f90d1cdd777ec5f32f4502271eb81a1bfab869ba.png)  

>能改那就是可以直接提权了，但是我得知道调用了什么，很烦
>

![picture 27](../assets/images/5240e9579070d949afecc3cf7c0e0786e48839bbd4e5b1840e4d5e8467e7be24.png)  

>这个进程也不对啊很烦，不应该啊
>

![picture 28](../assets/images/5a2bee20b54a8970513b9d603dc7385fc00e616d28a1396b2baa2a5c0aa5b723.png)  

>改一下看看谁会调用它
>

![picture 29](../assets/images/817f0ffcfe5c7656d471127face9dfe9f6959a6cf4a1322bd738d01c1a37e1fa.png)  
![picture 30](../assets/images/d12cbd3a4eeda3d2e4450c734c6092c80b9777e9b2e4cd7e3b519c95b55fec6b.png)  

>还是得提权到randy才行
>

![picture 31](../assets/images/3658b95016bf902203ec69d39ec43727a5a895464511848ed4dec91765af35e4.png)  
![picture 32](../assets/images/9194bb440c3bd06aa87d53c05ba41b30876bce7506d185f18ab8671f4189bdba.png)  

>无,真不知道咋找这个信息，给我卡在这里卡得死死的
>

![picture 33](../assets/images/338b5502a7eb383ca6129532621501f929188ffad5f5ec770d7b49e679638d2d.png)  

>对了个东西，看看能拿信息不再不行我受不了了
>

![picture 34](../assets/images/437ba726b9fafc1154a1370a8754f1a22b4198dded0f9d2e1f3a6aa7846da566.png)  

>啥啊不能干
>

![picture 35](../assets/images/5cfc46d6b5a4017f956ab6856aac3525b89013b00e84177a03b1f51e275ec419.png)  

>无语找一下发现look是这个，很烦
>

![picture 36](../assets/images/e13cf4b2d4050cbccba9769409e3cb2dea33f84975e511e56038c77f348d13e1.png)  
![picture 37](../assets/images/958a895361929c64073dc62cbc5149209f3349e4fcd7408083c6a74317bc30f7.png)  

>要空格
>

![picture 38](../assets/images/e12480c0d567e165b5c1186cc239a07c5936601232f62abd39c7f311e75801f9.png)  

>真是垃圾靶场
>

![picture 39](../assets/images/336572ddfd90b4b170cf4e2d7ef42e1315ac99ccd28c3cc3aae3a0abf72cdf5f.png)  

>等着把密码跑了
>

![picture 40](../assets/images/e25b459f31ddb7c84f959beed38d19982fce2807beda58b1556aca4e95ecdb41.png)  

>算了记得有ssh
>

![picture 41](../assets/images/2d7b6178843f0e345cf202e9a46af312550b3cde78f384d243a738c7f269f9d2.png)  

>哈？真的假的啥也没有，还要一直等，无语了现在没辙不过其实也不用拿我也知道后面咋提权，就是改base64直接提权没啥压力所以我再跑8分钟没有就下机了，主要是base64可以写奥，所以怎么猜，反正我拿root了，所以拿不拿shell不重要感觉
>

>算了密码没有，不打了，反正目前看也不是啥难的靶机
>


>userflag:ca73a018ae6908a7d0ea5d1c269ba4b6
>
>rootflag:2fdbf8d4f894292361d6c72c8e833a4b
>