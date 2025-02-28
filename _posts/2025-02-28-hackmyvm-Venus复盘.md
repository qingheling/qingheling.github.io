---
title: hackmyvm Venus复盘
author: LingMj
data: 2025-02-28
categories: [hackmyvm]
tags: [HMVLabs]
description: 难度-Medium
---

>这是来自hackmyvm的环境游戏，现在来完成整个venus的复盘
>

```
Host: venus.hackmyvm.eu
Port: 5000
User: hacker
Pass: havefun!
```

## Venus01

```
hacker@venus:~$ ls -la
total 44
drwxr-x--- 1 root   hacker 4096 Apr  5  2024 .
drwxr-xr-x 1 root   root   4096 Apr  5  2024 ..
-rw-r----- 1 root   hacker   31 Apr  5  2024 ...
-rw-r--r-- 1 hacker hacker  220 Apr 23  2023 .bash_logout
-rw-r--r-- 1 hacker hacker 3526 Apr 23  2023 .bashrc
-rw-r----- 1 root   hacker   16 Apr  5  2024 .myhiddenpazz
-rw-r--r-- 1 hacker hacker  807 Oct  8 18:39 .profile
-rw-r----- 1 root   hacker  287 Apr  5  2024 mission.txt
-rw-r----- 1 root   hacker 2542 Apr  5  2024 readme.txt
hacker@venus:~$ cat .myhiddenpazz 
Y1o645M3mR84ejc
hacker@venus:~$ cat mission.txt 
################
# MISSION 0x01 #
################

## EN ##
User sophia has saved her password in a hidden file in this folder. Find it and log in as sophia.

## ES ##
La usuaria sophia ha guardado su contraseña en un fichero oculto en esta carpeta.Encuentralo y logueate como sophia.
hacker@venus:~$ cat readme.txt 

# EN
Hello hax0r,
Welcome to the HMVLab Chapter 1: Venus!
This is a CTF for beginners where you can practice your skills with Linux and CTF
so lets start! :)
First of all, the home of each user is in /pwned/USER and in it you will find a file called mission.txt which will contain
the mission to complete to get the password of the next user.
It will also contain the flagz.txt file, which if you are registered at https://hackmyvm.eu you can enter to participate in the ranking (optional).
And for a bit of improvisation, there are secret levels and hidden flags: D
You will not have write permissions in most folders so if you need to write a script or something
use /tmp folder, keep in mind that it is frequently deleted ...

And last (and not least) some users can modify the files that are in the
folder /www, these files are accessible from http://venus.hackmyvm.eu so if you get a user
that can modify the file /www/hi.txt, you can put a message and it will be reflected in http://venus.hackmyvm.eu/hi.txt. 

If you have questions/ideas or want to comment us anything you can join
to our Discord: https://discord.gg/DxDFQrJ

Remember there are more people playing so be respectful.
Hack & Fun! 

# ES
Hola hax0r,
Bienvenid@ al HMVLab Chapter 1: Venus!
Este es un CTF para principiantes donde podras practicar tus habilidades con Linux y los CTF
asi que vamos a trastear un poco! :)
Antes de nada, el home de cada usuario se encuentra en /pwned/USUARIO y en el encontraras un fichero llamado mission.txt el cual contendra
la mision a completar para conseguir la password del siguiente usuario.
Tambien contendra el fichero flagz.txt, que si estas registrado en https://hackmyvm.eu podras introducir para participar en el ranking (opcional).
Y para que haya un poco de improvisacion, hay niveles secretos y flags escondidas :D
No tendras permisos de escritura en la mayoria de carpetas asi que si necesitas escribir algun script o algo
usa la carpeta /tmp, ten en cuenta que es eliminada de manera frecuente...

Y por ultimo (y no menos importante) algunos usuarios pueden modificar los ficheros que estan en la 
carpeta /www, estos ficheros son accesibles desde http://venus.hackmyvm.eu asi que si consigues un usuario
que pueda modificar el fichero /www/hi.txt, podras poner un mensaje y se verá reflejado en http://venus.hackmyvm.eu/hi.txt.

Si tienes dudas/ideas o quieres comentar cualquier cosa puedes unirte 
a nuestro Discord: https://discord.gg/DxDFQrJ

Recuerda que hay mas gente jugando asi que se respetuoso.
Hack & Fun! 
hacker@venus:~$ su - sophia
Password: 
sophia@venus:~$ 
```

