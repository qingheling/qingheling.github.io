---
title: Self-VM DevOops靶机复盘
author: LingMj
data: 2025-04-28
categories: [VulNyx]
tags: [upload]
description: 难度-Easy
---

## 网段扫描
```
root@LingMj:~/xxoo/jarjar# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.135	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.154	3e:21:9c:12:bd:a3	(Unknown: locally administered)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.020 seconds (126.73 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/xxoo/jarjar# nmap -p- -sV -sC 192.168.137.154
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-27 21:21 EDT
Nmap scan report for devoops.hmv.mshome.net (192.168.137.154)
Host is up (0.011s latency).
Not shown: 65534 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
3000/tcp open  ppp?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, Help, Kerberos, NCP, RPCCheck, SMBProgNeg, SSLSessionReq, TLSSessionReq, TerminalServerCookie, X11Probe: 
|     HTTP/1.1 400 Bad Request
|   FourOhFourRequest: 
|     HTTP/1.1 403 Forbidden
|     Vary: Origin
|     Content-Type: text/plain
|     Date: Mon, 28 Apr 2025 09:21:38 GMT
|     Connection: close
|     Blocked request. This host (undefined) is not allowed.
|     allow this host, add undefined to `server.allowedHosts` in vite.config.js.
|   GetRequest: 
|     HTTP/1.1 403 Forbidden
|     Vary: Origin
|     Content-Type: text/plain
|     Date: Mon, 28 Apr 2025 09:21:37 GMT
|     Connection: close
|     Blocked request. This host (undefined) is not allowed.
|     allow this host, add undefined to `server.allowedHosts` in vite.config.js.
|   HTTPOptions, RTSPRequest: 
|     HTTP/1.1 204 No Content
|     Vary: Origin, Access-Control-Request-Headers
|     Access-Control-Allow-Methods: GET,HEAD,PUT,PATCH,POST,DELETE
|     Content-Length: 0
|     Date: Mon, 28 Apr 2025 09:21:37 GMT
|_    Connection: close
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3000-TCP:V=7.95%I=7%D=4/27%Time=680ED824%P=aarch64-unknown-linux-gn
SF:u%r(GetRequest,FE,"HTTP/1\.1\x20403\x20Forbidden\r\nVary:\x20Origin\r\n
SF:Content-Type:\x20text/plain\r\nDate:\x20Mon,\x2028\x20Apr\x202025\x2009
SF::21:37\x20GMT\r\nConnection:\x20close\r\n\r\nBlocked\x20request\.\x20Th
SF:is\x20host\x20\(undefined\)\x20is\x20not\x20allowed\.\nTo\x20allow\x20t
SF:his\x20host,\x20add\x20undefined\x20to\x20`server\.allowedHosts`\x20in\
SF:x20vite\.config\.js\.")%r(Help,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r
SF:\n\r\n")%r(NCP,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(HTTPO
SF:ptions,D2,"HTTP/1\.1\x20204\x20No\x20Content\r\nVary:\x20Origin,\x20Acc
SF:ess-Control-Request-Headers\r\nAccess-Control-Allow-Methods:\x20GET,HEA
SF:D,PUT,PATCH,POST,DELETE\r\nContent-Length:\x200\r\nDate:\x20Mon,\x2028\
SF:x20Apr\x202025\x2009:21:37\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(R
SF:TSPRequest,D2,"HTTP/1\.1\x20204\x20No\x20Content\r\nVary:\x20Origin,\x2
SF:0Access-Control-Request-Headers\r\nAccess-Control-Allow-Methods:\x20GET
SF:,HEAD,PUT,PATCH,POST,DELETE\r\nContent-Length:\x200\r\nDate:\x20Mon,\x2
SF:028\x20Apr\x202025\x2009:21:37\x20GMT\r\nConnection:\x20close\r\n\r\n")
SF:%r(RPCCheck,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(DNSVersi
SF:onBindReqTCP,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(DNSStat
SF:usRequestTCP,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(SSLSess
SF:ionReq,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(TerminalServe
SF:rCookie,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(TLSSessionRe
SF:q,1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(Kerberos,1C,"HTTP/
SF:1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(SMBProgNeg,1C,"HTTP/1\.1\x204
SF:00\x20Bad\x20Request\r\n\r\n")%r(X11Probe,1C,"HTTP/1\.1\x20400\x20Bad\x
SF:20Request\r\n\r\n")%r(FourOhFourRequest,FE,"HTTP/1\.1\x20403\x20Forbidd
SF:en\r\nVary:\x20Origin\r\nContent-Type:\x20text/plain\r\nDate:\x20Mon,\x
SF:2028\x20Apr\x202025\x2009:21:38\x20GMT\r\nConnection:\x20close\r\n\r\nB
SF:locked\x20request\.\x20This\x20host\x20\(undefined\)\x20is\x20not\x20al
SF:lowed\.\nTo\x20allow\x20this\x20host,\x20add\x20undefined\x20to\x20`ser
SF:ver\.allowedHosts`\x20in\x20vite\.config\.js\.");
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 23.39 seconds
```

