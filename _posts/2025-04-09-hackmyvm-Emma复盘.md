---
title: hackmyvm Emma靶机复盘
author: LingMj
data: 2025-04-09
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.31	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.051 seconds (124.82 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC  192.168.137.31 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-08 21:23 EDT
Nmap scan report for hogwarts.htb (192.168.137.31)
Host is up (0.015s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 4a:4c:af:92:cc:bb:99:59:d7:2f:1b:99:fb:f1:7c:f0 (RSA)
|   256 ba:0d:85:69:43:86:c1:91:7c:db:2a:1e:34:ab:68:1e (ECDSA)
|_  256 a1:ac:2c:ce:f4:07:da:96:12:74:d1:54:9e:f7:09:04 (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.94 seconds
```

## 获取webshell

![picture 0](../assets/images/9a5ba0de36a56d43a5852b1f85217bcf896eb23149882148d6039e990b899d9b.png)  
![picture 1](../assets/images/27d6d5084a4b6f46aea6fa87e0c0baa716661e80ba147d6e5411c4e98c5f0eab.png)  

>是密码还是域名
>

![picture 2](../assets/images/c023208d069023ce7da42cb620c35f6055a95e71ed7b286768ad433f5c62ffff.png)  
![picture 3](../assets/images/0315140045bd3db8b1f8ad4c3ec8bcc36100ce907066b6fd857f5cc339140d54.png)  
![picture 4](../assets/images/6e670c5e3b45e2651d2ade7dc17708e063944458e89ccb454f4d9bbc3a3b9e39.png)  
![picture 5](../assets/images/1f71e7aec98ef22beb4016550ff56a82c346d7ed46c6f83cb245636ca207f8d9.png)  
![picture 6](../assets/images/39ec72fab903c3e55dfd57d967c3ee6ec2a07b1c1df126123e0cd7537bb18846.png)  
![picture 7](../assets/images/3f53671cd4b4e2b0484748ee8492251401c13a61a7677858553c4c8bbde8833d.png)  
![picture 8](../assets/images/30d7805444f0a5870dd78fe5578d7a7ea079cb9976283a9b6f07140b0936b5d3.png)  

>有点难找，感觉差点什么换一个扫描器,不见是udp,换一下我感觉这一大长串不想是密码没准用户名
>

![picture 9](../assets/images/7f611d44e7f85213f80413d8f63a11045249297dbee893889fd59b10fcbaf267.png)  
![picture 10](../assets/images/a4203ea5f1c85592fce08d2929fff8ac2e292ad1e63acab46aedecb603e834c6.png)  
![picture 11](../assets/images/7904a0334641945d14927156be657a101e444197034bde63a51f5150d8b65488.png)  

>这里有个地址：https://github.com/neex/phuip-fpizdam
>

![picture 12](../assets/images/f7f58ea14f7b0133951b25ee4b28b1340d1097388022e73b0ba40e1604905e64.png)  

>我直接msf了要安装什么的但是懒得挂东西
>

![picture 13](../assets/images/cd6b6f1e8e824d057db299f1e8e5d37aae9e2cfa21a8c550be5149b25103ad4d.png)  
![picture 14](../assets/images/654eaffd14fa0f8b38e778298b80511bcacdeb61ac2c8e57f2187942666970aa.png)  

>失败了好奇怪
>

![picture 15](../assets/images/260955b3cfab61c4e4f162ed94d821dc75d8fbbd878cfb55560294fb5cb0ab80.png)  

>我靶机bug了我怀疑，不然不应该不能使用msf
>

![picture 16](../assets/images/df88625bb3a09fe8d10ffce9edd2711911a52e226b45ae386e687ed7db11f779.png)  
![picture 17](../assets/images/437c1802e448e7112df92697b6639fe9b5bc3a744fed3cc5458f64445c39b952.png)  
![picture 18](../assets/images/21b8afed35833c36365e2fe98007a64d15bd3e6889db1fbaf46d3fe6dc3db7b6.png)  


>还是失败,重新安装靶机还是失败了,先搁置了
>

![picture 19](../assets/images/1c29fb7351f7732664ba249c4f28ced6ed4dbd475e6079e90b3872232de4d0fd.png)  
![picture 20](../assets/images/4104c529da9fccacefdbfa586ac4b7c8e26ed30ef89a6865c3896d3f4363492b.png)  

>好了多搞到东西就行了
>

![picture 21](../assets/images/7cfa0d7aa174d513aa7483c6c0d73a72d9ef6279e28ebe76601e61b5d7177255.png)  
![picture 22](../assets/images/b1777c0939dcb9e82ffe22190e35a131369063b09e7067b0c63e185d3f499e84.png)  

>成功了
>

![picture 23](../assets/images/9681b3b1dea396279c461e69b84bc6a54ec8fc358b06b7cad0fca317e9893665.png)  
![picture 24](../assets/images/26297a4451704610e71046a116418dc33ff14324b19bf77d3c5555c5c6bf7d14.png)  

>curl失败我就换web搞
>

## 提权

![picture 25](../assets/images/b2ec1ef88e633d63b7d244da967f01d1b8eea4e5cef8ef5d9c3e54e8a261b9b9.png)  
![picture 26](../assets/images/e56dc3e4f335fd061120779ccc1511ae268b10553a8a72bb080ffef613194e4a.png)  
![picture 27](../assets/images/e9f27ee5802eeab3a405f814f1b6ebab76933427f140c791779dfb65bb0a73e5.png)  

>话说为啥原来的爆破不成功
>

![picture 28](../assets/images/272ff331b5078996bf461914a6516f852eed699a81c3cdc12abb5d8de2515537.png)  

```
#!/bin/sh
# gzexe: compressor for Unix executables.
# Use this only for binaries that you do not use frequently.
# The compressed version is a shell script which decompresses itself after
# skipping $skip lines of shell commands.  We try invoking the compressed
# executable with the original name (for programs looking at their name).
# We also try to retain the original file permissions on the compressed file.
# For safety reasons, gzexe will not create setuid or setgid shell scripts.
# WARNING: the first line of this file must be either : or #!/bin/sh
# The : is required for some old versions of csh.
# On Ultrix, /bin/sh is too buggy, change the first line to: #!/bin/sh5
# Copyright (C) 1998, 2002, 2004, 2006-2007, 2010-2018 Free Software
# Foundation, Inc.
# Copyright (C) 1993 Jean-loup Gailly
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# You should have received a copy of the GNU General Public License along
# with this program; if not, write to the Free Software Foundation, Inc.,
# 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
tab='	'
nl='
IFS=" $tab$nl"
version='gzexe (gzip) 1.9
Copyright (C) 2007, 2011-2017 Free Software Foundation, Inc.
This is free software.  You may redistribute copies of it under the terms of
the GNU General Public License <https://www.gnu.org/licenses/gpl.html>.
There is NO WARRANTY, to the extent permitted by law.
Written by Jean-loup Gailly.'
usage="Usage: $0 [OPTION] FILE...
Replace each executable FILE with a compressed version of itself.
Make a backup FILE~ of the old version of FILE.
  -d             Decompress each FILE instead of compressing it.
      --help     display this help and exit
      --version  output version information and exit
Report bugs to <bug-gzip@gnu.org>."
decomp=0
res=0
while :; do
  case $1 in
  -d) decomp=1; shift;;
  --h*) printf '%s\n' "$usage"   || exit 1; exit;;
  --v*) printf '%s\n' "$version" || exit 1; exit;;
  --) shift; break;;
  *) break;;
  esac