>隐藏用户密码，用户为：sophia， 密码为：Y1o645M3mR84ejc，flag：8===LUzzNuv8NB59iztWUIQS===D~~
>



## Venus02

```
sophia@venus:~$ ls -al
total 36
drwxr-x--- 1 root   sophia 4096 Apr  5  2024 .
drwxr-xr-x 1 root   root   4096 Apr  5  2024 ..
-rw-r--r-- 1 sophia sophia  220 Apr 23  2023 .bash_logout
-rw-r--r-- 1 sophia sophia 3526 Apr 23  2023 .bashrc
-rw-r--r-- 1 sophia sophia  807 Apr 23  2023 .profile
-rw-r----- 1 root   sophia   31 Apr  5  2024 flagz.txt
-rw-r----- 1 root   sophia  359 Apr  5  2024 mission.txt
sophia@venus:~$ cat mission.txt 
################
# MISSION 0x02 #
################

## EN ##
The user angela has saved her password in a file but she does not remember where ... she only remembers that the file was called whereismypazz.txt 

## ES ##
La usuaria angela ha guardado su password en un fichero pero no recuerda donde... solo recuerda que el fichero se llamaba whereismypazz.txt
```

>存在一个文件我们需要找到它
>

```
sophia@venus:~$ find / -name 'whereismypazz.txt' 2>/dev/null
/usr/share/whereismypazz.txt
sophia@venus:~$ cat /usr/share/whereismypazz.txt
oh5p9gAABugHBje
sophia@venus:~$ su - angela
Password: 
angela@venus:~$ id
uid=1003(angela) gid=1003(angela) groups=1003(angela),1054(www3)
```

>用户：angela， 密码：oh5p9gAABugHBje ， flag:8===SjMYBmMh4bk49TKq7PM8===D~~
>



## Venus03

```
angela@venus:~$ cat mission.txt 
################
# MISSION 0x03 #
################

## EN ##
The password of the user emma is in line 4069 of the file findme.txt

## ES ##
La password de la usuaria emma esta en la linea 4069 del fichero findme.txt
angela@venus:~$ 
```

>我们的密码在4069行上我们可以通过head和tail处理
>

```
angela@venus:~$ cat findme.txt |head -4068|tail 
90c7b8559da5876
92031da2831bbb8
c9c80c57d4eb52a
a92d9ac1e29813e
6d91adc8f07ca55
7205b254d51cf5c
6ae44d75b2839ce
e9358b4c1ba0c2e
d04237a51d34be1
0db1473c6ad7caf
angela@venus:~$ cat findme.txt |head -4069|tail -1
fIvltaGaq0OUH8O
angela@venus:~$ su - emma
Password: 
emma@venus:~$ id
uid=1004(emma) gid=1004(emma) groups=1004(emma)
emma@venus:~$ 
```

>用户：emma ， 密码：fIvltaGaq0OUH8O ， flag：8===0daqdDlmd9XogkiHu4yq===D~~
>



## Venus04

```
emma@venus:~$ cat mission.txt 
################
# MISSION 0x04 #
################

## EN ##
User mia has left her password in the file -.
## ES ##
La usuaria mia ha dejado su password en el fichero -.
```

>这个文件在-.的目录里，我记得我能越过这个操作的方案是--但是现在没成功，所以我进行了整个目录查看我记得 -- '-' 应该可以完成
>

```
emma@venus:~$ cat /pwned/emma/*
iKXIYg0pyEH2Hos
8===0daqdDlmd9XogkiHu4yq===D~~
################
# MISSION 0x04 #
################

## EN ##
User mia has left her password in the file -.
## ES ##
La usuaria mia ha dejado su password en el fichero -.
emma@venus:~$ 
```

>用户：mia ， 密码：iKXIYg0pyEH2Hos ， flag：8===FBMdY8hel2VMA3BaYJin===D~~
>

## Venus05

```
mia@venus:~$ cat mission.txt 
################
# MISSION 0x05 #
################

## EN ##
It seems that the user camila has left her password inside a folder called hereiam 

## ES ##
Parece que la usuaria camila ha dejado su password dentro de una carpeta llamada hereiam
mia@venus:~$ 
```

>密码放在一个在hereiam的文件夹里，我们还是使用find去找一下
>

