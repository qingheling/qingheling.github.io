---
title: hackmyvm Nowords靶机复盘
author: LingMj
data: 2025-02-03
categories: [hackmyvm]
tags: [ftp,image,les]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:13       (Unknown: locally administered)
192.168.56.100  08:00:27:fd:61:55       (Unknown)
192.168.56.127  08:00:27:a3:e0:b0       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.859 seconds (137.71 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.127        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-02 22:52 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.127
Host is up (0.00058s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
MAC Address: 08:00:27:A3:E0:B0 (Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 575.42 seconds
```

## 获取webshell

![图 0](../assets/images/0af5bbffacac9e43c8ef06192c072ab60fd309265592ee6702ddb94ea545e165.png)  
![图 1](../assets/images/2a90f69298136e8f2099f732d7ff65d777fcdac640bba92a69abf95cae143c63.png)  
![图 2](../assets/images/8325097a39c9a9e695764c7325b45693677de3099941b929f83cec9f521d39b2.png)  
![图 3](../assets/images/6ff37ad32af9c90e06ddf015621b59dacca5290559ae985a98d8fa1bc0063374.png)  

```
/ewqpoiewopqiewq
/490382490328423
/fdsnmrewrwerew
/uoijfdsijiofds
/rewjlkjsdf
/rwen908098vcxvcx
/kvjciovcuxioufhydsfdsyr
/klvcxhyvcxkljhyvcxiuzxcioyv
/oiufdsaoifndasuiofhdsa
/klhvcoixzuyvxcizoyvzxcuiyv
/pvycuxivhyzxcuivyzxiouvyzxc
/kifjdsaoipfuasoifjasipofudas
/oidfphkljerhwqlkjrheqwkjlh
/mncvmzxoiurewqioyrwqrewqrr
/oiupoiuopiuopiuioyuiyiuyio
/ghasfdhasfdhasdghasfdhgasfdasjf
/nbvnmvnbvnmzxvncvznbxcvznvzx
/vnbewqveqwbmvenqwbvrnwevrhjwefhjvwerj
/hvuixctyvcyuxgivxcyuvfxcyuvigfxc
/uihysaidouyasiudysquidhqiuodhqiudqhiodu
/hfdsioufhdsiuhvcxiuovyhcxiuvgxcivhcxbcviux
/vhsdiufyhdsuivhxcuivhuisdhfids
/jfd9s87fds89cvxyvxc789v6cx
/m98789789ds7a89d7sah98zxc78
/dsaknewiquiodusjadsa
/vcxjhkluioyfdsrew
/vcxoiufdsnkjnewq
/iouoiuvcxvcxfds
/uoihbnnmxcbmxcnbvx
/mdsaydqnfdsoiurewnh
/ioufdosijnmieowryu
/oiufdsnrewjhuiyfsd
/rewnkvoiuxvfdsfdsrwqe
/kuviosjdfiojdsifoyuewhq
/hvioxcuyiofuasdhfkjlsnafoidsy
```

![图 4](../assets/images/b4f21bd8e28550ba8444b4e3a76b28ed6c908acd44750ee5cb4e3d5f24d0e4ed.png)  



```
root@LingMj:/home/lingmj/xxoo# cat tmp|xargs|tr ' ' '\n'|tr '/' '' 
tr: when not truncating set1, string2 must be non-empty
                                                                                                                                                                                             
root@LingMj:/home/lingmj/xxoo# cat tmp|xargs|tr ' ' '\n'|tr '\/' ''
tr: when not truncating set1, string2 must be non-empty
                                                                                                                                                                                             
root@LingMj:/home/lingmj/xxoo# cat tmp|xargs|tr ' ' '\n'|sed 's/\///g'
ewqpoiewopqiewq
490382490328423
fdsnmrewrwerew
uoijfdsijiofds
rewjlkjsdf
rwen908098vcxvcx
kvjciovcuxioufhydsfdsyr
klvcxhyvcxkljhyvcxiuzxcioyv
oiufdsaoifndasuiofhdsa
klhvcoixzuyvxcizoyvzxcuiyv
pvycuxivhyzxcuivyzxiouvyzxc
kifjdsaoipfuasoifjasipofudas
oidfphkljerhwqlkjrheqwkjlh
mncvmzxoiurewqioyrwqrewqrr
oiupoiuopiuopiuioyuiyiuyio
ghasfdhasfdhasdghasfdhgasfdasjf
nbvnmvnbvnmzxvncvznbxcvznvzx
vnbewqveqwbmvenqwbvrnwevrhjwefhjvwerj
hvuixctyvcyuxgivxcyuvfxcyuvigfxc
uihysaidouyasiudysquidhqiuodhqiudqhiodu
hfdsioufhdsiuhvcxiuovyhcxiuvgxcivhcxbcviux
vhsdiufyhdsuivhxcuivhuisdhfids
jfd9s87fds89cvxyvxc789v6cx
m98789789ds7a89d7sah98zxc78
dsaknewiquiodusjadsa
vcxjhkluioyfdsrew
vcxoiufdsnkjnewq
iouoiuvcxvcxfds
uoihbnnmxcbmxcnbvx
mdsaydqnfdsoiurewnh
ioufdosijnmieowryu
oiufdsnrewjhuiyfsd
rewnkvoiuxvfdsfdsrwqe
kuviosjdfiojdsifoyuewhq
hvioxcuyiofuasdhfkjlsnafoidsy
                                                                                                                                                                                             
root@LingMj:/home/lingmj/xxoo# cat tmp|xargs|tr ' ' '\n'|sed 's/\///g' > dir
                                                                                                                                                                                             
root@LingMj:/home/lingmj/xxoo# gobuster dir -w dir -u 'http://192.168.56.127/'                     
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.56.127/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                dir
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/oiufdsnrewjhuiyfsd   (Status: 200) [Size: 58506]
Progress: 35 / 36 (97.22%)
===============================================================
Finished
===============================================================
```


![图 5](../assets/images/dedfe37a982546eae3a747adb6f4e9f19e4eab0a0ac9e82af70f371b3a10e985.png)  


>又是一个文件
>

![图 6](../assets/images/5d8800b53a76b8e306338d41216667a078f03b17ab97e58f83d46be17c5f490d.png)  
![图 7](../assets/images/a28a8c1642051ec15f91a33ec5ba40c79bc97c1a3251042242550366f89c4e35.png)  
![图 8](../assets/images/d8e8e668d1ef0b0553a55fa01b9aa17904090a9c9fb4f936d5cecf6fe66553fe.png)  
![图 9](../assets/images/2e71b0f10b0fde1e6988a6fe94b8f8ab4022710649986238a58c2ca1c2fc3327.png)  
![图 10](../assets/images/3512971e5d1646813d4465495bfc9c5ac4e561a5e27cc8e563da6ae27f6b6135.png)  

>有思路了直接进行提权爆破ftp
>

```
Brielle
Quinn
Mary
Athena
Delilah
Nevaeh
Andrea
Leilani
Jasmine
Lyla
Margaret
Alyssa
Adalyn
Arya
Norah
Isla
Piper
Ruby
Rylee
Katherine
Serenity
Willow
Sophie
Josephine
Ivy
Everly
Cora
Kaylee
Liliana
Lydia
Jade
Aubree
Maria
Arianna
Taylor
Khloe
Hadley
Kayla
Eden
Eliza
Eliana
Kylie
Peyton
Emery
Melanie
Rose
Gianna
Adalynn
Natalia
Ariel
Isabelle
Melody
Alexis
Isabel
Sydney
Juliana
Brianna
Lauren
Mackenzie
lris
Raelynn
Madeline
Bailey
Emerson
Julia
Annabelle
Faith
Valentina
Nova
Alexandra
Ximena
Clara
Vivian
Ashley
Reagan
```

```
root@LingMj:/home/lingmj/xxoo# cat tmp|tr 'A-Z' 'a-z'                       
brielle
quinn
mary
athena
delilah
nevaeh
andrea
leilani
jasmine
lyla
margaret
alyssa
adalyn
arya
norah
isla
piper
ruby
rylee
katherine
serenity
willow
sophie
josephine
ivy
everly
cora
kaylee
liliana
lydia
jade
aubree
maria
arianna
taylor
khloe
hadley
kayla
eden
eliza
eliana
kylie
peyton
emery
melanie
rose
gianna
adalynn
natalia
ariel
isabelle
melody
alexis
isabel
sydney
juliana
brianna
lauren
mackenzie
lris
raelynn
madeline
bailey
emerson
julia
annabelle
faith
valentina
nova
alexandra
ximena
clara
vivian
ashley
reagan
                                                                                                                                                                                             
root@LingMj:/home/lingmj/xxoo# cat tmp|tr 'A-Z' 'a-z' > user
```

![图 11](../assets/images/edb38acec31e748fe410694497436e77032a9614923764b0d77ce0789996bd35.png)  

![图 12](../assets/images/51ebdd35bee8821f7a9b28a4aeebf19d4047dc2c0fb593a2ba1b63bc4db080a8.png)  
![图 13](../assets/images/021ea2453b3570c2e8c2faada9868a84e606142bb90590727d610993c63e6352.png)  

>有ssh，但是没有22端口，查看ipv6
>

![图 14](../assets/images/69eb5b87ca73e3585a2c20f0b9c31ba76dfcad3a85f4caf43e01dc4c5679dc06.png)  

>无法上传到www，所以不能直接获取东西
>

![图 15](../assets/images/a4a66093afbc8108f475d3b7459450ca200183071ca8f12687972b6be526306e.png)  
![图 16](../assets/images/c257f361aa6ebb4615feef5fe703f681be210e127bfced56b568a2bd7612f57d.png)  
![图 17](../assets/images/5bab8bbb4209de3a64a77fbe00d67b08f9644f6e9a95eb4d886723361f89c48f.png)  

![图 18](../assets/images/dc87201f3db0217f326c6aa4b0842572a88f2035c73de547ed564eeb81efd594.png)  
![图 19](../assets/images/6e91c4347135d5afbf3545661b1f460c8feebc9e2523f02cfb47a3b46fcd8545.png)  
![图 20](../assets/images/7874c3bfb7bfad2f11ba78b17aade758f027cd6c599d2b1738c6ae46ef84d9a8.png)  

>有点问题
>

![图 21](../assets/images/c1fe824a5a7bf3fef20562805e7b5722d37ccb487c68196e6b114a1ff96718d5.png)  

>猜测定时任务进行操作
>

![图 22](../assets/images/8c453e94452ac0e7337b3df5c3d112222d63a2ad90a26d06e4eecf702586e579.png)  
![图 23](../assets/images/b1d37ad6c36dfa46ab6b7d2dbce2494957f1564e3bde56f8a7394b9a488e8824.png)  
![图 24](../assets/images/6033a54b3894296fa29249ea476c738a40ab970832ccc4ad133562b6e5585485.png)  


>没成功
>
![图 25](../assets/images/da4ca7829476973354da0034f897f5a0063768e08eded222a9a484f800a00170.png)  
![图 26](../assets/images/ea00728192263cc371ae4a3e3762bbe12fb4d17754ca429cca6ace8835fb51cf.png)  

>成功了
>


## 提权
```
sophie@nowords:~$ sudo -l
[sudo] password for sophie: 
Sorry, try again.
[sudo] password for sophie: 
Sorry, user sophie may not run sudo on nowords.
uid=1000(sophie) gid=1000(sophie) groups=1000(sophie),24(cdrom),30(dip),46(plugdev),132(sambashare)
```

>id,可以进行一下
>

![图 27](../assets/images/b0578b8cf5ebfc13b7aefb8c588f83319a04477133bb547e9d8fae8ae6a331fc.png)  

![图 28](../assets/images/aa4f555e792d9cf1acfe0415406c0b394a7efcadf268f9281e29fb4217be44b5.png)  

>有一个631
>

```
sophie@nowords:/home/me$ id
uid=1000(sophie) gid=1000(sophie) groups=1000(sophie),24(cdrom),30(dip),46(plugdev),132(sambashare)
sophie@nowords:/home/me$ find / -groups 'sambashare' 2>/dev/null
sophie@nowords:/home/me$ find / -group sambashare 2>/dev/null
sophie@nowords:/home/me$ find / -group dip 2>/dev/null
/etc/chatscripts
/etc/chatscripts/provider
/etc/ppp
/etc/ppp/peers
/etc/ppp/peers/provider
/usr/sbin/pppd
sophie@nowords:/home/me$ find / -group cdrom 2>/dev/null
/dev/sg0
/dev/sr0
sophie@nowords:/home/me$ find / -group plugdev 2>/dev/null
```

>算了尝试内核吧，老靶机内核提权都是很正常
>
![图 29](../assets/images/069dd1facdf460cadc5b412585b467315d9d665a7d8c4e730ba0b061c4c56190.png)  
![图 30](../assets/images/7c79cf90d605e24a95d19f84bc8dc4e09877416f51fb5540affc1fd374ce330f.png)  

>结束
>


>userflag:Icanreadyeah
>
>rootflag:Ihavenowords
>