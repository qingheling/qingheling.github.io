---
title: Self-VM Link复盘
author: LingMj
data: 2025-11-01
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---


## 网段扫描
```
root@LingMj:~# arp-scan -l   
Interface: eth0, type: EN10MB, MAC: 00:0c:29:fb:0f:16, IPv4: 192.168.137.194
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.97	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.91	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered) (DUP: 2)
192.168.137.91	3e:21:9c:12:bd:a3	(Unknown: locally administered) (DUP: 2)

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.037 seconds (125.68 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p80 -sVC 192.168.137.91  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-10-31 19:52 EDT
Nmap scan report for link.dsz (192.168.137.91)
Host is up (0.41s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-generator: WordPress 6.7
|_http-server-header: Apache/2.4.62 (Debian)
| http-git: 
|   192.168.137.91:80/.git/
|     Git repository found!
|     .git/config matched patterns 'user'
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: wordpress 
|_http-title: RedBean&#039;s Blog
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.64 seconds
```

## 获取webshell

>80端口有git
>

![picture 0](../assets/images/7e1134b995cff82f8392f71bfcd7266f1619bc29f8520247134db8df1aabb6ef.png)  
![picture 1](../assets/images/95835a8b6a3247fd295d17fb42dc307425eb2a0ae8ccb185583e8b93d459e6af.png)  

>wordpress 可以进去看一下利用wpscan
>

![picture 2](../assets/images/80fbadf90b82ea5e28bfcb6b1ffac883bc33719656c8086eac76f513809f0a50.png)  

>还有域名
>

>爆破一手，继续看看git文件
>

```
root@LingMj:~/tools/GitHack-master# cd /root/tools/GitHack-master/dist/192.168.137.91
                                                                                                                                                                                                        
root@LingMj:~/tools/GitHack-master/dist/192.168.137.91# ls -al
total 360
drwxr-xr-x  6 root root   4096 Oct 31 19:52 .
drwxr-xr-x  8 root root   4096 Oct 31 19:47 ..
drwxr-xr-x  8 root root   4096 Oct 31 19:52 .git
-rw-r--r--  1 root root    405 Oct 31 19:52 index.php
-rw-r--r--  1 root root  19915 Oct 31 19:52 license.txt
-rw-r--r--  1 root root   7409 Oct 31 19:52 readme.html
-rw-r--r--  1 root root 111312 Oct 31 19:52 wordpress.sql
-rw-r--r--  1 root root   7387 Oct 31 19:52 wp-activate.php
drwxr-xr-x  9 root root   4096 Oct 31 19:52 wp-admin
-rw-r--r--  1 root root    351 Oct 31 19:52 wp-blog-header.php
-rw-r--r--  1 root root   2323 Oct 31 19:52 wp-comments-post.php
-rw-r--r--  1 root root   3336 Oct 31 19:52 wp-config-sample.php
-rw-r--r--  1 root root   3507 Oct 31 19:52 wp-config.php
drwxr-xr-x  5 root root   4096 Oct 31 19:52 wp-content
-rw-r--r--  1 root root   5617 Oct 31 19:52 wp-cron.php
drwxr-xr-x 30 root root  12288 Oct 31 19:52 wp-includes
-rw-r--r--  1 root root   2502 Oct 31 19:52 wp-links-opml.php
-rw-r--r--  1 root root   3937 Oct 31 19:52 wp-load.php
-rw-r--r--  1 root root  51367 Oct 31 19:52 wp-login.php
-rw-r--r--  1 root root   8543 Oct 31 19:52 wp-mail.php
-rw-r--r--  1 root root  29032 Oct 31 19:52 wp-settings.php
-rw-r--r--  1 root root  34385 Oct 31 19:52 wp-signup.php
-rw-r--r--  1 root root   5102 Oct 31 19:52 wp-trackback.php
-rw-r--r--  1 root root   3246 Oct 31 19:52 xmlrpc.php
```

>这个是个完整的wordpress应该能拿到点东西
>

