---
title: Self-evaluation ta0靶机复盘
author: LingMj
data: 2025-04-25
categories: [Self-evaluation]
tags: [upload]
description: 难度-Medium
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.55	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.135	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.224	3e:21:9c:12:bd:a3	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.130 seconds (120.19 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.224 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-25 06:09 EDT
Nmap scan report for ta0.mshome.net (192.168.137.224)
Host is up (0.0096s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 f6:a3:b6:78:c4:62:af:44:bb:1a:a0:0c:08:6b:98:f7 (RSA)
|   256 bb:e8:a2:31:d4:05:a9:c9:31:ff:62:f6:32:84:21:9d (ECDSA)
|_  256 3b:ae:34:64:4f:a5:75:b9:4a:b9:81:f9:89:76:99:eb (ED25519)
5000/tcp open  http    Werkzeug httpd 3.1.3 (Python 3.9.2)
|_http-title: \xE5\x9C\xA8\xE7\xBA\xBF\xE5\x9B\xBE\xE7\x89\x87\xE8\xBD\xAC Base64
|_http-server-header: Werkzeug/3.1.3 Python/3.9.2
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.41 seconds
```

## 获取webshell
![picture 0](../assets/images/2e3d3fecc5ca77e082edaf9f5dee53d258a35050b485838a668071aef31dac11.png)  

>文件上传转换么
>

![picture 1](../assets/images/68e78a0d28d3f8b323f916fa3b992a5067736ae6396f76c98dd32badf0e3350b.png)  
![picture 2](../assets/images/04b00e563fb3c2deaa70d1ba17aa4129cd7a627fb148736f4420353a510df86a.png)  
![picture 3](../assets/images/73e61156e730c109f3092a315450554541a3d23ef0aaf9c28482efb546acd13e.png)  

>哈？这样也不行要base64转么
>

![picture 4](../assets/images/b8dc30194fecf682f0fbdc0dd28e80bba35092137a689fb759034c3aeb2d980d.png)  
![picture 5](../assets/images/c5ce01d828d6924b6274e49df268f7a5f50a537daab05f61b4a39a079d7da8e4.png)  

>这个页面没东西
>

![picture 6](../assets/images/20430fc61ad44d7e980022f285e972a7f82367abec562cca8faabdc33f79c3f6.png)  

>单纯宣传么，哈哈哈
>

![picture 7](../assets/images/2070b1f542ce89685aef5544e2eeba3cb94c6b26099480302ca74376b74c3105.png)  
![picture 8](../assets/images/ba26b8a5d9795ae7241f200288208ad2a8eda1e2a287075667db18ac477b7f6c.png)  

>传啥都no hack啥玩意感觉要做不出啦,看wp去了可以|执行命令看来太久不打已经不转了大脑
>

![picture 9](../assets/images/d7b618fc9bf7df87d9b21cf1970962e5d46eafb5ba0800399023a073f8da4143.png)  
![picture 10](../assets/images/35cc8fb77c52a67cd4372982633823d6d5a083fe7c5284de4d82bd275d5bb392.png)  

![picture 11](../assets/images/73b75889c3f873648397c2c06cc68245cb5c980993e8e6ca80069f637e17641a.png)  

>可用字符，所以应该可以注入
>

![picture 12](../assets/images/5a61944249d5932786a57548da7ad9f6d8ba81feb77bcfeca94de3ffdae2c68f.png)  
![picture 13](../assets/images/401b2d364a3947193da5777f850539c2cb0ea279c3678758554bc00a2f3950f4.png)  

>不能sh和bash，用$SHELL
>

![picture 14](../assets/images/2c1a233a7fa92bf93065137a09439bee9e14e9fde2a2e44f43cbd801d89dc6f7.png)  

>没成，都不行应该要进制转换
>

![picture 18](../assets/images/7e335ccf6ca4d8ceccfee86368c6e413381c57d9b509d032123021e2abd0b3e9.png)  

![picture 15](../assets/images/b4db143a30dc7b420681e9ab348a6ba025537eed87e80967a6631787eaba90cc.png)  

>差不多了
>

## 提权

![picture 16](../assets/images/5693dc22e8722c5809a5f114c6217fe9b236b10262a46ea5138f7ce40026e0c8.png)  
![picture 17](../assets/images/51c17f403015c8e0126faad3feca721e8392c5ddc3b717fa77e651af3b93b59a.png)  

```
import random
import socketserver

class MultiplicationGame(socketserver.BaseRequestHandler):
    def send(self, msg):
        self.request.sendall(msg.encode())

    def recv_input(self, prompt='', timeout=2.0):
        import socket
        self.send(prompt)
        data = b""
        self.request.settimeout(timeout)
        try:
            while True:
                part = self.request.recv(1)
                if not part:
                    break
                data += part
                if part == b'\n':
                    break
        except (socket.timeout, ConnectionResetError, BrokenPipeError):
            pass
        return data.decode(errors='ignore').strip()


    def handle(self):
        self.send("=== Welcome to the Totally Legit Multiplication Challenge ===\n")
        menu = "[1] Multiply some numbers\n[2] Get the secret flag (if you're lucky)\n"
        self.send(menu)

        while True:
            choice = self.recv_input(">> Choose your destiny: ")

            if choice == '1':
                try:
                    factor = int(self.recv_input("Give me a number to multiply: "))
                    rand_val = random.getrandbits(32)
                    result = rand_val * factor
                    self.send(f"Boom! {rand_val} * {factor} = {result}\n")
                except:
                    self.send("That's not a number! I need digits, my friend.\n")

            elif choice == '2':
                try:
                    ans = int(self.recv_input("Alright, what’s the product? "))
                    r1 = random.getrandbits(11000)
                    r2 = random.getrandbits(10000)
                    expected = r1 * r2
                    if ans == expected:
                        self.send("Congratulation,there is no real random\n")
                        with open("pass", "r") as f:
                            self.send(f"Here's your pass: {f.read()}\n")
                    else:
                        self.send(f"Nope! The actual answer was {expected}\n")
                except:
                    self.send("No funny business, just give me a number.\n")

            else:
                self.send("I don’t understand that choice. Try again.\n")

if __name__ == "__main__":
    HOST, PORT = "127.0.0.1", 4444
    with socketserver.ThreadingTCPServer((HOST, PORT), MultiplicationGame) as server:
        print(f"🔧 Server running on port {PORT} - waiting for challengers!")
        server.serve_forever()

```

![picture 19](../assets/images/d9ab4f06803750d8f72e0eb49f2d71558f83b80fb018d60b76a2afa7e5ab6787.png)  

![picture 20](../assets/images/ce70043334326c28b68f0c72077423a992ab0375be9e29d506989c021b8efb9a.png)  

>不知道咋解决这个随机数问题我看wp一下
>

```
from pwn import *
from randcrack import RandCrack
from tqdm import tqdm
rc = RandCrack()
p = remote('192.168.137.190',8080)
p.recvuntil(b'iny: ')
for i in tqdm(range(624)):
	p.sendline(b'1')
	p.sendlineafter(b'multiply: ',b'1')
	rand = p.recvline().decode().split('=')[-1]
	rand = rand.replace(' ', '')
	rc.submit(int(rand))
p.sendline(b'2')
rand1 = rc.predict_getrandbits(11000)
rand2 = rc.predict_getrandbits(10000)
p.recvuntil(b'uct? ')
p.sendline(str(rand1*rand2).encode())
print(p.recvuntil(b'\n'))
print(p.recvuntil(b'\n'))
```


![picture 21](../assets/images/1a56b76413020c30288c28a88f5cbbb6c8df37bdf5232d949ccc1d7021b4b4d0.png)  


>这个概率不高啊随机数，不是很了解没拿到直接拿密码下一步哈哈哈
>

![picture 22](../assets/images/a553cb25b736fa9c91cfc4635ac2507703d71c3575c8e1b0e28e11b2c5244ab9.png)  
![picture 23](../assets/images/f6ee48a5787e361016e78b8324abb5ec14bfcec01d14dba3fe743304382ebecb.png)  

>wp又换密码了？
>

![picture 24](../assets/images/36f98b0dd0a028a9f5cfec79f40581e9d25c00e6d7c6ebfbffdefee2a740590b.png)  

>跳一下，打下一关
>

![picture 25](../assets/images/61c332fbb591a29df19db1f5b75313c87e3650f38d146ae3af2c022fd0d2c022.png)  
![picture 26](../assets/images/194b0b282ffcaa3f93ac32f401534834c8579fc4be5c56b45ae877905d11d506.png)  
![picture 27](../assets/images/e90e6681036be4e5e9479fb23ec6099930e44b84f8ff68c6964c5ecf24ca46c2.png)  
![picture 28](../assets/images/351c36d0a91a577dfe6ff5655122283d3d6b807adc7f0ce32f3eb74d8416f61d.png)  

>等待出发漏洞,需要重启的好像因为不是循环程序
>

![picture 29](../assets/images/14e540dee52f5cb6b9fc9cf15529c0f49d15f90d5063c59c27ec39b64b4abd83.png)  
![picture 30](../assets/images/4dbe8eff3961ca9d67ff8f093ea0c211ca715f545abdb6b07116d39bb8297395.png)  

>密码没错啊还是没成功解压，结束，那个密码部分在研究了
>



>userflag:
>
>rootflag:
>