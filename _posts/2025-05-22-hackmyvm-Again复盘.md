---
title: hackmyvm Again靶机复盘
author: LingMj
data: 2025-05-22
categories: [hackmyvm]
tags: [cap_fowner]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.13	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.150 seconds (119.07 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-22 07:09 EDT
Nmap scan report for again.mshome.net (192.168.137.13)
Host is up (0.0048s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   3072 d3:7b:32:92:4e:2e:e7:22:0f:71:92:e8:ac:f7:4b:58 (RSA)
|   256 75:d7:be:78:b0:c2:8c:78:98:a5:aa:ff:bb:24:95:0c (ECDSA)
|_  256 09:fe:ed:a8:ad:af:c1:37:98:24:3d:a6:9d:e7:9b:6d (ED25519)
80/tcp open  http    nginx 1.18.0
|_http-title: Again
|_http-server-header: nginx/1.18.0
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.88 seconds
```

## 获取webshell

![picture 0](../assets/images/8e2fb2d0e0d852f88b5558fee63f07b79f03eaf3b5285ca90c8eeabdd6e63da0.png)  
![picture 1](../assets/images/79381f9683da83e86437df6269e9ee5f90ee02a8dd034b54e53867f00866e64e.png)  
![picture 2](../assets/images/7babdaace5474fe2a00edff6916543bd8a1f773a542182c12ee079103b22abf0.png)  

>什么东西
>

![picture 3](../assets/images/69722f3ca71474633f9fcdb1e5b0efdb317a5852d9ffe1219bf14111a58ffe8e.png)  
![picture 4](../assets/images/c583af31bca3b5d4180920ecaffd1540de9dc2c7ec084b56ca149ecbf992b082.png)  
![picture 5](../assets/images/ef3660c49696832cd7be59178b3f8bfbbf2e5a09a086d75b4e45a993fc60f454.png)  
![picture 6](../assets/images/0d992aa368c302fb88fe4a2ef3760df841833673c6981896fcaabe2e3211dfec.png)  

>挺多不行的
>

![picture 7](../assets/images/6342fd96d6551af2f4d7bc01be73820542ab3c7a6a08f200c66b855d8a250ed6.png)  
![picture 8](../assets/images/5ace191857224794f0cb0f55e02c68869003d334f378ab94cf2340cf2988c148.png)  
![picture 9](../assets/images/4b0d407dd886197d9f79a1cc0422b1f0f27f1aa6fc4bfaebfdcfa141a951839e.png)  
![picture 10](../assets/images/65c3a3c2bbbf4335fed54feb600b1a8d143b7c07206e9b6dbe88e10fca151123.png)  
![picture 11](../assets/images/218d415328125e75ff6b08fbbff4a005c11afe074391b9dec867a59453475425.png)  

```
<?php
if (!isset($_FILES["myFile"])) {
    die("There is no file to upload.");
}

$filepath = $_FILES['myFile']['tmp_name'];
$fileSize = filesize($filepath);
$fileinfo = finfo_open(FILEINFO_MIME_TYPE);
$filetype = finfo_file($fileinfo, $filepath);

if ($fileSize === 0) {
    die("The file is empty.");
}

$allowedTypes = [
   'image/jpeg' => 'jpg',
   'text/plain' => 'txt'
];

if (!in_array($filetype, array_keys($allowedTypes))) {
echo $filetype;
    die("File not allowed.");
}

$filename = basename($filepath);
$extension = $allowedTypes[$filetype];
$newFilepath = $_FILES['myFile']['name'];
if (!copy($filepath, $newFilepath)) {
    die("Can't move file.");
}

$blacklistchars = '"%\'*|$;^`{}~\\#=&';
if (preg_match('/[' . $blacklistchars . ']/', $newFilepath)) {
echo ("No valid character detected");
exit();
}

if ($filetype === "image/jpeg"){
echo $newFilepath;
$myfile = fopen("outputimage.php", "w") or die("Unable to open file!");
$command = "base64 ".$newFilepath;
$output = shell_exec($command);
unlink($newFilepath);
echo "File uploaded";
$lol = '<img src="data:image/png;base64,'.$output.'" alt="Happy" />';
fwrite($myfile, $lol);
}

else{
$myfile2 = fopen("outputtext.txt", "w") or die("Unable to open file!");
$command = "cat ".$newFilepath;
$output = shell_exec($command);
unlink($newFilepath);
echo "File uploaded";
fwrite($myfile2, $output);
}
?>
```

>这是源码
>

![picture 12](../assets/images/481763a0dd6554bfa1528ceca7cefd047a7c72faecf512f4ae779a9ecd11a63d.png)  
![picture 13](../assets/images/76f93618f00179e5f2f9b7c3bf2832511497864f1b84be7951b8a680e85ab62d.png)  

>让他成为txt
>

![picture 14](../assets/images/22cf2c6a4a5d94f70afb8a5a7320323775d374a741092c0262698795ffc188f1.png)  

>进来没生效，这个部分没想明白看wp让cat哪部分中止即可
>

![picture 15](../assets/images/e9f1d41c7af9f6e3a4042038494c657f5176e6b00ce5b4cc59c40845e00dc23b.png)  

![picture 16](../assets/images/97b978bbb2adfdf1e85fde517889c5ac11f67bd9ab597502dff260b010c3e791.png)  

>好了卡住就生效了
>


## 提权


![picture 17](../assets/images/888c39fa0ba6c1b992c1249349ea1f38f95ad80001e3f7f9536aa8afdf06ec33.png)  
![picture 18](../assets/images/2cda9f20a40abf30b4f77be34d7ded34352aadfd806055588a10b9ccbf97292e.png)  
![picture 19](../assets/images/f8249459e6466b965fd9d21cfa33054899e6675eca6adc54746b917e36e29e37.png)  

>等跑出密码
>

![picture 20](../assets/images/3c230e699c08c68e421ef98035c039576839789b95b8e55fda6487ac7b78616c.png)  

>话说有个人运气好扫出这个会不会通关了
>

>现在密码没有所以还不能下怎么早定论
>

![picture 21](../assets/images/31be7c362fec91bb741af1e03c269943f53c064ef572e987a360aef0791a9b42.png)  
![picture 22](../assets/images/8307dc04446e81b3d24d4706c3ffe79c5f9fb17b249470e0623f3669c4f083aa.png)  
![picture 23](../assets/images/b985bca8685b0372105ec2e87bc32673137f30d1abe388aa059a04badc8212e3.png)  

>尝试用户名不对
>

![picture 24](../assets/images/ae64b59a2129f4b27f78d164db689b06e1a46d8e5320b0763f01a91e23ec403b.png)  

>怎么都有这个问题
>

![picture 25](../assets/images/074415dd1b6526125e4089195fdbdb5e8b1af3f9267689e5916bf748f0d4faac.png)  
![picture 26](../assets/images/215dc7e3b00f88da35a1394e1450becd27a191e4bb52676456ece0459493bd9f.png)  

>地址：https://man7.org/linux/man-pages/man7/capabilities.7.html，https://blog.pentesteracademy.com/abusing-cap-fowner-capability-402f6808cd9d
>

![picture 27](../assets/images/e773b870c818ad7c338e58012e59ced25a7c7a7a68dd4a495c053f773b187064.png)  
![picture 28](../assets/images/1e65044aad7cd737982ae2e1b916c56ce92928c76548bf79feb8288b39d36e35.png)  

![picture 29](../assets/images/4d32ea0ac1b0f46db45218ece06d180a1e8d0c6ac64b568b3d6e342f2b09ae4f.png)  

>好了结束了，那么那个私钥有啥用
>

![picture 30](../assets/images/0b72121a82f313e8bd2843b21ea6f1ce8b58256fa395193746e491c6b48a3a20.png)  

>没爆出来算了
>

>userflag:nowtheeasypart
>
>rootflag:andagainandagainandagain
>
