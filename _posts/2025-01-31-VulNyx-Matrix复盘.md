---
title: VulNyx Matrix靶机复盘
author: LingMj
data: 2025-01-231
categories: [VulNyx]
tags: [php,domain,sudo]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.202  00:0c:29:28:0a:0b       (Unknown)
192.168.26.254  00:50:56:ec:c0:9f       (Unknown)

5 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.908 seconds (134.17 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.202
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-30 21:28 EST
Nmap scan report for 192.168.26.202 (192.168.26.202)
Host is up (0.0015s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u4 (protocol 2.0)
| ssh-hostkey: 
|   256 67:78:c9:d2:e3:ff:be:fc:9e:13:9a:af:9d:59:17:66 (ECDSA)
|_  256 1a:78:b1:e6:f1:f0:d1:b3:ab:c8:3f:95:fd:46:52:67 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Enter The Matrix
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 00:0C:29:28:0A:0B (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 64.11 seconds
```

## 获取webshell
![图 2](../assets/images/34cfb6bf7303ec8899fe0fb82fbacf08ba926ba94cdaeadd97dd55572a9f39c1.png)  
![图 0](../assets/images/3885057a07a06c95e3fe33c88bdc99c3dc54c5677d65aac63c0262b34cc82a8e.png)  
![图 1](../assets/images/fa14e64b44e07e7768de03022ea99ea803f58f1956c26d1886acad70a0a64820.png) 
![图 6](../assets/images/5a0ccb2a51bbd0c5f63ff2baefb5b868a6a6b041ce1ce923f0390a7791b2f723.png)  
![图 3](../assets/images/4a5ce6255a3b1068582b5c866b2c622d131f307b6eae9566cf0f1de49a0d4717.png)  
![图 4](../assets/images/674ef049472dab620e5db4fb56340a3d463a646fff911b5026bdac8eaddfb2bd.png)  
![图 5](../assets/images/26efeabc647e5bfb017120a21a194fb66cb25b9a41e5db926a995e4ea0160a1d.png)  

>到这里我很有感觉但是感觉端口缺了
>
![图 7](../assets/images/a553ceec3f0a2b79f1c6f905da935ab17544f86a5c18a5baac09156417e7d4a5.png)  

>把域名都检查了一下没有东西
>
![图 8](../assets/images/ecd9299b224fae2f7cf506740240bf048a675fa64eb7238476c6b90196bf2005.png)  

>也不是这个，有点懵了，没啥线索了，我直接扫目录，不然就得爆破，目前看用户名是知道的，密码不知道
>

![图 9](../assets/images/22e476f2ecaf9d02dfeddfcdd7418146e92a95564841f8d3f1cf15f33acbcfc3.png)  

>这里还有一个域名
>

![图 10](../assets/images/74df5b705c90af2cb2c9bb9886afd0d41fa8ea7f9eb77f38e8053f805eef278c.png)  

>有些眉目了
>

![图 11](../assets/images/02692ae64d0565cdfa89e1d6364dd2f722b6ea6bfe9978639663d9a612718ba6.png)  
![图 12](../assets/images/57e62378f48bbd5c6d4133bf473733b4c9eff4f2939ec976ed6b0e3c9271e2f9.png)  


```

            // Chat logic with AJAX
            const messages = document.getElementById('messages');
            const sendButton = document.getElementById('sendButton');
            const spinner = document.getElementById('spinner');

            /**
             * Serializes an object to PHP format (similar to serialize() in PHP)
             * @param {string} message - The string message to serialize
             */
            function phpSerialize(message) {
                return 'O:7:"Message":1:{s:7:"message";s:' + message.length + ':"' + message + '";}';
            }

            function sendMessage() {
                const input = document.getElementById('input');
                const text = input.value.trim();

                if (text) {
                    addMessage(text, 'user');
                    input.value = '';

                    // Disable button and show spinner
                    sendButton.disabled = true;
                    spinner.style.visibility = 'visible';

                    // Serializar los datos al formato PHP
                    const serializedData = phpSerialize(text);

                    // Enviar el mensaje al servidor usando AJAX
                    const xhr = new XMLHttpRequest();
                    xhr.open('POST', '', true);
                    xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
                    xhr.onreadystatechange = function () {
                        if (xhr.readyState === 4) {
                            // Enable button and hide spinner
                            sendButton.disabled = false;
                            spinner.style.visibility = 'hidden';

                            if (xhr.status === 200) {
                                addMessage(xhr.responseText, 'bot');
                            } else {
                                addMessage('Error: Unable to reach the server.', 'bot');
                            }
                        }
                    };
                    xhr.send(`data=${encodeURIComponent(serializedData)}`);
                }
            }

            function addMessage(text, sender) {
                const message = document.createElement('div');
                message.className = `message ${sender}`;
                message.textContent = text;
                messages.appendChild(message);
                messages.scrollTop = messages.scrollHeight;
            }
        
```


![图 13](../assets/images/88ac8ee191f1da47daba52d44ec2c9be3d911b2508ea960d41a1377874bbc1cf.png)  

![图 14](../assets/images/941135c2df989e7d306b7eb24d1770d59a9ade981448a28c3b11094e73825223.png)  
![图 17](../assets/images/c9082a73769a4a25328dc5afad12d075cafcf03c6ca9a034187217f0050b38b5.png)  

![图 15](../assets/images/c84286d389406dc0b55ee8839604fec1724eec370bfeba12439cef1548b59622.png)  

![图 16](../assets/images/99124d17962f4602b3b13dbb0f96fad0a849438a5912ef029a07c0ab4ebdb9ef.png)  

>这玩意是有一定的那个消息回显的，你输入的东西够多就能回显出一个想要的东西
>
![图 18](../assets/images/375130ed7ca7b3d0f7c720f98c13856bde70e81e5ec31e1af71a9b1a1642c677.png)  
![图 19](../assets/images/520a1b6d3a25773b6d9b0c8e0f8af5f974861af98666603b897d410b510788bb.png)  
![图 20](../assets/images/fec77b17645ca621867994a52042aed7eb5918c5cd7ec6dff1bcf5596fede5e3.png)  
![图 21](../assets/images/5bae2a03a81a6fe318b55c3e6254ddd6ed64326dd63fdfdd3c6d74aefe440a80.png)  

>这里很烦主机访问不了，得去kali，kali还卡死
>
![图 22](../assets/images/23b0a5d0ff2ad76417a8f16b038a898e49ff9df68dd8ab2cf1c4262c22c582ae.png)  

>巨卡，我感觉可以利用hackbar去做
>
![图 24](../assets/images/c4ea4644ac6a7936b7761efdecdffdd8efbc06e70e3eab6d8d48b1a0429f9d7d.png)  

![图 23](../assets/images/b4b672a558e41ade3cd06a0111528234201bd20cce6177c485f4bfa1ffb218e7.png)  
![图 25](../assets/images/af538e861c8b6553adec6b9d395a0fd45f2014c9f8982de3b3362ec4a50a8045.png)  
![图 26](../assets/images/6519d1dbd23bebcea93af037c5f411db28f95f50b3412c57bca15a65084eb4e2.png)  


## 提权
```
www-data@matrix:/var/www/M47r1X.matrix.nyx$ stty rows 43 columns 208
www-data@matrix:/var/www/M47r1X.matrix.nyx$ ls -al
total 40
drwxr-xr-x 2 www-data www-data 4096 Jan 31 12:59 .
drwxr-xr-x 4 root     root     4096 Jan 28 21:00 ..
-rw-r--r-- 1 www-data www-data   31 Jan 31 12:59 cmd.php
-rw-r--r-- 1 root     root      361 Jan 27 02:47 filtrate-backend-matrix.php.txt
-rw-r--r-- 1 root     root     1765 Jan 27 01:04 hoja.css
-rw-r--r-- 1 root     root     4782 Jan 28 23:55 index.php
-rw-r--r-- 1 root     root      806 Jan 27 00:48 matrix.js
-rw-r--r-- 1 www-data www-data  173 Jan 31 12:55 messages.txt
-rw-r--r-- 1 www-data www-data    2 Jan 31 12:49 shell.php
www-data@matrix:/var/www/M47r1X.matrix.nyx$ 
```
![图 27](../assets/images/bf4c2489632e3065999a04cc67c25d13cac8e5c55676fe579d5e64331c29a53b.png)  
![图 28](../assets/images/edd7fdee4f65a740e1a6732e07ce4d873159792e3f215b49440485351f5b5629.png)  

>这个密码是里面的su不能使用ssh
>

```
smith@matrix:~$ sudo -l
[sudo] contraseña para smith: 
Matching Defaults entries for smith on matrix:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User smith may run the following commands on matrix:
    (ALL) PASSWD: /usr/bin/rsync
```

>好了到这里就算是结束了
>

![图 29](../assets/images/edc90ba49bd873c33bb58f5c5a0c256546e481fd68c4df30a97851c06d9fd874.png)  




>userflag:13fd11421e33199c2029bc8e5ed94626
>
>rootflag:5f3cae74fbcf1919cc7db7604317187a
>