```
--
-- Dumping data for table `wp_users`
--

LOCK TABLES `wp_users` WRITE;
/*!40000 ALTER TABLE `wp_users` DISABLE KEYS */;
INSERT INTO `wp_users` VALUES (1,'Yliken','$P$B.58QLT1rmg1yTSJN7Qzzkoi9WnXF9.','yliken','Yliken@RedBean.com','http://192.168.56.164','2025-10-28 16:08:56','',0,'Yliken');
/*!40000 ALTER TABLE `wp_users` ENABLE KEYS */;
UNLOCK TABLES;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
```

>看sql文件它是有yliken的尝试hash密码
>

![picture 3](../assets/images/79266568e25393c63cecf9f2afb3925777956609db121ccca9a651a6bd1ef13e.png)  

>不过wpscan出密码了
>

![picture 4](../assets/images/ee27a50596d8f61e6369e7eb8296c81129862bc505629372532fd4191fd3fcd7.png)  

>密码是一样的
>

![picture 5](../assets/images/d1c32d1d875685ccf53186c77f729acb56928755082fc4cb6853a26b919f85ef.png)  

>进来了那就zip插件上传
>

![picture 6](../assets/images/7aea393b82344710bf2127e28645958dba49e94485166e273799efe6c5adae53.png)  

>还有密码，而且输入获取这个不对
>

![picture 7](../assets/images/028711804a9f19254bd27bf97c49ccfb518d0ab8fa9c1353c882464841d65023.png)  

>不过这个提示好像是那个21端口没开
>

>改插件或者主题了
>

![picture 8](../assets/images/32fe005487ee195bb55f4844deeaf505ba6aceb4e7d7052e77cc04fa93025f76.png)  

>我能想到快的就是这个文本的插件了
>

![picture 9](../assets/images/661b0cd4ffdaaba127e72cb0fe76b6023ccc64eddda19574c9b7a7ef28b77b65.png)  

>这个好像不是插件，试试主题
>

![picture 10](../assets/images/726ede5b037e37457d76cb1c663ae18e9a4c838313f5ddc53c338237c7a28af9.png)  

>主题倒是有
>

![picture 11](../assets/images/37858c5a838550a3860ce5324d814e42faad893716d670df50c3000f6712f246.png)  

>懒得扫问ai拿路径
>

![picture 12](../assets/images/d574c2e4b7aaa93e2c2e58d78f78698e50a6c92eaa9c0d3bab918c8ab78e27da.png)  
![picture 13](../assets/images/ab5ebfbb080f91b3504ea50d8eac07d900e39cbb26a301a788a73fa01465c202.png)  

>全是php改个404的吧
>

![picture 14](../assets/images/293892a9650cf4e96762bfecb7b0966995e15f709f567a060a158143163de55a.png)  

>看一下生效不
>

![picture 15](../assets/images/b8e7f189ee6a9915155d8b0c23ef959e3dc54cabcc938712679269abc93f0830.png)  

>OK的直接拿shell
>

![picture 16](../assets/images/de1928cc8b1b529432a4b5339af0604b822096aebd31d484a7391be5f8687583.png)  

>懒得去找bypass function用蚁剑吧
>

![picture 17](../assets/images/ce7a9efb3143248afb58daf2c2cf2e414d29df4c2c5ec9065fee0c16206dd5a6.png)  

>OK连接成功
>

![picture 18](../assets/images/2021f0bf42c8b646261355637f296a4a6ecda153b3f8b11a5469b3c20370da55.png)  

>getshell到我终端
>


## 提权

>拿到了看一下mysql
>

