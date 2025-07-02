---
title: Self-VM Leak复盘
author: LingMj
data: 2025-07-02
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
192.168.137.64	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.088 seconds (122.61 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sC -sV 192.168.137.64
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-01 19:58 EDT
Nmap scan report for Leak.mshome.net (192.168.137.64)
Host is up (0.0070s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Kali Linux - \xE5\xAE\x89\xE5\x85\xA8\xE6\xB8\x97\xE9\x80\x8F\xE6\xB5\x8B\xE8\xAF\x95\xE5\xB9\xB3\xE5\x8F\xB0
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.70 seconds
```

## 获取webshell

![picture 0](../assets/images/788bb8f8ae89373a21efed60836842f561033d762e691271b2b7f2631c5b3bd5.png)  
![picture 1](../assets/images/cf7eb51695219acb232a1eedc06af3927595491a591ccbbf38440f9001250add.png)  
![picture 2](../assets/images/521474f8d673c65513aa8cf54f52faea219f157e4ea6e101af1bb94bc199344b.png)  
![picture 3](../assets/images/453b317e8f627cb98c7459450a114f3893b0bc48c04368419611c6aec7d1a873.png)  
![picture 4](../assets/images/33d0adde021e86bdc800ce502ca82be9613bad34d66951f16d52ede7efe1ac1a.png)  

>出现账号密码了，先填写域名
>

![picture 5](../assets/images/4d927ce5df1e1bc3308253ba992e058467f9115df30fa274b94075794960e613.png)  
![picture 6](../assets/images/707bc4b7f41773677244db864d9b9e9c3d3cda14322f9a68ccb8bd9ca3a52212.png)  
![picture 7](../assets/images/aee609820eddb55fb60fb793c1bcd11830c38c4aa7fd6acc28cea248a0f947d4.png)  
![picture 8](../assets/images/12003fcba861f04dfd7fd4f7b97c09d96552c601035cd5189c63eb78559b18f8.png)  

>这里有如何构造这个密码的方案
>

![picture 9](../assets/images/9fc50aa1165fdedfef4c71c3b16c443d6faae0dc21c33b3efa58e9560fd096d0.png)  

>好了又密码了
>

## 提权

![picture 10](../assets/images/3e6e7efa4bf7d1c25f1b5e0274bc1e740c0044554f9cd7bc85e5ddb8407cda45.png)  
![picture 11](../assets/images/d9a99d7672609e0f40493b41d9374f428dfeefbdcdf350d10fc4d3d1420fd710.png)  

>这个东西我查了很久，然后拿了提示包里面我以为构造deb包，但是没有sudo，suid所以我一直卡住
>

```
ai@Leak:~$ dpkg --help
Usage: dpkg [<option> ...] <command>

Commands:
  -i|--install       <.deb file name>... | -R|--recursive <directory>...
  --unpack           <.deb file name>... | -R|--recursive <directory>...
  -A|--record-avail  <.deb file name>... | -R|--recursive <directory>...
  --configure        <package>... | -a|--pending
  --triggers-only    <package>... | -a|--pending
  -r|--remove        <package>... | -a|--pending
  -P|--purge         <package>... | -a|--pending
  -V|--verify [<package>...]       Verify the integrity of package(s).
  --get-selections [<pattern>...]  Get list of selections to stdout.
  --set-selections                 Set package selections from stdin.
  --clear-selections               Deselect every non-essential package.
  --update-avail [<Packages-file>] Replace available packages info.
  --merge-avail [<Packages-file>]  Merge with info from file.
  --clear-avail                    Erase existing available info.
  --forget-old-unavail             Forget uninstalled unavailable pkgs.
  -s|--status [<package>...]       Display package status details.
  -p|--print-avail [<package>...]  Display available version details.
  -L|--listfiles <package>...      List files 'owned' by package(s).
  -l|--list [<pattern>...]         List packages concisely.
  -S|--search <pattern>...         Find package(s) owning file(s).
  -C|--audit [<package>...]        Check for broken package(s).
  --yet-to-unpack                  Print packages selected for installation.
  --predep-package                 Print pre-dependencies to unpack.
  --add-architecture <arch>        Add <arch> to the list of architectures.
  --remove-architecture <arch>     Remove <arch> from the list of architectures.
  --print-architecture             Print dpkg architecture.
  --print-foreign-architectures    Print allowed foreign architectures.
  --assert-<feature>               Assert support for the specified feature.
  --validate-<thing> <string>      Validate a <thing>'s <string>.
  --compare-versions <a> <op> <b>  Compare version numbers - see below.
  --force-help                     Show help on forcing.
  -Dh|--debug=help                 Show help on debugging.

  -?, --help                       Show this help message.
      --version                    Show the version.

Assertable features: support-predepends, working-epoch, long-filenames,
  multi-conrep, multi-arch, versioned-provides.

Validatable things: pkgname, archname, trigname, version.

Use dpkg with -b, --build, -c, --contents, -e, --control, -I, --info,
  -f, --field, -x, --extract, -X, --vextract, --ctrl-tarfile, --fsys-tarfile
on archives (type dpkg-deb --help).

Options:
  --admindir=<directory>     Use <directory> instead of /var/lib/dpkg.
  --root=<directory>         Install on a different root directory.
  --instdir=<directory>      Change installation dir without changing admin dir.
  --path-exclude=<pattern>   Do not install paths which match a shell pattern.
  --path-include=<pattern>   Re-include a pattern after a previous exclusion.
  -O|--selected-only         Skip packages not selected for install/upgrade.
  -E|--skip-same-version     Skip packages whose same version is installed.
  -G|--refuse-downgrade      Skip packages with earlier version than installed.
  -B|--auto-deconfigure      Install even if it would break some other package.
  --[no-]triggers            Skip or force consequential trigger processing.
  --verify-format=<format>   Verify output format (supported: 'rpm').
  --no-debsig                Do not try to verify package signatures.
  --no-act|--dry-run|--simulate
                             Just say what we would do - don't do it.
  -D|--debug=<octal>         Enable debugging (see -Dhelp or --debug=help).
  --status-fd <n>            Send status change updates to file descriptor <n>.
  --status-logger=<command>  Send status change updates to <command>'s stdin.
  --log=<filename>           Log status changes and actions to <filename>.
  --ignore-depends=<package>,...
                             Ignore dependencies involving <package>.
  --force-...                Override problems (see --force-help).
  --no-force-...|--refuse-...
                             Stop when problems encountered.
  --abort-after <n>          Abort after encountering <n> errors.

Comparison operators for --compare-versions are:
  lt le eq ne ge gt       (treat empty version as earlier than any version);
  lt-nl le-nl ge-nl gt-nl (treat empty version as later than any version);
  < << <= = >= >> >       (only for compatibility with control file syntax).

Use 'apt' or 'aptitude' for user-friendly package management.
```

![picture 12](../assets/images/712a5acf8d7bd4fb0d4120ac3859bc55c258e00ab204e6afdc505d73660481fe.png)  
![picture 13](../assets/images/72b01a6f661f7c17245d7fc8d074fbb6b8918a5378d6d01c8052d9cb58219478.png)  

>好了结束了
>

>userflag:
>
>rootflag:
>