```
mia@venus:~$ find / -name  'hereiam' 2>/dev/null
/opt/hereiam
mia@venus:~$ cat /opt/hereiam/*
cat: '/opt/hereiam/*': No such file or directory
mia@venus:~$ cat /opt/hereiam  
cat: /opt/hereiam: Is a directory
mia@venus:~$ cd /opt/hereiam/
mia@venus:/opt/hereiam$ ls -al
total 12
drwxr-xr-x 2 root root 4096 Apr  5  2024 .
drwxr-xr-x 1 root root 4096 Apr  5  2024 ..
-rw-r--r-- 1 root root   16 Apr  5  2024 .here
mia@venus:/opt/hereiam$ cat .here 
F67aDmCAAgOOaOc
mia@venus:/opt/hereiam$ 
```

>用户：camila ， 密码：F67aDmCAAgOOaOc ， flag： 8===iDIi5sm1mDuqGmU5Psx6===D~~
>


## Venus06

```
camila@venus:~$ cat mission.txt 
################
# MISSION 0x06 #
################

## EN ##
The user luna has left her password in a file inside the muack folder. 

## ES ##
La usuaria luna ha dejado su password en algun fichero dentro de la carpeta muack.
```

>密码在一个叫muack的文件夹里，还是继续find
>

```
camila@venus:~$ find / -name 'muack' 2>/dev/null
/pwned/camila/muack
/pwned/camila/muack/111/111/muack
camila@venus:~$ cd /pwned/camila/muack
camila@venus:~/muack$ ls -al
camila@venus:~/muack$ cd 111
camila@venus:~/muack/111$ ls -al
camila@venus:~/muack/111$ cd 11
11/  110/ 111/ 112/ 113/ 114/ 115/ 116/ 117/ 118/ 119/ 
camila@venus:~/muack/111$ cd 11
11/  110/ 111/ 112/ 113/ 114/ 115/ 116/ 117/ 118/ 119/ 
camila@venus:~/muack/111$ cd 111/
camila@venus:~/muack/111/111$ ls -al
total 12
drwxr-xr-x   2 root root   4096 Apr  5  2024 .
drwxr-xr-x 152 root root   4096 Apr  5  2024 ..
-rw-r-----   1 root camila   16 Apr  5  2024 muack
camila@venus:~/muack/111/111$ cat muack 
j3vkuoKQwvbhkMc
```

>用户：luna ，密码：j3vkuoKQwvbhkMc ， flag： 8===KCO34FpIq3nBmHbyZvFh===D~~
>


## Venus07

```
luna@venus:~$ cat mission.txt 
################
# MISSION 0x07 #
################

## EN ##
The user eleanor has left her password in a file that occupies 6969 bytes. 

## ES ##
La usuaria eleanor ha dejado su password en un fichero que ocupa 6969 bytes.
```

>密码存放在对应6969数据大小的文件里
>

```
luna@venus:~$ ls -al
total 32
drwxr-x--- 2 root luna 4096 Apr  5  2024 .
drwxr-xr-x 1 root root 4096 Apr  5  2024 ..
-rw-r--r-- 1 luna luna  220 Apr 23  2023 .bash_logout
-rw-r--r-- 1 luna luna 3526 Apr 23  2023 .bashrc
-rw-r--r-- 1 luna luna  807 Apr 23  2023 .profile
-rw-r----- 1 root luna   31 Apr  5  2024 flagz.txt
-rw-r----- 1 root luna  224 Apr  5  2024 mission.txt
luna@venus:~$ find / -name 'occupies' 2>/dev/null
luna@venus:~$ find / -type f 6969c 2>/dev/null
luna@venus:~$ find / f -type  -size 6969c 2>/dev/null
luna@venus:~$ find / -type f -size 6969c 2>/dev/null
/usr/share/moon.txt
luna@venus:~$ cat /usr/share/moon.txt
UNDchvln6Bmtu7b
luna@venus:~$ 
```

>用户：eleanor ， 密码：UNDchvln6Bmtu7b ， flag：8===Iq5vbyiQl4ipNrLDArjD===D~~
>


## Venus08

```
eleanor@venus:~$ cat mission.txt 
################
# MISSION 0x08 #
################

## EN ##
The user victoria has left her password in a file in which the owner is the user violin. 

## ES ##
La usuaria victoria ha dejado su password en un fichero en el cual el propietario es el usuario violin.
eleanor@venus:~$ 
```

>是violin的文件并且我们可以读
>

