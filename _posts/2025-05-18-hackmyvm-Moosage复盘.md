---
title: hackmyvm Moosage靶机复盘
author: LingMj
data: 2025-05-18
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.83	3e:21:9c:12:bd:a3	(Unknown: locally administered)

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.059 seconds (124.33 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.83 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-18 02:56 EDT
Nmap scan report for moosage.mshome.net (192.168.137.83)
Host is up (0.064s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 02:65:e6:05:af:c8:81:9c:30:b0:da:e3:1e:d8:be:02 (RSA)
|   256 3f:7d:4b:86:8d:c7:01:8f:b3:56:6d:65:c2:e5:cf:4e (ECDSA)
|_  256 8e:d4:b8:d6:8e:d9:61:a1:3e:7f:5e:d7:ec:dc:bb:de (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-title: 403 Forbidden
|_http-server-header: nginx/1.14.2
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.85 seconds
```

## 获取webshell

![picture 0](../assets/images/c23442cf14907dec683920179c3600be41b4660f3137b1fce96c8e93b12cfd2d.png)  
![picture 1](../assets/images/9e256f263ed37d59ef0e2fb3e56360ffb5649eed96a5ed3e9fbf62e8c2ec578e.png)  
![picture 2](../assets/images/8b438c07420a661cac4689ea6c2750a284b69d862bc68bad7fb39ee146cd23cc.png)  
![picture 3](../assets/images/11a3783d8707818e972e049d8ad0866e357a51aa254ab5cc242e286961d4ed82.png)  

>不知道密码
>

![picture 4](../assets/images/38dd6eeafdf30402f8834723bb539f079926b488f89124fff68557f61e430de2.png)  
![picture 5](../assets/images/9af4a8c79cd70d1fc6e6a2d4fb3b426637c0d07b7e2af502ccc658f03f440a2b.png)  
![picture 6](../assets/images/f3f4536f7cea8972ee90ea4e5c0282dd77a01fe6da418afa003bb28b50d192ae.png)  

```
version: "3"

services:
  webserver:
    image: m1k1o/blog:latest
    container_name: blog_apache
    environment:
      TZ: Europe/Vienna
      BLOG_DB_CONNECTION: mysql
      BLOG_MYSQL_HOST: mariadb
      BLOG_MYSQL_PORT: 3306
      BLOG_MYSQL_USER: root
      BLOG_MYSQL_PASS: root
      BLOG_DB_NAME: blog
    restart: unless-stopped
    ports:
      - ${HTTP_PORT-80}:80
    volumes: 
      - ${DATA-./data}:/var/www/html/data
  mariadb:
    image: mariadb:10.1
    container_name: blog_mariadb
    environment:
      MYSQL_DATABASE: blog
      MYSQL_ROOT_PASSWORD: root
    restart: unless-stopped
    volumes:
      - mariadb:/var/lib/mysql
      - ./app/db/mysql:/docker-entrypoint-initdb.d:ro
volumes:
  mariadb:
```

>这里有一个docker imgaes
>

```
[database]
db_connection = sqlite
;sqlite_db = data/sqlite.db

;[database]
db_connection = mysql
mysql_socket = /run/mysqld/mysqld.sock
mysql_host = localhost
mysql_port = 3306
mysql_user = baca
mysql_pass = youareinsane
db_name = moosage

[profile]
title = Blog
name = Max Musermann
pic_small = static/images/profile.jpg
pic_big = static/images/profile_big.jpg
;cover = static/images/cover.jpg

[language]
lang = en

[components]
highlight = true

[custom]
theme = theme02
;header = data/header.html
;styles[] = static/styles/custom1.css
;styles[] = static/styles/custom2.css
;scripts = static/styles/scripts.css

[bbcode]
;bbtags[quote] = "<quote>{param}</quote>"

[admin]
force_login = true
nick = demo
pass = demo

[friends]
;friends[user] = pass
;friends[user] = pass

[directories]
images_path = data/i/
thumbnails_path = data/t/
logs_path = data/logs/

[proxy]
;proxy = hostname:port
;proxyauth = username:password
;proxytype = CURLPROXY_HTTP ; default, if not set
;proxytype = CURLPROXY_SOCKS4
;proxytype = CURLPROXY_SOCKS5

;URL_PREFIX type:
;proxy = http://your.page.com/proxy.cgi?
;proxyauth = username:password
;proxytype = URL_PREFIX

[system]
;timezone = Europe/Vienna
system_name = blog
version = 1.3
debug = false
logs = false
```

>还有一个config.ini
>

![picture 7](../assets/images/70cb2fe90a8db48b4acb35ec9c793242fd70db0068f6271300e0d57f8f68a641.png)  

>上面有登录的demo
>

![picture 9](../assets/images/cf3b453f91b12e5ace3ab1215a52d7b2ca77977f97fee1eeb04ccada5b179c29.png)  

![picture 8](../assets/images/636604ff0bcaae4156df59a5eec8f2a1ea2f618639e68945a8d181df1c113db9.png)  

>文件上传
>

## 提权

![picture 10](../assets/images/2013fe5f6be7c3d9b71734ae79f8aaaa82ad029aeb6af4f68021d82f107f42ac.png)  

>密码上面也有感觉这里都挺简单
>

![picture 11](../assets/images/dea75c894c763db7bcc322812f839c2be1d4f7f24944d1da8671f2631365a644.png)  
![picture 12](../assets/images/bdc172ea9696f50df3ea6ea3c8dfbbd5ba5b5e6bc02224d2faaedfc6ec06844b.png)  

>登不上
>

![picture 13](../assets/images/a4a9943e4e71146a7c3235f0b0b8e9797b3385c6cc9ec0d544ee925c989cffbc.png)  
![picture 14](../assets/images/8866f20dc45e0a2b66f968576f2053fb7765ffeeb43e682e17bffd64826e1f4e.png)  

>目前没啥有用信息推测2条路了一个suforce一个是内核了，先看内核
>

![picture 15](../assets/images/fd35c52cf889a955aa9940889c7e949bf29bfadfec428065e1c7005c3ff9b643.png)  

>无定时任务
>

![picture 16](../assets/images/85068a968cc8b19f1f676a02aab45d166126852597d3b47fe79ccd530db420a8.png)  

>内核也不见
>

![picture 17](../assets/images/3c1f54b3ebb52dd259a78ba15e983e30c599f1b1bf07e3db62fdb7739eafa154.png)  
![picture 18](../assets/images/5a97da08ff6809ce32f55777132e88af2191382db96c21bebf7bebb8e7b4e472.png)  


>把希望寄托在/usr/games/cowsay
>

![picture 19](../assets/images/5153ff7e636a6b293abee29bcbcef5f2b4c57dcd5dfcbb6441e1dbd2c7b9b12c.png)  

![picture 20](../assets/images/8aa9db930cd62578a4f70176213ce48c038792acb03f1054c6b8f601011ee4cb.png)  

>还必须私钥我以为可以密码登录
>

![picture 21](../assets/images/5c04d0fd81ee904740bfde5c3ef2ab98354af9bb4aa962adf1692ccb0a253c2b.png)  
![picture 22](../assets/images/739cc494895b1ccc2763a6254065a2b0dd8f4e00bc86361feacfe1dc0c910f1b.png)  

>果然在这里
>

![picture 23](../assets/images/bae186d2c4db25a37b7dd39de94cc12a39fb3853dbd394e8509d57674e5515e8.png)  

>一登录出现这个
>

![picture 24](../assets/images/62ec86c3eaaa3428f996f93c59f788669666b7639ba944583066ff7da7d4e0f5.png)  

>咋利用呢
>

```
#!/usr/bin/perl

##
## Cowsay 3.03
##
## This file is part of cowsay.  (c) 1999-2000 Tony Monroe.
##

use Text::Tabs qw(expand);
use Text::Wrap qw(wrap fill $columns);
use File::Basename;
use Getopt::Std;
use Cwd;
use Text::CharWidth qw(mbswidth);

if (${^UTF8LOCALE}) {
    binmode STDIN, ':utf8';
    binmode STDOUT, ':utf8';
    require Encode;
    eval { $_ = Encode::decode_utf8($_,1) } for @ARGV;
}

$version = "3.03";
$progname = basename($0);
$eyes = "oo";
$tongue = "  ";
$cowpath = $ENV{'COWPATH'} || '/usr/share/cowsay/cows';
@message = ();
$thoughts = "";

## Yeah, this is rude, I know.  But hopefully it gets around a nasty
## little version dependency.

$Text::Wrap::initial_tab = 8;
$Text::Wrap::subsequent_tab = 8;
$Text::Wrap::tabstop = 8;

## One of these days, we'll get it ported to Windows.  Yeah, right.

if (($^O eq "MSWin32") or ($^O eq "Windows_NT")) {	## Many perls, eek!
    $pathsep = ';';
} else {
    $pathsep = ':';
}

%opts = (
    'e'		=>	'oo',
    'f'		=>	'default.cow',
    'n'		=>	0,
    'T'		=>	'  ',
    'W'		=>	40,
);

getopts('bde:f:ghlLnNpstT:wW:y', \%opts);

&display_usage if $opts{'h'};
&list_cowfiles if $opts{'l'};

$borg = $opts{'b'};
$dead = $opts{'d'};
$greedy = $opts{'g'};
$paranoid = $opts{'p'};
$stoned = $opts{'s'};
$tired = $opts{'t'};
$wired = $opts{'w'};
$young = $opts{'y'};
$eyes = substr($opts{'e'}, 0, 2);
$tongue = substr($opts{'T'}, 0, 2);
$the_cow = "";

&slurp_input;
$Text::Wrap::columns = $opts{'W'};
@message = ($opts{'n'} ? expand(@message) : 
	    split("\n", fill("", "", @message)));
&construct_balloon;
&construct_face;
&get_cow;
print @balloon_lines;
print $the_cow;

sub list_cowfiles {
    my $basedir;
    my @dirfiles;
    chop($basedir = cwd);
    for my $d (split(/$pathsep/, $cowpath)) {
	print "Cow files in $d:\n";
	opendir(COWDIR, $d) || die "$0: Cannot open $d\n";
	for my $file (readdir COWDIR) {
	    if ($file =~ s/\.cow$//) {
		push(@dirfiles, $file);
	    }
	}
	closedir(COWDIR);
	print wrap("", "", sort @dirfiles), "\n";
	@dirfiles = ();
	chdir($basedir);
    }
    exit(0);
}

sub slurp_input {
    unless ($ARGV[0]) {
	chomp(@message = <STDIN>);
    } else {
	&display_usage if $opts{'n'};
	@message = join(' ', @ARGV);
    }
}

sub maxlength {
    my ($l, $m);
    $m = -1;
    for my $i (@_) {
	# $l = mbswidth $i;
        $l = mbswidth $i =~ s/\e\[\d+(?>(;\d+)*)m//gr;
	$m = $l if ($l > $m);
    }
##  maxlength patch from Jeronimo Pellegrini (Closes: #165218)
    if ($m == -1) {
	$m = 0;
    }
    return $m;
}

sub colstr {
    (my $str, my $columns) = @_;
    $str . ' ' x ($columns - mbswidth $str)
}

sub construct_balloon {
    my $max = &maxlength(@message);
    my $max2 = $max + 2;	## border space fudge.
    my $format = "%s %s %s\n";
    my @border;	## up-left, up-right, down-left, down-right, left, right
    if ($0 =~ /think/i) {
	$thoughts = 'o';
	@border = qw[ ( ) ( ) ( ) ];
    } elsif (@message < 2) {
	$thoughts = '\\';
	@border = qw[ < > ];
    } else {
	$thoughts = '\\';
	if ($V and $V gt v5.6.0) {		# Thanks, perldelta.
	    @border = qw[ / \\ \\ / | | ];
	} else {
	    @border = qw[ / \ \ / | | ];	
	}
    }
## no trailing spaces (#276144)
    push(@balloon_lines, 
	" " . ("_" x $max2) . "\n" ,
        sprintf($format, $border[0], colstr($message[0], $max), $border[1]),
	(@message < 2 ? "" :  
            map { sprintf($format, $border[4], colstr($_, $max), $border[5]) } 
		@message[1 .. $#message - 1]),
	(@message < 2 ? "" : 
            sprintf($format, $border[2], colstr($message[$#message], $max), $border[3])),
        " " . ("-" x $max2) . "\n"
    );
}

sub construct_face {
    if ($borg) { $eyes = "=="; }
    if ($dead) { $eyes = "xx"; $tongue = "U "; }
    if ($greedy) { $eyes = "\$\$"; }
    if ($paranoid) { $eyes = "@@"; }
    if ($stoned) { $eyes = "**"; $tongue = "U "; }
    if ($tired) { $eyes = "--"; } 
    if ($wired) { $eyes = "OO"; } 
    if ($young) { $eyes = ".."; }
}

sub get_cow {
##
## Get a cow from the specified cowfile; otherwise use the default cow
## which was defined above in $the_cow.
##
    my $f = $opts{'f'};
    my $full = "";
    if ($opts{'f'} =~ m,/,) {
	$full = $opts{'f'};
    } else {
	for my $d (split(/:/, $cowpath)) {
	    if (-f "$d/$f") {
		$full = "$d/$f";
		last;
	    } elsif (-f "$d/$f.cow") {
		$full = "$d/$f.cow";
		last;
	    }
	}
	if ($full eq "") {
	    die "$progname: Could not find $f cowfile!\n";
	}
    }
    do $full;
    die "$progname: $@\n" if $@;
}

sub display_usage {
	die <<EOF;
cow{say,think} version $version, (c) 1999 Tony Monroe
Usage: $progname [-bdgpstwy] [-h] [-e eyes] [-f cowfile] 
          [-l] [-n] [-T tongue] [-W wrapcolumn] [message]
EOF
}
```

>perl写的东西
>

![picture 25](../assets/images/2d9bec98ab6f3c52233fcbca88d8b2671ccf7d5c03767bb3cd57a4c294f1f3e0.png)  

>全是可写的那就是写个命令进去得了
>

![picture 26](../assets/images/92b1c3172414855caee5fb4c73488d8948f932855c54664ee7e643a5bb98faaa.png)  
![picture 27](../assets/images/d3da0bfb9ab7ede3bc0f80bcbd497c0bbee232a8a90be2eea4041c4c496b5c35.png)  

>没成功
>

![picture 28](../assets/images/dfc1a766cf55942173f670ce8321e6dd36853b273ab1d97e4bd7e284c2fc1960.png)  

>用perl操作一下
>

![picture 30](../assets/images/d52ad30dc5ec5fff16f7776e622c3e732ee7752543235ab8ff99432351b9f57e.png)  

![picture 29](../assets/images/1dc12a99c2b4ae5ad8fe731a039f2fe6a6846105b106d321a2067db8b3004d2c.png)  

>还真是这个结束了
>

![picture 31](../assets/images/5bbf5060976919a564589d62e11d4f965c040f79cf2a6785755ed602c6029263.png)  

>整体难度不难medium最多
>

>userflag:hmvmessageme
>
>rootflag:hmvyougotmooooooo
>