done
if test $# -eq 0; then
  printf >&2 '%s\n' "$0: missing operand
Try \`$0 --help' for more information."
  exit 1
tmp=
trap 'res=$?
  test -n "$tmp" && rm -f "$tmp"
  (exit $res); exit $res
' 0 1 2 3 5 10 13 15
mktemp_status=
for i do
  case $i in
  -*) file=./$i;;
  *)  file=$i;;
  esac
  if test ! -f "$file" || test ! -r "$file"; then
    res=$?
    printf >&2 '%s\n' "$0: $i is not a readable regular file"
    continue
  fi
  if test $decomp -eq 0; then
    if sed -e 1d -e 2q "$file" | grep "^skip=[0-9][0-9]*$" >/dev/null; then
      printf >&2 '%s\n' "$0: $i is already gzexe'd"
      continue
    fi
  fi
  if test -u "$file"; then
    printf >&2 '%s\n' "$0: $i has setuid permission, unchanged"
    continue
  fi
  if test -g "$file"; then
    printf >&2 '%s\n' "$0: $i has setgid permission, unchanged"
    continue
  fi
  case /$file in
  */basename | */bash | */cat | */chmod | */cp | \
  */dirname | */expr | */gzip | \
  */ln | */mkdir | */mktemp | */mv | */printf | */rm | \
  */sed | */sh | */sleep | */test | */tail)
    printf >&2 '%s\n' "$0: $i might depend on itself"; continue;;
  esac
  dir=`dirname "$file"` || dir=$TMPDIR
  test -d "$dir" && test -w "$dir" && test -x "$dir" || dir=/tmp
  test -n "$tmp" && rm -f "$tmp"
  if test -z "$mktemp_status"; then
    type mktemp >/dev/null 2>&1
    mktemp_status=$?
  fi
  case $dir in
    */) ;;
    *) dir=$dir/;;
  esac
  if test $mktemp_status -eq 0; then
    tmp=`mktemp "${dir}gzexeXXXXXXXXX"`
  else
    tmp=${dir}gzexe$$
  fi && { cp -p "$file" "$tmp" 2>/dev/null || cp "$file" "$tmp"; } || {
    res=$?
    printf >&2 '%s\n' "$0: cannot copy $file"
    continue
  if test -w "$tmp"; then
    writable=1
  else
    writable=0
    chmod u+w "$tmp" || {
      res=$?
      printf >&2 '%s\n' "$0: cannot chmod $tmp"
      continue
    }
  fi
  if test $decomp -eq 0; then
    (cat <<'EOF' &&
#!/bin/sh
skip=44
tab='	'
nl='
IFS=" $tab$nl"
umask=`umask`
umask 77
gztmpdir=
trap 'res=$?
  test -n "$gztmpdir" && rm -fr "$gztmpdir"
  (exit $res); exit $res
' 0 1 2 3 5 10 13 15
case $TMPDIR in
  / | /*/) ;;
  /*) TMPDIR=$TMPDIR/;;
  *) TMPDIR=/tmp/;;
esac
if type mktemp >/dev/null 2>&1; then
  gztmpdir=`mktemp -d "${TMPDIR}gztmpXXXXXXXXX"`
else
  gztmpdir=${TMPDIR}gztmp$$; mkdir $gztmpdir
fi || { (exit 127); exit 127; }
gztmp=$gztmpdir/$0
case $0 in
-* | */*'
') mkdir -p "$gztmp" && rm -r "$gztmp";;
*/*) gztmp=$gztmpdir/`basename "$0"`;;
esac || { (exit 127); exit 127; }
case `printf 'X\n' | tail -n +1 2>/dev/null` in
X) tail_n=-n;;
*) tail_n=;;
esac
if tail $tail_n +$skip <"$0" | gzip -cd > "$gztmp"; then
  umask $umask
  chmod 700 "$gztmp"
  (sleep 5; rm -fr "$gztmpdir") 2>/dev/null &
  "$gztmp" ${1+"$@"}; res=$?
else
  printf >&2 '%s\n' "Cannot decompress $0"
  (exit 127); res=127
fi; exit $res
    gzip -cv9 "$file") > "$tmp" || {
      res=$?
      printf >&2 '%s\n' "$0: compression not possible for $i, file unchanged."
      continue
    }
  else
    # decompression
    skip=44
    skip_line=`sed -e 1d -e 2q "$file"`
    case $skip_line in
    skip=[0-9] | skip=[0-9][0-9] | skip=[0-9][0-9][0-9])
      eval "$skip_line";;
    esac
    case `printf 'X\n' | tail -n +1 2>/dev/null` in
    X) tail_n=-n;;
    *) tail_n=;;
    esac
    tail $tail_n +$skip "$file" | gzip -cd > "$tmp" || {
      res=$?
      printf >&2 '%s\n' "$0: $i probably not in gzexe format, file unchanged."
      continue
    }
  fi
  test $writable -eq 1 || chmod u-w "$tmp" || {
    res=$?
    printf >&2 '%s\n' "$0: $tmp: cannot chmod"
    continue
  ln -f "$file" "$file~" 2>/dev/null || {
    # Hard links may not work.  Fall back on rm+cp so that $file always exists.
    rm -f "$file~" && cp -p "$file" "$file~"
  } || {
    res=$?
    printf >&2 '%s\n' "$0: cannot backup $i as $i~"
    continue
  mv -f "$tmp" "$file" || {
    res=$?
    printf >&2 '%s\n' "$0: cannot rename $tmp to $i"
    continue
  tmp=
done
(exit $res); exit $res
```


>太长了扔给gtp了
>

![picture 29](../assets/images/dbb30e23507ee761211858c7942810d63784cccdce58155e635a4f1252ece363.png)  
![picture 30](../assets/images/8f6bf307f19fe349ad8352b3c606a48252396cfff7b9700607172aa185f429f8.png)  

>可以环境劫持么
>

![picture 31](../assets/images/702a21896e5b45054d647b703e833b0110f680c4c92b4899157f3e8cd446c21c.png)  
![picture 32](../assets/images/bb0417c95ba0d898431907ea6bca86b50ae9ea6c8c0a9b67180e4e08eaf2309f.png)  
![picture 33](../assets/images/149ff85eec23e0ec2b6d30de77010651268c058371becb8660726b0620552d0b.png)  
![picture 34](../assets/images/9010e3f896ccc753e78801246bf4b2a2cbdb3c071229c7853621646273d76de2.png)  
![picture 35](../assets/images/35440a28fea31fc622b23854aa03fe90c5f715cd9e283bf0f5d38cd26aee2a9a.png)  

>好了结束了，前面有点小插曲
>


>userflag:youdontknowme
>
>rootflag:itsmeimshe
>