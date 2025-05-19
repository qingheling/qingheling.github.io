---
title: hackmyvm Otte靶机复盘
author: LingMj
data: 2025-05-19
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/tools# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.96	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.114	62:2f:e8:e4:77:5d	(Unknown: locally administered)

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.063 seconds (124.09 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sV -sC 192.168.137.96 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-19 07:15 EDT
Nmap scan report for otte.mshome.net (192.168.137.96)
Host is up (0.019s latency).
Not shown: 65532 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--   1 ftp      ftp            89 May 15  2021 note.txt
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 e8:38:58:1b:75:c5:53:47:32:10:d4:12:79:69:c8:ad (RSA)
|   256 35:92:34:4e:cd:65:c6:08:20:76:35:ba:d9:09:64:65 (ECDSA)
|_  256 a2:87:9f:60:a4:0d:c5:43:6a:4f:02:79:56:ff:6e:d9 (ED25519)
80/tcp open  http    Apache httpd 2.4.38
|_http-server-header: Apache/2.4.38 (Debian)
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Basic realm=Siemens - Root authentification
|_http-title: 401 Unauthorized
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: Host: 127.0.0.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.62 seconds
```

## 获取webshell

![picture 0](../assets/images/69279e0776b53f919bf70e1eefa2f3ff8f6f2abb5951a21531735afebf1aa174.png)  
![picture 1](../assets/images/24fb1b9734550cfec19438f271caa78240dad82c37c961ba3a50506a9d292901.png)  
![picture 2](../assets/images/187608d91bc8629aceb9fe8ef7b1539812e123bde494d2b65a3a8931baf18591.png)  
![picture 3](../assets/images/98d8fc2dd545ca26e42568e46082163dbd47e0e08ac481bc771c20eab77bd37e.png)  

>用户名知道：thomas
>


![picture 4](../assets/images/4b6f2358e6ef7fd704b77d9cf12ceb51b752e1ba6cb9dfd72dc78fed40c343ba.png)  
![picture 5](../assets/images/5b2f2ba5368eac1639d953d78fac6018f2f15c56412d11ee5c451ea0b23fddab.png)  

>感觉爆破ftp如果爆破ssh那不应该这个提示
>

![picture 6](../assets/images/437468f96a7a6e67a5318264f4a663b864ecc3b2310ff3616cce94be11b33b9a.png)  

>没有
>

![picture 7](../assets/images/0e05809565a6244548d76a3726dbf0dd02243602bd4ef4fc264c1a4546d85b90.png)  

>admin也不是,等着ftp爆破
>

![picture 8](../assets/images/2d6077c34617dc4d11219f3fe6b54a486e9d08854573c5d803d376ad3588a338.png)  
![picture 9](../assets/images/20740f028723f1eac199bd1166e5a9e2f844678d84ec31f020eda3077c5408c3.png)  

>看了wp奥我跑了ftp啥🪶没有所以我打算看看入口咋进行，这里有个很狗的东西我们还显示不了，一个登录提示奥,elinks
>

![picture 10](../assets/images/d055a2830105df0438be928e19053f33dcad48ae29a2f49ddfca84a0a4d00553.png)  

>正常是这个样子的，siemens
>

![picture 11](../assets/images/258de28c672eb6b1a9b7e11a809c41904abee23b19a2e285d862ee2186e68949.png)  
![picture 12](../assets/images/295c99cb71e254748e2f68f7a4de40d365d505b64b7abd6480df030b1b7a4e49.png)  

>有点难找，目前没找到，随便找到的网站：https://www.192-168-1-1-ip.co/router/siemens/s7-1200-s7-1500/17618/
>

>如果不是的话那就没招了，去wp扒了
>

![picture 13](../assets/images/8663a210ea577f9ee3cae7ba89cbba4ca958f0e73084b90932bfdf3c276b39e0.png)  

>找到了
>

![picture 14](../assets/images/6db71de8bd73296b1ac31929f566d92b0ab7f45aae88ce5e80a3bfdb2da7c417.png)  
![picture 15](../assets/images/28bb46a417a5be41d030319d6dd0ad44f4bc267a34749c170f9f02556ff32a4a.png)  

>看一下咋写这个认证
>

![picture 16](../assets/images/cf9eda1834fbfec0da434c99f469121222ea187aa5e893b52df0d8f443c15394.png)  
![picture 17](../assets/images/94e6d33cb1b4fde7d851147921beaf3214a24c057e359687019643987549c929.png)  
![picture 18](../assets/images/6097d2ae48e0567c09990bf51738b1923ddbf7f310748eb1c78906f08d1d3ced.png)  
![picture 19](../assets/images/3fb3f05a55e9f666cafb1c8fdb7b29bdc0faba3c57a0e4302603621f11c4adf6.png)
![picture 21](../assets/images/b9eebc0b5704f57ea238da1943baa89aa9b0ff31ea1855444afcead890fae2d6.png)  

![picture 20](../assets/images/bcacfb90ad75852b6c55d0de0904a0fad79acd90462a91d2d30dc6b727b4bd7f.png)  
![picture 22](../assets/images/5fea79ced31366e4028cfb3770c29b829fd5bd657edc514e031b813204dc4451.png)  

>还有其他php
>

![picture 23](../assets/images/f59175100fb2446454a388dba6c858899a398989f0d11432566cf0158801e1d1.png)  
![picture 24](../assets/images/2fb951a7203ff2fe0b9ece709c54ae17fc89db369f42243b65ed721a63b3ad4c.png)  
![picture 25](../assets/images/05dff6a6d62d15a5c054bae012e832f8314d8d47295c8f6565cdfa019d7d3551.png)  
![picture 26](../assets/images/b9c8585b18ca2670582607588540f7b953354482839c158f03c7f77522e946a7.png)  
![picture 27](../assets/images/c9fa97950a1071314798401870dcac6098b35b3f1e99b7cc108a1c66b037dadc.png)  

>php有啥呢，用fuzz跑一下
>

![picture 28](../assets/images/192b244eacc664e585f337482aa7417c38872f7078548334e55ad84a9a28a434.png)  
![picture 29](../assets/images/8832db026b50b2488bbec4376773522bd485ead7afc1543dac740559d82988de.png)  
![picture 30](../assets/images/0913ba29c9421c67b2ecc6febcc350b58a44b97f34f6be863de5295dea3914bc.png)  

>什么东西一个have fun！，是密码么
>

![picture 31](../assets/images/4ebced454ff7c37b692ebf697d2b10e7f8cdf828a0a02354f16eafb291cd3ae8.png)  

>不是
>

![picture 32](../assets/images/5169f719ef5008cc75ea5043cf736587232ddd15bf38349c90a3d26d041f6e5a.png)  
![picture 33](../assets/images/a90d50ecf5468f6ee45a5fd3ac0a9b166745b63c82f13d88181e707bdbcff117.png)  

>file也不对,又卡主了不知道怎么操作
>

![picture 34](../assets/images/3ea13a212da30ec0ea77eaafee67698f762d2266a7a0a8f3d75e98d071c5be7f.png)  
![picture 35](../assets/images/fa7c59e9102ebc79955f2b8cd76af4f66e81b052b281ce2c5889e55d629ced4b.png)  

>方向包错的换一个方式吧
>
![picture 36](../assets/images/8a928259a2a1d6142e131b463c21b014ce1a93b8af3e923edf0766a84db696a8.png)  

>看完了没收获
>
![picture 37](../assets/images/7755f7e70e53757200699762e724d9ba2361b1ddf34ea70816284d98e56e2456.png)  

>还不能看自己只剩一个
>

![picture 38](../assets/images/320c97dd99f4fb432890b537e0a163c49c528cae92e8d8cd4624d6663a14c0d5.png)  

>也没有
>

![picture 39](../assets/images/9c823a1bb84acf97ccc2e408e20e6a0339341d6897949075ff0548cf5d3b6c2a.png)  
![picture 40](../assets/images/e165f12ccd5ebbbf10be62a3d72186430146044b940e68222e0172f424c66db5.png)  

>开始爆破了，这shell.php一点用也没有，就一个have fun而且还不是密码
>

![picture 41](../assets/images/91f5934e5b1992ef14ef6cfb5e87990656ed377291e08b6757e7e9eabe9760b5.png)  

>离谱的是非得跑完这个字典真6
>

![picture 42](../assets/images/138e64cf31ecd3da2772447e61cb06a2ce5c63a67004e6c341b6315c238f38d3.png)  

>还不是这个用户
>

## 提权

![picture 43](../assets/images/9dcd701b1cb1019bf8f254396757504be45ac4aa012ab6b16adb09319dc91218.png)  
![picture 44](../assets/images/8bebbd2ca6bc2fda3e6b2443d8525572419662926b5e4146f3e8570ac9679b43.png)  

>这个密码是之前那个的
>

![picture 45](../assets/images/1e304914be0b031a081cfc3537bdf920077225c0f15b414a60fa7a2563b3a7eb.png)  

>没有密码复用问题
>

![picture 46](../assets/images/9bc101cd4a54365c1dc231389eea9cbca27402d5b648f73976f19f1a24afcc94.png)  

>有信息但是不多
>

![picture 47](../assets/images/5eeab1839234b6f4bfbf211eabebb58eb33d833cec6a72dabbcf895233712ee3.png)  

>真翻译真脏，哈哈哈哈
>

![picture 48](../assets/images/bec3810aef225105d7a7cf598ff88f1bd7e741a19dbaf8d493e49b12bb9e4d35.png)  

>看出来了全是xxxxx但是感觉是图片啊什么的，或者压缩包，所以我们修改一下如果是手动修改的话确实骂一下，哈哈哈哈
>

>爆破的话是4*4**26吧还是很多感觉误解，但是前面之前不是有jpg么拿那个头应该就可以了，这是我的推测奥
>

![picture 49](../assets/images/ca4bb643c46277d85185ef8908c44f6c2c92d7d1bb781408af99817241664d0f.png)  
![picture 50](../assets/images/f3e496a75f7f472191fd5283f066fe40cefc6f6d853b1360868acd814293a35f.png)  

>gtp直接给我驳回了
>

![picture 51](../assets/images/32ca716ac58e240d56bcbdf89be962262098dcbd1d8298dc488fafc0ec1942a7.png)  

>c的文件我随便找的
>

![picture 52](../assets/images/6af8b09a9a9ec1e4072d8b5338c9835df19642765339c9722f9bbe2152cdb60c.png)  

>问题是咋转回来
>

![picture 53](../assets/images/3990d028b55708ce22a1d7b2d8dd85c2b469dbd8d3b5ca02571d4bdeb7f25189.png)  
![picture 54](../assets/images/5adddc227899713a84df025d32ff8434a65ededeeafc4b069d814f10b157bd63.png)  
![picture 55](../assets/images/3d261bbffd4e1900edc610cfd1a52a57c3de21f889ab0816cdfb39124b88d84d.png)  
![picture 56](../assets/images/0804097f42f7ff492e851275f0cdda79377575f7700e5454ee5c6f3ca49fb0f2.png)  
![picture 57](../assets/images/ef0552257a136554fe4bd5761a1974e93cb7fa2615a4296543b7f199c84596d7.png)  
![picture 58](../assets/images/4dca980bfc848f6d11a62af6db017fc9e6f7c22dbc304855eaff378162f510bd.png)  

>多此一举的感觉直接扫码给密码得了还非得开个网站：thomas:youareonthegoodwaybro
>

![picture 59](../assets/images/57b3f0c24e7f3e958db398fcc43a9af0543a4b2a0ad9117d7e2115f0283e67cf.png)  

>私钥复用前2天学的
>

![picture 60](../assets/images/e7ade42837499c815d70d60a48aac750c50cec48e1002985bfe65b93c0fecd8b.png)  

```
#!/usr/bin/env python3
from datetime import datetime
import sys
import os
from os import listdir
import re

def show_help():
    message='''
********************************************************
* Simpler   -   A simple simplifier ;)                 *
* Version 1.0                                          *
********************************************************
Usage:  python3 simpler.py [options]

Options:
    -h/--help   : This help
    -s          : Statistics
    -l          : List the attackers IP
    -p          : ping an attacker IP
    '''
    print(message)

def show_header():
    print('''***********************************************
     _                 _
 ___(_)_ __ ___  _ __ | | ___ _ __ _ __  _   _
/ __| | '_ ` _ \| '_ \| |/ _ \ '__| '_ \| | | |
\__ \ | | | | | | |_) | |  __/ |_ | |_) | |_| |
|___/_|_| |_| |_| .__/|_|\___|_(_)| .__/ \__, |
                |_|               |_|    |___/
                                @ironhackers.es

***********************************************
''')

def show_statistics():
    path = '/home/pepper/Web/Logs/'
    print('Statistics\n-----------')
    listed_files = listdir(path)
    count = len(listed_files)
    print('Number of Attackers: ' + str(count))
    level_1 = 0
    dat = datetime(1, 1, 1)
    ip_list = []
    reks = []
    ip = ''
    req = ''
    rek = ''
    for i in listed_files:
        f = open(path + i, 'r')
        lines = f.readlines()
        level2, rek = get_max_level(lines)
        fecha, requ = date_to_num(lines)
        ip = i.split('.')[0] + '.' + i.split('.')[1] + '.' + i.split('.')[2] + '.' + i.split('.')[3]
        if fecha > dat:
            dat = fecha
            req = requ
            ip2 = i.split('.')[0] + '.' + i.split('.')[1] + '.' + i.split('.')[2] + '.' + i.split('.')[3]
        if int(level2) > int(level_1):
            level_1 = level2
            ip_list = [ip]
            reks=[rek]
        elif int(level2) == int(level_1):
            ip_list.append(ip)
            reks.append(rek)
        f.close()

    print('Most Risky:')
    if len(ip_list) > 1:
        print('More than 1 ip found')
    cont = 0
    for i in ip_list:
        print('    ' + i + ' - Attack Level : ' + level_1 + ' Request: ' + reks[cont])
        cont = cont + 1

    print('Most Recent: ' + ip2 + ' --> ' + str(dat) + ' ' + req)

def list_ip():
    print('Attackers\n-----------')
    path = '/home/pepper/Web/Logs/'
    listed_files = listdir(path)
    for i in listed_files:
        f = open(path + i,'r')
        lines = f.readlines()
        level,req = get_max_level(lines)
        print(i.split('.')[0] + '.' + i.split('.')[1] + '.' + i.split('.')[2] + '.' + i.split('.')[3] + ' - Attack Level : ' + level)
        f.close()

def date_to_num(lines):
    dat = datetime(1,1,1)
    ip = ''
    req=''
    for i in lines:
        if 'Level' in i:
            fecha=(i.split(' ')[6] + ' ' + i.split(' ')[7]).split('\n')[0]
            regex = '(\d+)-(.*)-(\d+)(.*)'
            logEx=re.match(regex, fecha).groups()
            mes = to_dict(logEx[1])
            fecha = logEx[0] + '-' + mes + '-' + logEx[2] + ' ' + logEx[3]
            fecha = datetime.strptime(fecha, '%Y-%m-%d %H:%M:%S')
            if fecha > dat:
                dat = fecha
                req = i.split(' ')[8] + ' ' + i.split(' ')[9] + ' ' + i.split(' ')[10]
    return dat, req

def to_dict(name):
    month_dict = {'Jan':'01','Feb':'02','Mar':'03','Apr':'04', 'May':'05', 'Jun':'06','Jul':'07','Aug':'08','Sep':'09','Oct':'10','Nov':'11','Dec':'12'}
    return month_dict[name]

def get_max_level(lines):
    level=0
    for j in lines:
        if 'Level' in j:
            if int(j.split(' ')[4]) > int(level):
                level = j.split(' ')[4]
                req=j.split(' ')[8] + ' ' + j.split(' ')[9] + ' ' + j.split(' ')[10]
    return level, req

def exec_ping():
    forbidden = ['&', ';', '-', '`', '||', '|']
    command = input('Enter an IP: ')
    for i in forbidden:
        if i in command:
            print('Got you')
            exit()
    os.system('ping ' + command)

if __name__ == '__main__':
    show_header()
    if len(sys.argv) != 2:
        show_help()
        exit()
    if sys.argv[1] == '-h' or sys.argv[1] == '--help':
        show_help()
        exit()
    elif sys.argv[1] == '-s':
        show_statistics()
        exit()
    elif sys.argv[1] == '-l':
        list_ip()
        exit()
    elif sys.argv[1] == '-p':
        exec_ping()
        exit()
    else:
        show_help()
        exit()
```

![picture 61](../assets/images/ea537a5433916608481849868e147f407f3e0227562627ec3d78ef64d503b137.png)  

>那很简单了
>

![picture 62](../assets/images/ecebafc49bf1a0494ae912f46185c1a16ec158d2b050c1a9023b52e6f72b6cdc.png)  

>有空格问题直接${IFS}
>

![picture 63](../assets/images/b33e42f83b7ca5f81601a8b976b52f45793483572cb363975f706f04bcebda4f.png)  

>有思路了可以执行一个命令我不绕直接写一个就好了
>

![picture 64](../assets/images/13c4457ce33b59da7e7075adf416d2abfcd7571d023f92e4dd1eb63aef1c8492.png)  

>这两天这些知识点都碰到过了，哈哈哈
>

![picture 65](../assets/images/420350e7cf4bd76b5725ee8e9fe3e499a88d7b6b7f33c88183a6bd60b2b1197e.png)  

>非终端的话改一下改成创建私钥得了
>

![picture 66](../assets/images/9761e2296aaf5c37bd99c16619d82e78aaa645f3666cb3f72a6cf504ff459730.png)  
![picture 67](../assets/images/144f137d9e1e25eb23869904f452d18084f0c1e8d9da94bcca25221ec993e1f0.png)  

>有点问题，那么我直接写入文件应该可以
>

![picture 68](../assets/images/f689e9cd0a2405cd566af966b6ea9c81803232f4647a3d8647466d3b60d7e8cc.png)  
![picture 69](../assets/images/a1900745dfc652846fe92ac3652d57ba775657f73fbb0bd2e092efb7141f1885.png)  
![picture 70](../assets/images/ec57b42a33d747c9c5e4496396ba2dcbcb18cb00a3ebf2d51d7c009ed10b233f.png)  
![picture 71](../assets/images/edb310d8e15f6d68047bd34b6e0f4eb47c245f05be2232f6a835eb7593f51493.png)  
![picture 72](../assets/images/fc6ddd75b97b5f5ea55b8dd2c3658d92062cb4b8cab8e8a71535e322439a5588.png)  

```
w3m version w3m/0.5.3+git20190105, options lang=en,m17n,image,color,ansi-color,mouse,gpm,menu,cookie,ssl,ssl-verify,external-uri-loader,w3mmailer,nntp,gopher,ipv6,alarm,mark,migemo
usage: w3m [options] [URL or filename]
options:
    -t tab           set tab width
    -r               ignore backspace effect
    -l line          # of preserved line (default 10000)
    -I charset       document charset
    -O charset       display/output charset
    -B               load bookmark
    -bookmark file   specify bookmark file
    -T type          specify content-type
    -m               internet message mode
    -v               visual startup mode
    -M               monochrome display
    -N               open URL of command line on each new tab
    -F               automatically render frames
    -cols width      specify column width (used with -dump)
    -ppc count       specify the number of pixels per character (4.0...32.0)
    -ppl count       specify the number of pixels per line (4.0...64.0)
    -dump            dump formatted page into stdout
    -dump_head       dump response of HEAD request into stdout
    -dump_source     dump page source into stdout
    -dump_both       dump HEAD and source into stdout
    -dump_extra      dump HEAD, source, and extra information into stdout
    -post file       use POST method with file content
    -header string   insert string as a header
    +<num>           goto <num> line
    -num             show line number
    -no-proxy        don't use proxy
    -4               IPv4 only (-o dns_order=4)
    -6               IPv6 only (-o dns_order=6)
    -no-mouse        don't use mouse
    -cookie          use cookie (-no-cookie: don't use cookie)
    -graph           use DEC special graphics for border of table and menu
    -no-graph        use ASCII character for border of table and menu
    -s               squeeze multiple blank lines
    -W               toggle search wrap mode
    -X               don't use termcap init/deinit
    -title[=TERM]    set buffer name to terminal title string
    -o opt=value     assign value to config option
    -show-option     print all config options
    -config file     specify config file
    -help            print this usage message
    -version         print w3m version
    -reqlog          write request logfile
    -debug           DO NOT USE
```

![picture 73](../assets/images/f39e4acdb9f758d95499a3bc74cb1fd95683f86574365ee0ab742dfe21231c3b.png)  

>用-v
>

![picture 75](../assets/images/a2fc6894a5e9d06333c8a9862c86dbd17acd903613874803001f43482ac8d9eb.png)  

![picture 74](../assets/images/72eaf6d77d38d1265a37abfec22cbf34e918886f5895e6131f34132afcd8e981.png)  

![picture 76](../assets/images/eeeb6a49ae92b499d406e633cf55d00ce905720ddbdb353ae1d675e02e43893d.png)  


```
#!/usr/bin/env python

import sys
import time
import subprocess
import datetime
import re


digits_re = re.compile("([0-9eE.+]*)")
to = 2.0
CLS='\033[2J\033[;H'
digit_chars = set('0123456789.')


def isfloat(v):
    try:
        float(v)
    except ValueError:
        return False
    return True

def total_seconds(td):
    return (td.microseconds + (td.seconds + td.days * 24. * 3600) * 10**6) / 10**6

def main(cmd):
    prevp = []
    prevt = None

    while True:
        t0 = datetime.datetime.now()
        out = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE).communicate()[0]

        p = digits_re.split(out.decode())

        if len(prevp) != len(p):
            s = p
        else:
            s = []
            i = 0
            for i, (n, o) in enumerate(zip(p, prevp)):
                if isfloat(n) and isfloat(o) and float(n) > float(o):
                    td = t0 - prevt
                    v = (float(n) - float(o)) / total_seconds(td)
                    if v > 1000000000:
                        v, suffix = v / 1000000000., 'g'
                    elif v > 1000000:
                        v, suffix = v / 1000000., 'm'
                    elif v > 1000:
                        v, suffix = v / 1000.,'k'
                    else:
                        suffix = ''
                    s.append('\x1b[7m')
                    s.append('%*s' % (len(n), '%.1f%s/s' % (v, suffix)))
                    s.append('\x1b[0m')
                else:
                    s.append(n)
            s += n[i:]

        prefix = "%sEvery %.1fs: %s\t\t%s" % (CLS, to, ' '.join(cmd), t0)
        sys.stdout.write(prefix + '\n\n' + ''.join(s).rstrip() + '\n')
        sys.stdout.flush()

        prevt = t0
        prevp = p
        time.sleep(to)

if __name__ == '__main__':
    try:
        main(sys.argv[1:])
    except KeyboardInterrupt:
        print('Interrupted')
        sys.exit(0)
    except SystemExit:
        os._exit(0)
```

![picture 78](../assets/images/b835d2b909842bc0ab0d422e84f807d5d7bf704b44e0acabad19c4874cd3ae2b.png)  

![picture 77](../assets/images/84daecd9cb1cf8dc20436db87cbe3db3655ff225f08e5980521c51671bae1bb3.png)  

>那很简单了
>

![picture 79](../assets/images/7fb1c164922576b342ed3627b3299496b26e04c341a80c4e213bcfa210ef1f7a.png)  
![picture 80](../assets/images/8a53e51baa8e96c2d42f562327642615639c30762b82728d833b82c4226a3de2.png)  

>结束
>


>userflag:e1e4e2e00a00df7b40c5436155ab4996
>
>rootflag:84decf19261819687b63c8210cd28f7c
>