## 获取webshell
![picture 0](../assets/images/4dd2b9bc601266e4a1ebd9908bf6887be4d62ebe2820faf260fd41668aa61ef0.png)  

>这个靶机对我还是很有难度的，哈哈哈
>

![picture 1](../assets/images/bd976925ab66fa920fcc7e0d6d74cfe973d93079ebb5657b0c1ab690f4777d5a.png)  
![picture 2](../assets/images/a007c533751af175ba3cfb791df429e75562e9b78f0d09513a17917b9756ba05.png)  

>没扫出对应的东西，更新gobuster没用
>

![picture 3](../assets/images/e9f8208d72674edeb1ba1ef55986b1c763685b78755b2aa24e644555b7b61744.png)  
![picture 4](../assets/images/ae352b38dae1c2d7a9218f0fe6d7c245b3a67987d712109e6ad6d6b1603a88a0.png)  
![picture 5](../assets/images/339737c44f23958a912d9a21df8c5606a713c20c618188dbd6f8a9f382524a47.png)  

>找一下密码
>

![picture 6](../assets/images/06e4a4972c4faa2fbe24f58c51e2db027db45f7e259d3716d014834f598acdca.png)  
![picture 7](../assets/images/6bbbc44d8fd9702b3eda501bd9c1780b248f3fe808ff6a4523c846e77ae8070b.png)  
![picture 8](../assets/images/ae299e2b056b821677017c71cb9eb50df4e5d856d82b4e8e2581e37ab81cefe1.png)  
![picture 9](../assets/images/851a41e0ba8cea58579fe240a3f487097f1d6b74f85014814652abfd5e5475fb.png)  
![picture 10](../assets/images/abeb419975df38a5a0230aab479a7a9802cfad54dea9a3234c89605c20e1d247.png)  

![picture 11](../assets/images/4e347712d6f680fbf7d57a2d71d746390a12993a5abe437bbc3349e6e7fdd53a.png)  

>确实可读任意文件
>

![picture 12](../assets/images/57df84f9425525c3aaf7e851ae7c696fb0e28224ccbce53c4b928255eb420a2e.png)  

>还能读什么
>

![picture 13](../assets/images/3b8f491bc47391b9eb4a83ed244e1043b1e2b81529ed50366ab913138226247a.png)  

>这个是提示？
>

![picture 14](../assets/images/7d089726d23359cafc2cd2da2da25680091645010eeca69f824bb3c57167227b.png)  

>太多不好看
>

![picture 15](../assets/images/8ae360584acda64270d09621ca0fd8c0484efe58250ecfcec72efd2626483b4c.png)  

>浅浅读一下server.js,一个认证，认证✅可以命令执行
>

```
import express from 'express';
import jwt from 'jsonwebtoken';
import 'dotenv/config'
import { exec } from 'child_process';
import { promisify } from 'util';

const app = express();

const address = 'localhost';
const port = 3001;

const exec_promise = promisify(exec);

const COMMAND_FILTER = process.env.COMMAND_FILTER
    ? process.env.COMMAND_FILTER.split(',')
        .map(cmd => cmd.trim().toLowerCase())
        .filter(cmd => cmd !== '')
    : [];

app.use(express.json());

function is_safe_command(cmd) {
    if (!cmd || typeof cmd !== 'string') {
        return false;
    }
    if (COMMAND_FILTER.length === 0) {
        return false;
    }

    const lower_cmd = cmd.toLowerCase();

    for (const forbidden of COMMAND_FILTER) {
        const regex = new RegExp(`\\\\b${forbidden.replace(/[.*+?^${}()|[\\]\\\\]/g, '\\\\$&')}\\\\b|^${forbidden.replace(/[.*+?^${}()|[\\]\\\\]/g, '\\\\$&')}$`, 'i');
        if (regex.test(lower_cmd)) {
            return false;
        }
    }

    if (/[;&|]/.test(cmd)) {
        return false;
    }
    if (/[<>]/.test(cmd)) {
        return false;
    }
    if (/[`$()]/.test(cmd)) {
        return false;
    }

    return true;
}

async function execute_command_sync(command) {
    try {
        const { stdout, stderr } = await exec_promise(command);

        if (stderr) {
            return { status: false, data: { stdout, stderr } };
        }
        return { status: true, data: { stdout, stderr } };
    } catch (error) {
        return { status: true, data: error.message };
    }
}

