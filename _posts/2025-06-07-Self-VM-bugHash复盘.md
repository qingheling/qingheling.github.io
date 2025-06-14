---
title: Self-VM bugHash复盘
author: LingMj
data: 2025-06-07
categories: [Self-VM]
tags: [upload]
description: 难度-Low
---


## 网段扫描
```
root@LingMj:~/xxoo# arp-scan -l 
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.9	62:2f:e8:e4:77:5d	(Unknown: locally administered)
192.168.137.103	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.202	a0:78:17:62:e5:0a	Apple, Inc.

9 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.099 seconds (121.96 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~/xxoo# nmap -p- -sC -sV 192.168.137.103            
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-07 05:21 EDT
Nmap scan report for lingdong.mshome.net (192.168.137.103)
Host is up (0.086s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 10.0 (protocol 2.0)
8080/tcp open  http    Node.js Express framework
| http-robots.txt: 1 disallowed entry 
|_zip2john 2026bak.zip > ziphash
|_http-title: \xE5\xA4\xA7\xE5\x82\xBB\xE5\xAD\x90\xE5\xBA\x8F\xE5\x88\x97\xE5\x8F\xB7\xE9\xAA\x8C\xE8\xAF\x81\xE7\xB3\xBB\xE7\xBB\x9F
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 32.27 seconds
```

## 获取webshell

![picture 0](../assets/images/af2992c82a7723d0d98c82dfee18148ac084ae60f902c964078d7fe80823952e.png)  
![picture 1](../assets/images/0e7ea00304d5081a05bf1c1140d004019b72a444c8d15724e419c6c0393c7eab.png)  
![picture 2](../assets/images/07d276cebd0428b1e6cdae3db3f5c9e2b413a2f1bd57fae4e6a69e26c2fc0ace.png)  

>好像是看答案的地方
>

```
const express = require('express');
const path = require('path');

const app = express();
const port = process.env.PORT || 8080;

// 解析 JSON 请求体
app.use(express.json());

// 静态文件服务
app.use(express.static('public'));

// /checkSN 路由 (POST请求)
app.post('/checkSN', (req, res) => {
    // 从请求体中获取 SN 参数
    const sn = req.body.sn;

    if (sn) {
        if (sn === "xxxxxxxxxxxxxxxxxxxxxxxxx") {
            res.json({
                code: 200,
                data: "xxxxxx:XXXXX",
                msg: 'Success: Valid SN '
            });
        } else {
            res.json({
                code: 401,
                data: null,
                msg: 'Error: Invalid SN'
            });
        }
    } else {
        res.status(400).json({
            code: 400,
            data: null,
            msg: 'Missing sn parameter in request body'
        });
    }
});
app.use((req, res) => {
    res.status(404).json({
        code: 404,
        data: null,
        msg: '404 Not Found'
    });
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}`);
}); 
```

![picture 3](../assets/images/8baa717dceaa089683b44dc6cf342d0f2947f8b62d89f457cb4ba6f2cd394336.png)  
![picture 4](../assets/images/d329bfe374a52265f34592cf6ec96e55c20980c92258dfc890e98d3625dd8aab.png)  
![picture 5](../assets/images/c062fc2ab737dce31270397041eeb337cdebaffcbf9a407033a7f7bb347f7833.png)  
![picture 6](../assets/images/a11631f81783c71004613fb8a3d32ba995632a5c9c6e89e1c536c35039aff1da.png)  
![picture 7](../assets/images/3f2f4bbae4e1cc3a08f701830ee364205ebc7f3b2987dbd639dc909fd83a8eb1.png)  
![picture 8](../assets/images/466273b45c58b53130aace5a9146e498b2d3c234216b4943619e4277d33cc957.png)  

>不是补充么
>

![picture 9](../assets/images/6c09b4c8a4d5fb7911865de6f08cf21b9becd10a00bb36623a0b6c91aab4d5fb.png)  
![picture 11](../assets/images/e6e57f40d3dfade70ccabe0e94c26b3a5ba7f20cc273058657556a7432ab0c16.png)  
![picture 10](../assets/images/05e9ce1bdd37aeeb91105113cb38c8446bd4c34f7cb0d21203fbf3a56c0d921b.png)  
![picture 12](../assets/images/1e0c073f0ab254bcfd7d7110e4b9f5c1dda9ce19e2489364463baeec3313fb7b.png)  
![picture 13](../assets/images/ec3d21076f820123632efd8464ef3fa2c9fbf648db34acb7c4850fd73bed0c5a.png)  

>这样就出来了
>

```
import hashlib
import json