```
eleanor@venus:~$ find / -group 'violin' 2>/dev/null
/pwned/violin
eleanor@venus:~$ ls -al/pwned/violin
ls: invalid option -- '/'
Try 'ls --help' for more information.
eleanor@venus:~$ ls -al /pwned/violin
ls: cannot open directory '/pwned/violin': Permission denied
eleanor@venus:~$ find / -user 'violin' 2>/dev/null
/usr/local/games/yo
eleanor@venus:~$ cd /usr/local/games/yo
-bash: cd: /usr/local/games/yo: Not a directory
eleanor@venus:~$ cat /usr/local/games/yo
pz8OqvJBFxH0cSj
eleanor@venus:~$ su - victoria 
Password: 
victoria@venus:~$ cat flagz.txt 
8===NWyTFi9LLqVsZ4OnuZYN===D~~
victoria@venus:~$ id
uid=1009(victoria) gid=1009(victoria) groups=1009(victoria)
```

>用户：victoria ， 密码：pz8OqvJBFxH0cSj ， flag：8===NWyTFi9LLqVsZ4OnuZYN===D~~
>

## Venus09

```
victoria@venus:~$ cat mission.txt 
################
# MISSION 0x09 #
################

## EN ##
The user isla has left her password in a zip file.

## ES ##
La usuaria isla ha dejado su password en un fichero zip.
victoria@venus:~$ 
```

>在zip里面但是我们无法直接目录下创建文件所以导致无法进行操作，可以去tmp，声明一个文件夹叫/var/tmp和tmp一样但是/var/tmp文件夹具有更多的权限，所以我们直接去/var/tmp
>

