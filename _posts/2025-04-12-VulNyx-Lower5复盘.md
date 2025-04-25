---
title: VulNyx Lower5靶机复盘
author: LingMj
data: 2025-04-19
categories: [VulNyx]
tags: [upload]
description: 难度-Low
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.201	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.081 seconds (123.02 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.201                                                                      
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-11 20:08 EDT
Nmap scan report for lower5.mshome.net (192.168.137.201)
Host is up (0.019s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 a9:a8:52:f3:cd:ec:0d:5b:5f:f3:af:5b:3c:db:76:b6 (ECDSA)
|_  256 73:f5:8e:44:0c:b9:0a:e0:e7:31:0c:04:ac:7e:ff:fd (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: vTeam a Corporate Multipurpose Free Bootstrap Responsive template
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.07 seconds
```

## 获取webshell

![picture 0](../assets/images/c37df2ee4ba2ab3226c2802efddfe820dc1786ff97c6c1eeeec2f7cede53c7ed.png)  

>爆破ssh失败和尝试wfuzz没有东西看来洞藏起来了
>

![picture 1](../assets/images/91181079d7353dd88f37452fd59534bd5c21618971f31819260ebb1b4ef8c100.png)  

>看起来像cupp
>

![picture 2](../assets/images/bcae10983798937648d23c1fe4c6acd11e3f7d6ee1e77be5894ac93eee459ac0.png)  
![picture 3](../assets/images/98d9b94438ea0e1109d6084adecee53db6c8d022fb3c17b99cfee732ec6fbed5.png)  

>都没成功开始怀疑不在这里了,看看udp先
>

![picture 4](../assets/images/04262882fa1482b22cd03f3ff40f6d4aab41f2369f0d8c911ee65b5a7c07f022.png)  

>慢慢想慢慢找了,话说能自己访问自己么
>

![picture 5](../assets/images/5c16478d36b53e1cab6debf8bc50bbdd753f9aab90b9faab7f77f6d12cd8a431.png)  
![picture 6](../assets/images/0a03be8e530b2677180306abb88f75346bc09d8c6188bf0ace2767e03832826d.png)  
![picture 7](../assets/images/12f3854bfc836569aaa5c62009bf325574f3ab75115ce1dae3fe3ab6bbe41af6.png)  

>我发现个问题，原来打过这个东西
>

![picture 8](../assets/images/b788bec088a253db7bb47df8af76b572caaffef422ef0080a6dd31f795c2704e.png)  

>没见phpfilter直接利用,爆破ssh密码了
>

![picture 9](../assets/images/825d7e60b116b16848331583fbcb31f7577dd217967b029509d706e129d7c2a9.png)  

>bp可读路径只有/etc/passwd
>

![picture 10](../assets/images/311f2df96bcd163159602efac8f5abf181ffb0994d01a0c81b127e7a065d6102.png)  

>还没爆出密码我开始怀疑这条路了
>

![picture 11](../assets/images/0f0c84966d12fcae6c8945afe0d32c23367aebc1bbc1c05d777ae7b086c57b44.png)  

>这个是500，难道靶机出bug了？
>

![picture 12](../assets/images/37b04f1bbf8486e188cd63e2b18c697688b21c0bbff1191284873e3d34918aef.png)  

>重启一下好了，又是日志注入，看来
>

![picture 13](../assets/images/79df6895d62fcb074b7eaef8d51c0e13bd917074b8382d82f0d7ab76647e1fc1.png)  

>注入成功了
>

![picture 14](../assets/images/01b743503e30dc15dc9332c6b2bd4b6803f8766c2b5b85572f049725c17a637e.png)  

>命令执行就失败，好奇怪啊,好了又死了这个服务
>

![picture 16](../assets/images/26ec0e3cd5d65d71d1d2908831174da5349602377dd8001267a551b83155d8f6.png)  

![picture 15](../assets/images/f712792c1532a1e3fa82ac9c76e0a9b6f417d5d97233e39663b486566c09d5a0.png)  
![picture 17](../assets/images/b48eab2e9d182346d53c912f4ffc85aee36a5399f32ce29b2465c4c32f4b78ce.png)  

>搞了半天这样才成功去看了wp，忘记咋使用这个log注入了
>

## 提权

![picture 18](../assets/images/133e9ad3b63e8146a551d83a2e9a56665df18cd08ce1b7c7fea19173ccba4d4e.png)  

![![alt text](image.png) 1](../assets/images/5c126f9ca72daee4f0370d6b557a1e34230fea7ee1cafa17e92dce87026919fb.png)  



```
low@lower5:~$ sudo -u root /usr/bin/pass --help
============================================
= pass: the standard unix password manager =
=                                          =
=                  v1.7.4                  =
=                                          =
=             Jason A. Donenfeld           =
=               Jason@zx2c4.com            =
=                                          =
=      http://www.passwordstore.org/       =
============================================

Usage:
    pass init [--path=subfolder,-p subfolder] gpg-id...
        Initialize new password storage and use gpg-id for encryption.
        Selectively reencrypt existing passwords using new gpg-id.
    pass [ls] [subfolder]
        List passwords.
    pass find pass-names...
    	List passwords that match pass-names.
    pass [show] [--clip[=line-number],-c[line-number]] pass-name
        Show existing password and optionally put it on the clipboard.
        If put on the clipboard, it will be cleared in 45 seconds.
    pass grep [GREPOPTIONS] search-string
        Search for password files containing search-string when decrypted.
    pass insert [--echo,-e | --multiline,-m] [--force,-f] pass-name
        Insert new password. Optionally, echo the password back to the console
        during entry. Or, optionally, the entry may be multiline. Prompt before
        overwriting existing password unless forced.
    pass edit pass-name
        Insert a new password or edit an existing password using editor.
    pass generate [--no-symbols,-n] [--clip,-c] [--in-place,-i | --force,-f] pass-name [pass-length]
        Generate a new password of pass-length (or 25 if unspecified) with optionally no symbols.
        Optionally put it on the clipboard and clear board after 45 seconds.
        Prompt before overwriting existing password unless forced.
        Optionally replace only the first line of an existing file with a new password.
    pass rm [--recursive,-r] [--force,-f] pass-name
        Remove existing password or directory, optionally forcefully.
    pass mv [--force,-f] old-path new-path
        Renames or moves old-path to new-path, optionally forcefully, selectively reencrypting.
    pass cp [--force,-f] old-path new-path
        Copies old-path to new-path, optionally forcefully, selectively reencrypting.
    pass git git-command-args...
        If the password store is a git repository, execute a git command
        specified by git-command-args.
    pass help
        Show this text.
    pass version
        Show version information.
```


![picture 20](../assets/images/244af24c3da0489309a021e87f8637d77afb463687c6dacb72aae8de06b93ada.png)  

>什么东西
>

![picture 21](../assets/images/cedc923ebccc41318b6f5bfa1f7e2dd1396e019954f915c1ff389440003f99fd.png)  

>没解开
>

![picture 22](../assets/images/376e69fd1b173c51bd73fb4857c819bd0fd0aa060a1bbebcd48aad6f2ff976c0.png)  

>应该还有什么方式可以处理比如删除密码或者改密码
>

![picture 23](../assets/images/80227735fe6fca7a7c2ec0d01f5d654dcf439ee1e7b46197e2df4288b91e8501.png)  
![picture 24](../assets/images/d13e26e4e028c648434d4a22612db944980e35b67c79a35f6971aa8cbb66b9dc.png)  

![picture 25](../assets/images/c1ca49b25f8716cade81da950427ddf247084496d3a81591b12d4a7f3de21ac3.png)  


>查一下发现这个
>

![picture 26](../assets/images/abd163a7cc15d03b09430041aba5ab017aedb6d06d05507c76e92a2d6280b29a.png)  

>密码被我改了哈哈哈哈，没有root密码了重装了
>

![picture 27](../assets/images/63cb222236c9e074aa49ecefe8a3e8136537c3dca555adb6e89e47edc445d688.png)  

>重装之后就完事了
>

![picture 28](../assets/images/440c07eb2d8d6ab3a03f1010bb8da530803536b439d2e18dd13267d349440b6f.png)  

>结束了顺便看一下完整wp看看有其他路线不
>

>userflag:30a7b18992fef054ca6d904769fac413
>
>rootflag:008cdc7563e1d5afbcac3a241eba4db8
>