---
title: Self-VM Bamuwe复盘
author: LingMj
data: 2025-05-05
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---


## 网段扫描
```
nterface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.59	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.132	3e:21:9c:12:bd:a3	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.135 seconds (119.91 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-04 21:27 EDT
Nmap scan report for Bamuwe.mshome.net (192.168.137.132)
Host is up (0.013s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Library Membership Registration
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.66 seconds
```

## 获取webshell
![picture 0](../assets/images/28dc07fc557700a621785611e9b2105f136b0771323795d1fd3386cbc54a0dd1.png)  
![picture 1](../assets/images/72b6bc308e656d6c24df17648061b45e6ad944e2f43eb2a156e21e0000fdcb9e.png)  
![picture 2](../assets/images/2c5034dc6851dc1a40a0229848299513ca65586817fc990f3946b778b3fa7875.png)  

![picture 3](../assets/images/7b7b39c31501a1fa3edde7e74b80ed361250d2a35441a89b3fe8c0d38366749c.png)  


```
<?php
// register.php
header("Content-Type: text/html; charset=utf-8");
libxml_disable_entity_loader(false);  // Preserved vulnerability point

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $xml = file_get_contents('php://input');
    try {
        $dom = new DOMDocument();
        $dom->loadXML($xml, LIBXML_NOENT | LIBXML_DTDLOAD);
        $data = simplexml_import_dom($dom);
        $email = (string)$data->email;
        echo "<div class='result'>Registration Status: Email ã".htmlspecialchars($email)."ã submitted!</div>";
    } catch (Exception $e) {
        die("XML Parse Error");
    }
} else {
    echo '<!DOCTYPE html>
    <html>
    <head>
        <title>Library Membership Registration</title>
        <style>
            body {
                background: #f0f2f5;
                font-family: "Helvetica Neue", Arial, sans-serif;
                margin: 0;
                padding: 20px;
            }
            .container {
                max-width: 500px;
                margin: 40px auto;
                background: white;
                padding: 40px;
                border-radius: 12px;
                box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            }
            h2 {
                color: #1a73e8;
                margin-bottom: 30px;
                text-align: center;
            }
            .form-group {
                margin-bottom: 20px;
            }
            label {
                display: block;
                margin-bottom: 8px;
                color: #5f6368;
                font-weight: 500;
            }
            input[type="text"], 
            input[type="password"] {
                width: 100%;
                padding: 12px;
                border: 1px solid #dadce0;
                border-radius: 6px;
                font-size: 16px;
                transition: border-color 0.2s;
            }
            input[type="text"]:focus, 
            input[type="password"]:focus {
                border-color: #1a73e8;
                outline: none;
            }
            input[type="submit"] {
                background: #1a73e8;
                color: white;
                padding: 14px 24px;
                border: none;
                border-radius: 6px;
                font-size: 16px;
                cursor: pointer;
                width: 100%;
                transition: background 0.2s;
            }
            input[type="submit"]:hover {
                background: #1557b0;
            }
            .result {
                padding: 20px;
                background: #e8f0fe;
                border-radius: 6px;
                color: #1967d2;
                margin-top: 20px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <h2>Member Registration</h2>
            <form method="post">
                <div class="form-group">
                    <label>Full Name:</label>
                    <input type="text" name="name">
                </div>
                <div class="form-group">
                    <label>Phone Number:</label>
                    <input type="text" name="tel">
                </div>
                <div class="form-group">
                    <label>Email Address:</label>
                    <input type="text" name="email">
                </div>
                <div class="form-group">
                    <label>Password:</label>
                    <input type="password" name="password">
                </div>
                <input type="submit" value="Register Now">
            </form>
        </div>
        
        <!-- XML STRUCTURE EXAMPLE -->
        <!--
        <user>
          <name>John Doe</name>
          <tel>123-4567890</tel>
          <email>admin@admin.com</email>
          <password>secret123</password>
        </user>
        -->
    </body>
    </html>';
}
?>

```

>input应该可以执行命令
>

![picture 4](../assets/images/3e57c41502ae53a88a120d7ca09145934ee20e9e4c80c87e42ef1a5c029cf4db.png)  
![picture 5](../assets/images/52cd43673b0a210fc075a6f3f4457e84aa5ea17d3a3793d0163d87345347efd2.png)  
![picture 6](../assets/images/4af6a9f5a75f54b342848de4436aa5ea6470a8abbb395beccf05c68faea1daf4.png)  

>爆破一下没啥东西，2条路一个phpfilter研究一个是继续爆破查能看什么
>

![picture 7](../assets/images/abb304df1cdc55a5aaf86dd37c250a062e009583809a6f2fb22aa6ed9932b76b.png)  
![picture 8](../assets/images/28c1232880eca31e91a3ddc64aeeade7ec0208262b00999eda5cb2c63aa848c0.png)  
![picture 9](../assets/images/c4f3601332680c4190a8063d46a16464eeb6d78c38d12d547823ba45fa106421.png)  


## 提权

![picture 10](../assets/images/5b9cedfebcf9160bef1b80c08b0ad6d4afd585f1f4a934631842ff4f9a3f7447.png)  
![picture 11](../assets/images/a3d7a5c092cb823ce1d41407bbc683943be7a740dbf50e876b97988ecfd3acf2.png)  

>有个东西root运行
>

![picture 12](../assets/images/26649d8a63c91d23e14c701847c363f8f32f5f226a36c65d536d747338db86c1.png)  

>要是能劫持就结束了
>

![picture 13](../assets/images/e9ca16190d66f65d98c3910d8b893ca0c545010a633961c1cd7771ff3cb0b1d4.png)  

>接受信息的话而且有文件在根目录选择软连接方式
>

![picture 14](../assets/images/0f44ef5aa8c3b809b0cc9e8761ed9166a20ed1281049300ecbe5b0656c480fc7.png)  

>之前安装过了
>

![picture 15](../assets/images/437e70fbc1857bb426320bf72f2447e39f785fa47b44d04aadcad91e5e753d0d.png)  
![picture 16](../assets/images/0fd25259d60a119d3a87bb2963f83fb22d8ceafc483e8d270bda100d3ba37b3b.png)  

![picture 17](../assets/images/091c3bba52dcd694546d03fdf1e83ec047409b4c4e368ece7ccd58cc06e940c2.png)  
![picture 18](../assets/images/1ec3ebd58491d5c5872e19a4dc93ce903004d3e7a766c4eb4821a3b396457214.png)  
![picture 19](../assets/images/50c20f51d5cb50cd0ec79bfa83c54bf3e3ec5015654f9a6014bfb334f70c598c.png)  

>好了结束
>


>userflag:
>
>rootflag:
>