```
victoria@venus:~$ ls -al
total 36
drwxr-x--- 2 root     victoria 4096 Apr  5  2024 .
drwxr-xr-x 1 root     root     4096 Apr  5  2024 ..
-rw-r--r-- 1 victoria victoria  220 Apr 23  2023 .bash_logout
-rw-r----- 1 root     victoria 3569 Apr  5  2024 .bashrc
-rw-r--r-- 1 victoria victoria  807 Apr 23  2023 .profile
-rw-r----- 1 root     victoria   31 Apr  5  2024 flagz.txt
-rw-r----- 1 root     victoria  179 Apr  5  2024 mission.txt
-rw-r----- 1 root     victoria  220 Apr  5  2024 passw0rd.zip
victoria@venus:~$ unzip passw0rd.zip 
Archive:  passw0rd.zip
checkdir error:  cannot create pwned
                 Permission denied
                 unable to process pwned/victoria/passw0rd.txt.
victoria@venus:~$ ls -al           
total 36
drwxr-x--- 2 root     victoria 4096 Apr  5  2024 .
drwxr-xr-x 1 root     root     4096 Apr  5  2024 ..
-rw-r--r-- 1 victoria victoria  220 Apr 23  2023 .bash_logout
-rw-r----- 1 root     victoria 3569 Apr  5  2024 .bashrc
-rw-r--r-- 1 victoria victoria  807 Apr 23  2023 .profile
-rw-r----- 1 root     victoria   31 Apr  5  2024 flagz.txt
-rw-r----- 1 root     victoria  179 Apr  5  2024 mission.txt
-rw-r----- 1 root     victoria  220 Apr  5  2024 passw0rd.zip
victoria@venus:~$ mkdir tmp
mkdir: cannot create directory 'tmp': Permission denied
victoria@venus:~$ mkdir /tmp/pass
mkdir: cannot create directory '/tmp/pass': File exists
victoria@venus:~$ mkdir /tmp/pas 
mkdir: cannot create directory '/tmp/pas': File exists
victoria@venus:~$ cd /var/tmp/
victoria@venus:/var/tmp$ ls -al
total 2080
-rw------- 1 emma     emma      12288 Dec  1 13:42 -.swp
drwxrwxrwt 1 root     root       4096 Feb 27 18:26 .
drwxr-xr-x 1 root     root       4096 Apr  5  2024 ..
-rw------- 1 sophia   sophia    28672 Feb 12 05:16 .flagz.txt.swp.swo
-rw------- 1 sophia   sophia    28672 Feb 12 05:07 .flagz.txt.swp.swp
-rw------- 1 ariel    ariel     45056 Oct 14 18:20 .goas.swp.swn
-rw------- 1 ariel    ariel     45056 Sep 29 23:31 .goas.swp.swo
-rw------- 1 ariel    ariel     28672 Jan 30 08:29 .goas.swp.swp
-rw------- 1 hacker   hacker    12288 Oct  8 19:35 .myhiddenpazz.swp
-rw------- 1 lola     lola      12288 Jan 16 13:59 .swn
-rw------- 1 angela   angela    12288 Oct  1 23:59 .swo
-rw------- 1 emma     emma          0 Sep 21 10:12 .swp
-rw-r--r-- 1 clara    clara         0 Oct 16 19:16 Hello
-rw-r--r-- 1 irene    irene        16 Feb 16 22:37 adelapass.txt
drwxr-xr-x 3 alora    alora      4096 Nov 18 10:58 alora
drwxr-xr-x 2 ariel    ariel      4096 Nov 28 15:24 ariel
-rw-r----- 1 ariel    ariel     12288 Oct  5 18:11 c0.swp
drwxr-xr-x 3 clara    clara      4096 Oct 16 20:02 clara
-rwxr-xr-x 1 clara    clara       338 Oct 17 21:31 crackpass.sh
-rw-r--r-- 1 iris     iris      12941 Oct 18 14:45 decode
-rw-r--r-- 1 irene    irene       240 Feb 20 00:25 decrypted.mp4
-rw-r--r-- 1 maia     maia      10816 Feb  9 19:31 dict.txt
-rw-r--r-- 1 freya    freya        13 Jan 13 01:55 dontReadMe
drwxr-xr-x 2 iris     iris       4096 Oct  9 09:14 eloise
-rwxr--r-- 1 lucia    lucia       427 Feb 18 05:38 filecheck.sh
-rw------- 1 angela   angela    20480 Oct 14 05:29 findme.txt.swj
-rw------- 1 angela   angela    20480 Oct 14 05:29 findme.txt.swk
-rw------- 1 angela   angela    20480 Oct 14 05:29 findme.txt.swl
-rw------- 1 angela   angela    20480 Oct 14 05:29 findme.txt.swm
-rw------- 1 angela   angela    20480 Oct 14 05:29 findme.txt.swn
-rw------- 1 angela   angela    20480 Oct 14 05:29 findme.txt.swo
-rw------- 1 angela   angela    20480 Oct  1 23:59 findme.txt.swp
-rw------- 1 sophia   sophia    12288 Oct  8 19:20 flagz.txt.swp
drwxr-xr-x 2 freya    freya      4096 Oct  9 10:51 freya
drwxr-xr-x 2 maia     maia       4096 Feb 19 19:57 gloria
-rw-r--r-- 1 maia     maia        635 Oct 15 16:35 gloria.sh
-rw------- 1 ariel    ariel     12288 Feb 27 18:27 goas.sux
-rw------- 1 ariel    ariel     12288 Feb 27 18:27 goas.suy
-rw------- 1 ariel    ariel     12288 Feb 26 20:18 goas.suz
-rw------- 1 ariel    ariel     12288 Feb 24 15:19 goas.sva
-rw------- 1 ariel    ariel     12288 Feb 23 20:09 goas.svb
-rw------- 1 ariel    ariel     12288 Feb 21 19:55 goas.svc
-rw------- 1 ariel    ariel     12288 Feb 11 21:19 goas.svd
-rw------- 1 ariel    ariel     12288 Jan 29 17:54 goas.sve
-rw------- 1 ariel    ariel     12288 Jan 29 17:54 goas.svf
-rw------- 1 ariel    ariel     12288 Jan 29 17:52 goas.svg
-rw------- 1 ariel    ariel     12288 Jan 29 17:51 goas.svh
-rw------- 1 ariel    ariel     12288 Jan 24 03:12 goas.svi
-rw------- 1 ariel    ariel     12288 Jan  9 05:48 goas.svj
-rw------- 1 ariel    ariel     12288 Jan  8 03:37 goas.svk
-rw------- 1 ariel    ariel     28672 Dec 20 20:22 goas.svl
-rw------- 1 ariel    ariel     12288 Nov 27 11:53 goas.svm
-rw------- 1 ariel    ariel     12288 Dec  9 18:41 goas.svn
-rw------- 1 ariel    ariel     12288 Nov 27 10:49 goas.svo
-rw------- 1 ariel    ariel     12288 Nov 27 10:43 goas.svp
-rw------- 1 ariel    ariel     12288 Nov 27 11:27 goas.svq
-rw------- 1 ariel    ariel     12288 Nov 27 10:39 goas.svr
-rw------- 1 ariel    ariel     12288 Nov 27 10:25 goas.svs
-rw------- 1 ariel    ariel     12288 Nov 27 08:29 goas.svt
-rw------- 1 ariel    ariel     12288 Nov 27 08:29 goas.svu
-rw------- 1 ariel    ariel     12288 Nov 24 15:19 goas.svv
-rw------- 1 ariel    ariel     12288 Nov 24 15:04 goas.svw
-rw------- 1 ariel    ariel     12288 Nov 24 15:01 goas.svx
-rw------- 1 ariel    ariel     12288 Nov  3 12:35 goas.svy
-rw------- 1 ariel    ariel     12288 Oct 31 14:40 goas.svz
-rw------- 1 ariel    ariel     12288 Oct 31 14:37 goas.swa
-rw------- 1 ariel    ariel     12288 Oct 31 12:15 goas.swb
-rw------- 1 ariel    ariel     12288 Oct 31 12:01 goas.swc
-rw------- 1 ariel    ariel     12288 Oct 13 08:05 goas.swd
-rw------- 1 ariel    ariel     12288 Oct  5 19:09 goas.swe
-rw------- 1 ariel    ariel         0 Sep 22 16:28 goas.swf
-rw------- 1 ariel    ariel         0 Sep 22 16:28 goas.swg
-rw------- 1 ariel    ariel         0 Sep 22 16:26 goas.swh
-rw------- 1 ariel    ariel         0 Sep 22 16:25 goas.swi
-rw------- 1 ariel    ariel         0 Sep 22 15:52 goas.swj
-rw------- 1 ariel    ariel         0 Sep 22 15:44 goas.swk
-rw------- 1 ariel    ariel         0 Sep 21 17:08 goas.swl
-rw------- 1 ariel    ariel     12288 Sep 16 20:27 goas.swm
-rw------- 1 ariel    ariel     12288 Sep 14 18:33 goas.swn
-rw------- 1 ariel    ariel     12288 Sep 14 18:38 goas.swo
-rw------- 1 ariel    ariel     12288 Sep  9 13:39 goas.swp
-rw-r--r-- 1 victoria victoria     16 Apr  5  2024 hey
-rw-r--r-- 1 clara    clara         0 Oct 16 19:17 jgnacioLinkedin
-rw-r--r-- 1 alexa    alexa     32249 Dec 18 18:28 lexi.txt
drwxr-xr-x 2 lola     lola       4096 Oct 10 11:09 lola
drwxr-xr-x 3 lucia    lucia      4096 Nov 25 19:27 lucia
-rw-r--r-- 1 alora    alora    486924 Feb  9 19:57 musci.txt
drwxr-xr-x 3 alora    alora      4096 Nov 18 10:54 music
-rw-r--r-- 1 alora    alora    486924 Feb  9 19:57 music.txt
-r--r--r-- 1 alora    alora       208 Nov  8 13:03 music.zip
-rwxr--r-- 1 freya    freya       156 Jan 13 18:58 my_script.sh
-rw-r--r-- 1 sophia   sophia     3570 Nov 28 20:44 new
drwxr-xr-x 3 lana     lana       4096 Oct 23 09:32 noa
-rw-r--r-- 1 lana     lana          0 Feb 16 20:29 noa.txt
-rw-r----- 1 mia      mia       16384 Feb 19 19:41 noob.txt.swp
-rw-r--r-- 1 maia     maia      10816 Feb 25 06:42 pass.txt
-rw-r--r-- 1 victoria victoria     16 Apr  5  2024 passvenus
drwxr-xr-x 3 victoria victoria   4096 Oct 29 04:19 passw0rd
-rw-r----- 1 victoria victoria    220 Nov  8 12:34 passw0rd.zip
-rw------- 1 isla     isla      16384 Oct  4 03:54 passy.swp
drwxr-xr-x 3 victoria victoria   4096 Oct  2 15:02 pwned
-rw------- 1 hacker   hacker    12288 Oct 12 17:23 readme.txt.swp
-rw-r--r-- 1 nina     nina         82 Dec 21 09:22 req.txt
-rw------- 1 freya    freya     12288 Feb 23 19:09 script.sh.swp
-rw-r--r-- 1 frida    frida      4704 Dec 10 21:30 sorted.txt
drwxr-xr-x 3 victoria victoria   4096 Dec 18 12:30 ssec
-rw-r--r-- 1 maia     maia       2758 Feb 25 06:41 suForce
-rw-r--r-- 1 irene    irene        16 Feb 20 00:30 top_secret.txt
-rw-r--r-- 1 freya    freya         0 Jan 13 01:54 u
-rw-r----- 1 lana     lana      10240 Feb 25 06:37 zip.gz
victoria@venus:/var/tmp$ cat passw0rd/pwned/victoria/passw0rd.txt 
D3XTob0FUImsoBb
victoria@venus:/var/tmp$ 
```

