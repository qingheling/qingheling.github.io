---
title: VulnVM Administrator靶机复盘
author: LingMj
data: 2025-03-02
categories: [VulnVM]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.5	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.66	a0:78:17:62:e5:0a	Apple, Inc.

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.101 seconds (121.85 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.5
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-04 18:56 EST
Nmap scan report for administrator.mshome.net (192.168.137.5)
Host is up (0.0072s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9e:5d:29:05:44:f2:fc:c7:b5:cd:c0:c0:4d:c7:b9:4b (RSA)
|   256 a4:35:b4:ca:be:d3:8b:95:fc:14:f2:55:c5:80:a5:bd (ECDSA)
|_  256 62:83:88:6a:5e:77:c1:c0:ed:ed:e6:eb:6d:10:68:9b (ED25519)
80/tcp open  http    Apache httpd 2.4.41
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-title: Login @ 17.0.0
|_Requested resource was http://administrator.mshome.net/dolibarr/htdocs/
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.43 seconds
                                                              
```

## 获取webshell
![picture 0](../assets/images/d5f2f582faff62d0e940becca0d04820812ebd5e801700dfafa8200c4ee72581.png)  
![picture 1](../assets/images/629e392f7a1bfafd726fd70fbc976f2a1e3e8e3abea9dcbf06b5426e278cccc4.png)  
![picture 2](../assets/images/cea6dca8cbbf36ccd6bf6b380632c5e004179234db036e8118dd37d427d84e68.png)  


>这里我研究2天了最后看wp获得突破口,这里我获得的突破口是连接自己数据库就说吧localhost改成自己的ip说实话真想不到我吧其他窗口都试了除了root和这个地方，被我默认不可更改
>

![picture 3](../assets/images/23de1942be408e3c78c7a02195dacc9d50044621203895bdddd7cd8076107bb5.png)  
![picture 4](../assets/images/bb806a69a42b4db57ce56d1232e2fa0298cd8dc7ebe863ecf8f3e760902f551f.png)  
![picture 5](../assets/images/377fc50226682daf2c7d01b20b3084e0d7071eb6eff9d0b555fbeecc70689d0b.png)  

>首先启动mysql并且把mysql的地址设计成0.0.0.0不是127.0.0.1,修改文件路径自取
>

![picture 6](../assets/images/be3c38055d14bc2f680d3ecc37e6048bf7174a5e6b0f4d3b27e8c58f4889a305.png)  

>接下来你需要自己找一个数据库创建并且换一个登录的比较好不用root这个，当然这个随意，保证创建一个用完就删，不要把自己配置搞得乱七八糟
>

>方便大家不用又查一遍命令我给我的命令出来你们自行修改
>

```
MariaDB [(none)]> CREATE USER 'xxoo'@'%' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.012 sec)

MariaDB [(none)]> CREATE DATABASE dolibarr;
Query OK, 1 row affected (0.003 sec)

MariaDB [(none)]> GRANT ALL PRIVILEGES ON dolibarr.* TO 'xxoo'@'%';
Query OK, 0 rows affected (0.004 sec)

MariaDB [(none)]> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.004 sec)
```

>用户名自己填密码也是
>

![picture 7](../assets/images/59660aa15258c29bea174c4aa6c987346d280b0d0ad7565873c71af1fd43119a.png)  

>好了前期工作做完可以连接数据库了
>

![picture 8](../assets/images/78971532293709f8724dac49ceeb2c358c44570ec3afd4356809c02d29ca72eb.png)  
![picture 9](../assets/images/59ea7b49a30d60fae136f173ae5b1e1cb39d34723a636f8647e76d093dd1b80f.png)  
![picture 10](../assets/images/e92e6aa64324a9c4c64f056a475b698ae5077fefd3d365f47ff64fbfc5e59715.png)  
![picture 11](../assets/images/37f9b56872b89c22404b04de7aa46a740068150dcd8ae90bab12fc154983700b.png)  
![picture 12](../assets/images/3f3832ec55e42128d30b861e01e41cdd75cb3a10b8779a4fc759b86027f3a031.png)  
![picture 13](../assets/images/36e1513f28de2c2a978dc3719451e75ce1ae6d4529866efa18d1ea85a0dcacb8.png)  

>接下来版本漏洞，这些自己查就行了
>

![picture 14](../assets/images/6b9ebc77c65b8b9bc7728cf062b9df6f89297265eda6150f646c77944b6128aa.png)  
![picture 15](../assets/images/2e7435ef0e8d7fd33d086b257c0532ee39414f51b450e304b2bdf820b596ee3d.png)  
![picture 16](../assets/images/5c547a6a121cee8728239514d7f24c3388b67251fe535cb34c25e07ce54f7516.png)  
![picture 17](../assets/images/ca047816b79ecd68c96b6e054c2900d5032540f2d6566488fa04d23ff373ef19.png)  
![picture 18](../assets/images/d47c6485ad829c7ca67544930e985836315df6347494cdd687b520d9c799c9fb.png)  

>这里有一个坑
>

![picture 19](../assets/images/f107cee7c3732dc244a45b270da518b092d3284142e9002325f9e7b202d442dd.png)  

>国外没直接见图文并茂的方案，可以看看，地址：https://blog.csdn.net/sycamorelg/article/details/140251903，这里演示的是手工注入，我们不用之需要把事web这个页面开启就行
>

![picture 20](../assets/images/ddc1e687655fb5da2f5cc9136136f7b9163ad5054ab7eab18a9c482eab2c9761.png)  
![picture 21](../assets/images/d830e3ef7b58992c2926106697147dc5e04d31415d6c600517993221f7a8b194.png)  
![picture 22](../assets/images/280f1514facb2105efba251049bd16821cf5ce3421ad487e18244923786a6e27.png)  
![picture 23](../assets/images/2e37023e92634c8b9432884d5dd782de2330ba5a17be5ad71406f071fdff6e84.png)  

>还是没成功换一个poc,换一个poc也没成功直接进行方案利用吧地址：https://github.com/dollarboysushil/Dolibarr-17.0.0-Exploit-CVE-2023-30253
>

![picture 24](../assets/images/913156e6de71da142f34f367e2f6746960386f204fcc4c426d6b67ad0270d0d7.png)  
![picture 25](../assets/images/14aa4d765b363fdd5be07cee6ec8114b63277c0a54ae51c3cc4caed95b118af4.png)  
![picture 26](../assets/images/532682bd04e85b4617a4310299f15096270c2acea9a985f40cd4174689d65d4b.png)  
![picture 27](../assets/images/10f1f5d5812471746c69269090f27afaf25456fc599d4978ad3d6ccdae938dbe.png)  
![picture 28](../assets/images/93ec10b9508e7715049cb5e96b8ec5039b4271f1383cea77a8d3a916b8300ca0.png)  

>没成功么？
>

![picture 29](../assets/images/e0b0cdfa3d1b99568e8106779b3cb54e928f1e6e72c6b973bbcbe80e0cf5060e.png)  
![picture 30](../assets/images/7d0ae439fb431cae2b805244870ad62f58861b2d09ef842cbf1483742f9ef127.png)  

>这玩意要打开
>

![picture 31](../assets/images/ea653c95e5f09a8dbac561dd4f3de9ce34a8b7fdabf59ccc37b4bde807454496.png)  
![picture 32](../assets/images/8bd6f9d81c39e936e1efc77e8e6d06b963ccd10e1d513dc18e4d87fa20cbaca8.png)  

>绕过一下就好了
>


## 提权
![picture 33](../assets/images/ee26c485da857c1198c6a9d8dfc5f0d28cbb3a1440fe2a60903e0f328d0e6622.png)  

>开始找数据库
>

![picture 34](../assets/images/29f11de9f0313506dec6492e0231ebd4ab27d1bd51771650f08d291942d4fa8a.png)  
![picture 35](../assets/images/89ce3d0c426221bce88b3bafff531d36ae9392128f92e02f19e3cec4e96bcb93.png)  

>看来不是这条路
>

![picture 36](../assets/images/99795f222860298bc2e9649f98e337ec4f8c9e6781d9b29566151e40a558257b.png)  

>找密码吗？
>

![picture 37](../assets/images/24738b0d84d20a12b64329bf9e666019e2a7d8bd7787bec644834855e61c0ddb.png)  

>这个更像root提权
>

![picture 38](../assets/images/261d0298ba88c0aa2dda9e34b93fddbc63d657ddf16524c50c4cb6f8aa37466a.png)  

>有ssh可执行权限，还有其他用户提示
>

![picture 39](../assets/images/12cb58323e5c8379e19f586c2e5e7baa3fcb5e35d67e17519f86d1cc85623bfd.png)  

>那没用但是不应该啥也没有
>

![picture 40](../assets/images/f1251d0369f7392025bb07f5ebc1247588ce4b276ea5487752f7b3c4bbd65198.png)  
![picture 41](../assets/images/f0e4972f68fe8cb51d08ed3789154f670aae6e2e1edb773e8710df9ecfeb754a.png)  
![picture 42](../assets/images/a1c8379cb71b2bd52217432d5b6b40f3320d42d218da2d73d9cef82323608672.png)  
![picture 43](../assets/images/91bd81167cf48d916b457a0eac8642eb76721221358300434e911b3ba7ca5a56.png)  
![picture 44](../assets/images/8cc81a3f2ce08417d5b63446ab39778fd1cdc24a483846c1c916a7cecc156a5d.png)  
![picture 45](../assets/images/28ddd85c70340ad0c8142262360cecb6ce52caaf6b7e15bb90ef0914faeabcdf.png)  
![picture 46](../assets/images/8dd6649437d2273db55e2bce7b3890faee02c43a9264791e34454138e950b19e.png)  

>查了半天浪费时间就应该回这个conf里面看
>

![picture 47](../assets/images/4a637dad3a705e5a76100886ff37e6cfa6d1f908e8861f0bff10adef940b9486.png)  

>是一个用户密码
>

![picture 49](../assets/images/6324897fbc17a56ca32dd4bcede60adb3648c857a46219ae9ce83e9de93fa5d6.png)  

![picture 48](../assets/images/4b8b55a650b32e4f94f3430359003079f35f97332ecddace294cef86afefc787.png)  

![picture 50](../assets/images/6a89844227745af1d1d517244b6105db4ce45e499d8d5068edcd2a88f617cbc6.png)  

![picture 51](../assets/images/ddc40fa726fdcfc15f2eb4b795673619aa38d8ed7d72a1bdc0d6344e1b50002a.png)  

![picture 52](../assets/images/87f776066ce832dcbeab2bc8ca5b01d7c9bd9402df2e5199bf8a7c3ce58a54b1.png)  

![picture 53](../assets/images/f65359a23e2b22fa7945edf30b39d47c3139fa4f6b114579bea17b03921446a9.png)  
![picture 54](../assets/images/62a22e7200fa3281bb94520263f47dffccca1bc755242dcac0a40c448c32d2a2.png)  

>好像变得不难了，找密码思路是对的找半天我是不理解我自己的
>

![picture 55](../assets/images/c5b0b66115329d3af73d6f0d1a7cfc5284d26d59d278b62c07c7d9af5e402041.png)  

>好了root方案也有了我自己给自己写个公钥算了
>

![picture 56](../assets/images/82b73d313c310ff0bbef5901e888645b1729b799d6798c26d00ad25809175131.png)  
![picture 57](../assets/images/ac1ea814d0fe0b016eace5ae7d43ce74e348fdaae93ba02abf882c00ebd9fcf5.png)  

![picture 58](../assets/images/c3d02cdb0b2c6fadb857f9c6093daddd21b00944e31793c75893591ce610556a.png)  

>不起作用
>

![picture 59](../assets/images/0c160e923e9b5b4f4112f19b60c6f3e849e115c0bdef2c32d97917c14f2c67f5.png)  
![picture 60](../assets/images/0147d256adfb091ed6f2abb2d7739340c7f06ed756bfb9d189eba07a21d53ce7.png)  

>原来没写进去
>

![picture 61](../assets/images/8e00617c640b9b2a9d173b125d25e66c1184526abd4539f229645bfb3ff5575f.png)  

>有读的话能软连接吗？但是目录不可以写不然王炸了
>

![picture 62](../assets/images/0b6a2eb2296d50cfe407b57c72f58f7967a299935edac5d5657030869745bf29.png)  
![picture 63](../assets/images/b2abf563d08af1ae7d769c984c05f3e8c58af99095b9355a6f1e57683b67642c.png)  
![picture 64](../assets/images/741234c8b1af40139bd64d46b55a36db30bf1e8daae6f8d50aca0d4f67487106.png)  

![picture 65](../assets/images/a1663ab8ee0a31cf9494a0c9d4b8eb9ab0adc5fe33687cce5dbd965469700ae9.png)  
![picture 66](../assets/images/5355a9a3226331f481933bf599b6b9986e8fdbe94315a0641aaee287564da948.png)  
![picture 67](../assets/images/305927ab738fb04794d4b010dc48fcebae1870c5b89130d292799e6f2efad7a9.png)  
![picture 68](../assets/images/b8877a0957b14084711226182f95e8a81d24987274c50103d6da77195fc75cc3.png)  
![picture 69](../assets/images/b38e2afd58b67c18e27632ac409b6bcf2238ef410fcc241ede5650aa4faa684a.png)  
![picture 70](../assets/images/56e691435d7a5bd9ee190a17ea2660a2912deee215e7517ead8992b4d791d5b5.png)  

>不枉我查怎么久的poc，ok，到这这个靶场对我基本结束了
>
![picture 71](../assets/images/1aa70e13bc788531ee73091ba9ee9839fe9d33c0e1736104a2ddaceaf5b4b9b8.png)  

> OK，总结一下这个靶机是非常有意思的靶机root提权和webshell都是我没见过的，算是一个全新体验。
>

>对了看了群主的视频还有自动生成工具补发一下地址和方案：https://github.com/j0lt-github/python-deserialization-attack-payload-generator
>


>userflag:c20f80f45da20ec2b6edcb6575297b85
>
>rootflag:8a0f989c11a724123ef293c7318bdab6
>