```
ww-data@link:/var/www/html$ mysql -u root -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 5138
Server version: 10.5.23-MariaDB-0+deb11u1 Debian 11

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| wordpress          |
+--------------------+
4 rows in set (0.003 sec)

MariaDB [(none)]> use wordpress
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [wordpress]> show table;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '' at line 1
MariaDB [wordpress]> show tables;
+-----------------------+
| Tables_in_wordpress   |
+-----------------------+
| wp_commentmeta        |
| wp_comments           |
| wp_links              |
| wp_options            |
| wp_postmeta           |
| wp_posts              |
| wp_term_relationships |
| wp_term_taxonomy      |
| wp_termmeta           |
| wp_terms              |
| wp_usermeta           |
| wp_users              |
+-----------------------+
12 rows in set (0.000 sec)

MariaDB [wordpress]> select * from wp_users;
+----+------------+------------------------------------+---------------+--------------------+-----------------------+---------------------+---------------------+-------------+--------------+
| ID | user_login | user_pass                          | user_nicename | user_email         | user_url              | user_registered     | user_activation_key | user_status | display_name |
+----+------------+------------------------------------+---------------+--------------------+-----------------------+---------------------+---------------------+-------------+--------------+
|  1 | Yliken     | $P$B.58QLT1rmg1yTSJN7Qzzkoi9WnXF9. | yliken        | Yliken@RedBean.com | http://192.168.56.164 | 2025-10-28 16:08:56 |                     |           0 | Yliken       |
+----+------------+------------------------------------+---------------+--------------------+-----------------------+---------------------+---------------------+-------------+--------------+
1 row in set (0.000 sec)

MariaDB [wordpress]> 
```

>感觉线索不在这
>

![picture 19](../assets/images/83ffea6faf985f839d9056ab76992e88bc1080f8092743e538fb995994e5557c.png)  

>密码都不对，跑个linpeas吧
>

![picture 20](../assets/images/81f6a10d204804dc06542f3e21d8025966da275d8bd70d5e08e7ff5fc75e04c2.png)  
![picture 21](../assets/images/83011a4b804212979d7bdbf2e1d0635bba7dc8e8fd6673be00865a0a512d8f5b.png)  

>有docker不过感觉我应该拉不下镜像
>

![picture 22](../assets/images/6f56534e59b6bf27840b92130bf4328465a9e6e63eedc03d8fbc919283e1979e.png)  

>root提权方案有了
>

![picture 23](../assets/images/e67b9ae91b9f8df2ce867a068917d9288681e192501b0cfe18c1942371a0bb8f.png)  
![picture 24](../assets/images/2ce8a9eb3c85b79b8b9ef0aae93e2c8c1bfd493ec477eb44628521c2edc30adc.png)  
![picture 25](../assets/images/147db1e3d0d6eeadf0b79e785c8f900e13fd784c5a8fd826c337106291e558f9.png)  

>猜谜啊，中文感觉有定时任务试试应该和这个txt有关
>

![picture 26](../assets/images/0bfe49515f9937c167ded5d6434061c8eba0b4b00a8c759d645277049fde313d.png)  

>也不是密码呢,没有定时任务哎呀有点不知道咋猜，cupp一下了
>

![picture 27](../assets/images/f1c3ab4b2b1c20d9d3e590ec74acb1f98bd9aa6845a351109a8a611e79999698.png)  

>fscan也试了，hydra也试了
>

>有8080端口
>

![picture 28](../assets/images/b44f223051ca3419c604d19de2113ba9ad59af942dd05f7adb5dab01bd8f4e75.png)  

>结合名字link和目录下有一个filebowber应该是这样的路
>

![picture 29](../assets/images/16d41ca51239b6114493e5db8ebdab09a584735a1ec9fb34baaf32741831d761.png)  

>拿到私钥可以结束了
>

![picture 30](../assets/images/3b20448c8c13ea2723abc253accb57b7564ccff19d253507032c07a0f1632a40.png)  
![picture 31](../assets/images/6c2a7ff6fadb5c8657ca0f5ad30f13a74062f8f1fed6d8ecbb8f0501f0727254.png)  

![picture 32](../assets/images/63ab1f8d7815e71251a46798d448f978b153aba95194a6178c152f18c8cd876b.png)  

>有留镜像，结束了
>

>userflag:
>
>rootflag:
>