>用户：isla ， 密码：D3XTob0FUImsoBb ， flag：8===ZyZqc1suvGe4QlkZHFlq===D~~
>


## Venus10

```
isla@venus:~$ cat mission.txt 
################
# MISSION 0x10 #
################

## EN ##
The password of the user violet is in the line that begins with a9HFX (these 5 characters are not part of her password.). 

## ES ##
El password de la usuaria violet esta en la linea que empieza por a9HFX (sin ser estos 5 caracteres parte de su password.).
```

>用户violet的密码位于以a9HFX开头的行中（这5个字符并不属于她的密码）。
>

```
isla@venus:~$ cat passy |grep '^a9HFX'
a9HFXWKINVzNQLKLDVAc
isla@venus:~$ su - violet
Password: 
su: Authentication failure
isla@venus:~$ su - violet
Password: 
violet@venus:~$ id
uid=1011(violet) gid=1011(violet) groups=1011(violet)
violet@venus:~$ 
```

>用户：violet ， 密码：WKINVzNQLKLDVAc ， flag： 8===LzErk0qFPYJj16mNnnYZ===D~~
>


## Venus11

```
violet@venus:~$ cat mission.txt 
################
# MISSION 0x11 #
################

## EN ##
The password of the user lucy is in the line that ends with 0JuAZ (these last 5 characters are not part of her password) 

## ES ##
El password de la usuaria lucy se encuentra en la linea que acaba por 0JuAZ (sin ser estos ultimos 5 caracteres parte de su password)
```