KEY = "6K+35LiN6KaB5bCd6K+V5pq05Yqb56C06Kej77yM5LuU57uG55yL55yL5Yqg5a+G5rqQ5Luj56CB44CC"
VI = "Jkdsfojweflk0024564555*"

# 根据索引限制获取字符集合
a0_chars = list(KEY[:12])
b0_chars = list(KEY[:9])
e0_chars = list(KEY[:8])
f0_chars = list(KEY[:7])
z0_chars = list(VI[:6])

# 保存所有可能的hashSN
allHashSN = set()

# 遍历所有组合
for a in a0_chars:
    for b in b0_chars:
        for e in e0_chars:
            for f in f0_chars:
                for z in z0_chars:
                    final_string = a + b + e + f + z
                    hash_sn = hashlib.md5(final_string.encode()).hexdigest()
                    allHashSN.add(hash_sn)

print(f"总共生成 {len(allHashSN)} 个唯一的hashSN值")

# 将所有结果写入文件
with open('allHashSN.json', 'w') as f:
    json.dump(list(allHashSN), f, indent=2)
```

>不过我发现用户名是welcome，密码是示例序列号，哈哈哈
>

## 提权

![picture 14](../assets/images/195049dd9a51adeed2acc21ef9704fff2d15ee75e83520283706ab86061616bd.png)  

>2个提权方案
>

```
lingdong:~$ sudo /usr/bin/pnpm -h
Version 10.11.1 (compiled to binary; bundled Node.js v20.11.1)
Usage: pnpm [command] [flags]
       pnpm [ -h | --help | -v | --version ]

Manage your dependencies:
      add                  Installs a package and any packages that it depends on. By default, any new package is installed as a prod dependency
      import               Generates a pnpm-lock.yaml from an npm package-lock.json (or npm-shrinkwrap.json) file
   i, install              Install all dependencies for a project
  it, install-test         Runs a pnpm install followed immediately by a pnpm test
  ln, link                 Connect the local project to another one
      prune                Removes extraneous packages
  rb, rebuild              Rebuild a package
  rm, remove               Removes packages from node_modules and from the project's package.json
      unlink               Unlinks a package. Like yarn unlink but pnpm re-installs the dependency after removing the external link
  up, update               Updates packages to their latest version based on the specified range

Review your dependencies:
      audit                Checks for known security issues with the installed packages
      licenses             Check licenses in consumed packages
  ls, list                 Print all the versions of packages that are installed, as well as their dependencies, in a tree-structure
      outdated             Check for outdated packages

Run your scripts:
      exec                 Executes a shell command in scope of a project
      run                  Runs a defined package script
      start                Runs an arbitrary command specified in the package's "start" property of its "scripts" object
   t, test                 Runs a package's "test" script, if one was provided

Other:
      cat-file             Prints the contents of a file based on the hash value stored in the index file
      cat-index            Prints the index file of a specific package from the store
      find-hash            Experimental! Lists the packages that include the file with the specified hash.
      pack                 Create a tarball from a package
      publish              Publishes a package to the registry
      root                 Prints the effective modules directory

Manage your store:
      store add            Adds new packages to the pnpm store directly. Does not modify any projects or files outside the store
      store path           Prints the path to the active store directory
      store prune          Removes unreferenced (extraneous, orphan) packages from the store
      store status         Checks for modified packages in the store

Options:
  -r, --recursive          Run the command for each project in the workspace.
```

>感觉能直接提权
>

![picture 15](../assets/images/84416f8dd76552631377655ba9c6ffd97e4b7afa80984733ada20a1d96cfcac5.png)  
![picture 16](../assets/images/c2bbf56e3219fe64c2918b12639b5c16c649c4ccf0d555756354fd5906ed8476.png)  
![picture 17](../assets/images/7ee0e2d6a4644e9b83605a8d7874f7e3e428a341e8c14cdbc91c0dcd48e1a9de.png)  
![picture 18](../assets/images/358f8cfb7900b83bb817006079bc9e651a987efb2332dbc4054f947150163892.png)  

>好了挺简单感谢LingDong大佬的靶机
>

>userflag:
>
>rootflag:
>