app.get('/', (req, res) => {
    return res.json({
        'status': 'working',
        'data': `listening on http://${address}:${port}`
    })
})

app.get('/api/sign', (req, res) => {
    return res.json({
        'status': 'signed',
        'data': jwt.sign({
            uid: -1,
            role: 'guest',
        }, process.env.JWT_SECRET, { expiresIn: '1800s' }),
    });
});

app.get('/api/execute', async (req, res) => {
    const authorization_header_raw = req.headers['authorization'];
    if (!authorization_header_raw || !authorization_header_raw.startsWith('Bearer ')) {
        return res.status(401).json({
            'status': 'rejected',
            'data': 'permission denied'
        });
    }

    const jwt_raw = authorization_header_raw.split(' ')[1];

    try {
        const payload = jwt.verify(jwt_raw, process.env.JWT_SECRET);
        if (payload.role !== 'admin') {
            return res.status(403).json({
                'status': 'rejected',
                'data': 'permission denied'
            });
        }
    } catch (err) {
        return res.status(401).json({
            'status': 'rejected',
            'data': `permission denied`
        });
    }

    const command = req.query.cmd;

    const is_command_safe = is_safe_command(command);
    if (!is_command_safe) {
        return res.status(401).json({
            'status': 'rejected',
            'data': `this command is unsafe`
        });
    }

    const result = await execute_command_sync(command);

    return res.json({
        'status': result.status === true ? 'executed' : 'failed',
        'data': result.data
    })
});

app.listen(port, address, () => {
    console.log(`Listening on http://${address}:${port}`)
});
```

![picture 16](../assets/images/ddc7b61e7b5d273a57d1bc4f5d55d1b92cdf4c374c654b5432419b864e1611c4.png)  
![picture 17](../assets/images/a7b5ac2ff861aa1c346f357119ece71839c536f1df037c307dfdfd68966c36ae.png)  

```
JWT_SECRET='2942szKG7Ev83aDviugAa6rFpKixZzZz'
COMMAND_FILTER='nc,python,python3,py,py3,bash,sh,ash,|,&,<,>,ls,cat,pwd,head,tail,grep,xxd'
```

![picture 18](../assets/images/40a6c13f54ad774c9b21ff9096744a453c504cb6b6eb8cb45a3ce5f2f8fe8e6f.png)  

```
jwt = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOi0xLCJyb2xlIjoiYWRtaW4iLCJpYXQiOjE3NDU4MzQ5NTgsImV4cCI6MTc0NTgzNjc1OH0.FKklJqCYfNPVJFqk43p-715A5T7jVpe0HroQGMua8BA"
```

>伪造一下才行
>

![picture 19](../assets/images/08c0ad9447eabebcbeb009b4f645674a40a40ae4f9802db91f8cc3532b43938e.png)  
![picture 20](../assets/images/b274b7728ab19272d59d5576d616be242df36c8d98358694713bfeea8621b594.png)  

>得处理一下不然不能提权,没ban wget和curl看看能不能用
>

![picture 21](../assets/images/82d010fd59ad3f6b6c948d576dbfbe4d9233f303ebe6fa70c211330b6c39aaec.png)  

>没成功
>

![picture 22](../assets/images/0125ab7dfc03c2ceca2624c0cdf081af1886c44c2cde2176713f5b47c0f31f04.png)  
![picture 23](../assets/images/721775135bb1297a107d4d23f3a25fe4137021ab55bc34fdc13aa202916006a1.png)  
![picture 24](../assets/images/4dcfd23fa2d22e33d71b48937ff455db751b3df60460fae7a5a6e1f3ae2dda2a.png)  
![picture 25](../assets/images/dd756878449a6461cecdae9970e5613bc8c14a03373e331b56f2afb431b89e64.png)  

>奇怪为啥都不成功呢
>
![picture 26](../assets/images/3eb558829f81a42c661b59d3f43dd276147d3419676c27b6f7ecae28322347d9.png)  
![picture 27](../assets/images/e87fe50981ac4c34dce1a1a24945e817b6946b760b3aa1be0115b06d194913b7.png)  

>换nc都ash成功了
>

![picture 28](../assets/images/b5cf63b38b0ed2c6516702c0f8ff811c2d415ef010c1cc6ba6656e4e9399254d.png)  
![picture 29](../assets/images/e904991cef45468ec3cd456baf2c2657f907640e226af233dd21537cfdc63f0a.png)  

>完了这个靶机寄了，哈哈哈，搁置一下
>



## 提权



>userflag:
>
>rootflag:
>