>0JuAZ结尾但是不属于里面
>

```
violet@venus:~$ ls -al          
total 52
drwxr-x--- 2 root   violet  4096 Apr  5  2024 .
drwxr-xr-x 1 root   root    4096 Apr  5  2024 ..
-rw-r--r-- 1 violet violet   220 Apr 23  2023 .bash_logout
-rw-r--r-- 1 violet violet  3526 Apr 23  2023 .bashrc
-rw-r--r-- 1 violet violet   807 Apr 23  2023 .profile
-rw-r----- 1 root   violet 16947 Apr  5  2024 end
-rw-r----- 1 root   violet    31 Apr  5  2024 flagz.txt
-rw-r----- 1 root   violet   327 Apr  5  2024 mission.txt
violet@venus:~$ cat end|grep '0JuAZ$'
OCmMUjebG53giud0JuAZ
violet@venus:~$ su - lucy
Password: 
lucy@venus:~$ id
uid=1012(lucy) gid=1012(lucy) groups=1012(lucy)
lucy@venus:~$ cat falg
cat: falg: No such file or directory
lucy@venus:~$ cat flagz.txt 
8===AdCJ4wl8pmbhi770Xbd3===D~~
lucy@venus:~$ 
```

>用户：lucy ， 密码：OCmMUjebG53giud ， flag：8===AdCJ4wl8pmbhi770Xbd3===D~~
>

## Venus12

```
lucy@venus:~$ cat mission.txt 
################
# MISSION 0x12 #
################

## EN ##
The password of the user elena is between the characters fu and ck 

## ES ##
El password de la usuaria elena esta entre los caracteres fu y ck
```

>
>





## Venus13


## Venus14


## Venus15


## Venus16


## Venus17


## Venus18


## Venus19


## Venus20


## Venus21


## Venus22


## Venus23


## Venus24


## Venus25


## Venus26


## Venus27


## Venus28


## Venus29


## Venus30


## Venus31


## Venus32


## Venus33


## Venus34


## Venus35


## Venus36


## Venus37


## Venus38


## Venus39


## Venus40


## Venus41


## Venus42


## Venus43


## Venus44


## Venus45


## Venus46


## Venus47


## Venus48


## Venus49


## Venus50

