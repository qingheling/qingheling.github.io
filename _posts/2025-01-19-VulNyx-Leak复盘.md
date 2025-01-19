---
title: VulNyx Leak靶机复盘
author: LingMj
data: 2025-01-19
categories: [VulNyx]
tags: [cve,look_file,ipv6,wkhtmltopdf]
description: 难度-Medium
---

## 网段扫描
```
└─# arp-scan -l
Interface: eth0, type: EN10MB, MAC: 00:0c:29:df:e2:a7, IPv4: 192.168.26.128
WARNING: Cannot open MAC/Vendor file ieee-oui.txt: Permission denied
WARNING: Cannot open MAC/Vendor file mac-vendor.txt: Permission denied
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.26.2    00:50:56:e8:d4:e1       (Unknown)
192.168.26.1    00:50:56:c0:00:08       (Unknown)
192.168.26.177  00:0c:29:a2:58:a9       (Unknown)
192.168.26.254  00:50:56:ff:4b:3d       (Unknown)

4 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 1.893 seconds (135.24 hosts/sec). 4 responded
```

## 端口扫描

```
└─# nmap -p- -sC -sV 192.168.26.177
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-19 02:15 EST
Nmap scan report for 192.168.26.177 (192.168.26.177)
Host is up (0.0010s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.56 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.56 (Debian)
8080/tcp open  http    Jetty 10.0.13
|_http-title: Panel de control [Jenkins]
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Jetty(10.0.13)
| http-robots.txt: 1 disallowed entry 
|_/
MAC Address: 00:0C:29:A2:58:A9 (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 59.48 seconds
```

## 获取Webshell
>通过扫描短裤信息可以看出这个大概率考的是某个东西的版本cve
>

![图 0](../assets/images/194863294538b985f1ad5f355f962749ab02dfd501c35dd264f74d510176d3a4.png)  
![图 1](../assets/images/f856a2eb8915b8e1d23ee7ece91c4bf6f6f8bdbedda82738878e91af85c5694f.png)  

>上web看一下就一个80端口就apache服务，可以换着看其他的
>
![图 2](../assets/images/785aff8d227dd6097046096eb5a845c61c41059066cd358a11de22e817de4320.png)  
![图 3](../assets/images/1a343376eb6da611a93e98a52b593eb0aa4b0cdc61786bbd9bfe7b093d62c30b.png)  
![图 4](../assets/images/7d409ad2e99d3b606980e05c05c1570a088f6c05745363bc4840ada1762a7a46.png)  
![图 5](../assets/images/e74975220690782a61f059b7261f15b0811ef14593f7fd4085f4efe1fc66bed8.png)  

>目前知道登录用户名可以尝试爆破
>
![图 6](../assets/images/5467a142db2b0694d44d80a3d1b30efb553fd6bafa99d16ab75f79af905f4c83.png)  

![图 7](../assets/images/672653f0f0f537c940353dd4d72f6c7d3aa6056255425ab4501661ef1d5b6736.png)  
![图 8](../assets/images/5322f0b2c4703ad81247a878c6a184708d18f16f35fc7e2d16597d6bc3883f9e.png)  

>特别慢，但是感觉应该不是去扫一下目录
>
![图 9](../assets/images/7ad8401b00953160b939be2cae3bc4eb0608378415dd91c6e4440847bb858cd9.png)  

>换个方式就有线索了
>
![图 10](../assets/images/1d686bf137776fc672e07456c1b89e9e150b69a456990c2db136c5a59844a5d2.png)  

![图 11](../assets/images/e473fdf1ffeb4579551e952c17ead3a5fc4ded76197a81f435d3747538860513.png)  

>小字典没结果，换一个大字典
>
![图 12](../assets/images/c242e9d5da9d72dff4524374a974a945f7323dc40b23615270e86db5ace46dc7.png)  
![图 13](../assets/images/f992fc23a936485a93699b3ed2458bccfb128d738b36146d0332eb8b90d0ad13.png)  

>...被坐牢半小时，看一下8080
>
![图 14](../assets/images/f8f80e34de63f4361c21eb830c721197208925968142d864c3cd04af323459fb.png)  
![图 15](../assets/images/9dac2fbd23268529f9f655a647ae014abef02677bf25282b4a25edefbe6ec696.png)  
![图 16](../assets/images/7bfbb9190b9f6923de1619cb0b2140828332909bc5c9297e5d8f2b4a27059257.png)  
![图 17](../assets/images/5fbe67418a9c8c53db02d9efc4fe1f7660c1b369ae12a3b04b1b5ca0c6fddaa9.png)  
![图 18](../assets/images/0d6f543470d8508472e83499cfa9701b70ce9018aa2b720859f9a8b4f9703e94.png)  

![图 19](../assets/images/7f58aafe997861b4e36af3a27db6d69270e992ae00c3504a79792c025b75a825.png)  
![图 20](../assets/images/e843fe270c269cbca8b088d4537b8fa632410478fca7fbf230831e83c68df284.png)  

>可用
>
![图 21](../assets/images/e3529280ff58c0301c4765d94bc4d19f8781ec2b0087eb013ed34a85102de6c6.png)  
![图 22](../assets/images/aaa90f7f7545e4414c31716d96e7e168a25e896dd5e340bb4f611c78d95c4069.png)  

>之前url路径错了，改一下能下载东西，咱看一下80端口的connect.php扫也扫了半个点
>
![图 23](../assets/images/3648e839bf62026d280a69d4b7b1c85a4e934009c8706dbde01d2a2766b00a4d.png)  

>账号密码,可以试试登录

![图 24](../assets/images/2e11fc452c4941623b86bfac3dabb4b50c4145b2b342ffa087ce8fa4f6f44322.png)  

>不行，看看ssh，不过ipv4没有ssh，可以看看ipv6
>
![图 25](../assets/images/292a18bc0043847e91f7f1e71bbd4aa3e26a2047773dfaca0e016185d21d207b.png)  

>没出来，可以看看其他方式比如目录靶机的ipv6文件，因为他有可能做了现在找不到ipv6的地址
>
![图 26](../assets/images/7e6cdbbd18eb0b2bf8f062f935c158c0a06bd67b70a5469b4b391d6fdf20993b.png)  

![图 27](../assets/images/ff9245b544ea2120996ca7c0f572f9fd74aae5f8b19dd4cda075ec15c5a831e7.png)  
![图 28](../assets/images/a0634674c255a64ad452e6c9d0fd6d3628b6405c3b2c1ab858ab610129e668d2.png)  
![图 29](../assets/images/dd95a59bf6eac209d3e43d62a1c4512d971479ba4847e76a0273b99d8d1a4edc.png)  

>不行,需要换一个方式进行
>
![图 30](../assets/images/b6097c9ef1b8fd57ecfbbcd9171d49d4146a376fd74d1a06e0489902a00319c1.png)  
![图 31](../assets/images/3a8ebd73690d5b0ad76195ac9a1f333887847f86b41df63a7f2779b73bd37057.png)  

>去查了一下这个0不能省略的
>
![图 32](../assets/images/811661b8f9d743314f4601580e059bc158f25cb1638fa09aaeaaa17531f52fa5.png)  

## 提权
```
george@leak:~$ sudo -l
Matching Defaults entries for george on leak:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User george may run the following commands on leak:
    (root) NOPASSWD: /usr/bin/wkhtmltopdf
```

```
george@leak:~$ sudo /usr/bin/wkhtmltopdf
You need to specify at least one input file, and exactly one output file
Use - for stdin or stdout

Name:
  wkhtmltopdf 0.12.6

Synopsis:
  wkhtmltopdf [GLOBAL OPTION]... [OBJECT]... <output file>
  
Document objects:
  wkhtmltopdf is able to put several objects into the output file, an object is
  either a single webpage, a cover webpage or a table of contents.  The objects
  are put into the output document in the order they are specified on the
  command line, options can be specified on a per object basis or in the global
  options area. Options from the Global Options section can only be placed in
  the global options area.

  A page objects puts the content of a single webpage into the output document.

  (page)? <input url/file name> [PAGE OPTION]...
  Options for the page object can be placed in the global options and the page
  options areas. The applicable options can be found in the Page Options and 
  Headers And Footer Options sections.

  A cover objects puts the content of a single webpage into the output document,
  the page does not appear in the table of contents, and does not have headers
  and footers.

  cover <input url/file name> [PAGE OPTION]...
  All options that can be specified for a page object can also be specified for
  a cover.

  A table of contents object inserts a table of contents into the output
  document.

  toc [TOC OPTION]...
  All options that can be specified for a page object can also be specified for
  a toc, further more the options from the TOC Options section can also be
  applied. The table of contents is generated via XSLT which means that it can
  be styled to look however you want it to look. To get an idea of how to do
  this you can dump the default xslt document by supplying the
  --dump-default-toc-xsl, and the outline it works on by supplying
  --dump-outline, see the Outline Options section.

Description:
  Converts one or more HTML pages into a PDF document, *not* using wkhtmltopdf
  patched qt.

Global Options:
      --collate                       Collate when printing multiple copies
                                      (default)
      --no-collate                    Do not collate when printing multiple
                                      copies
      --copies <number>               Number of copies to print into the pdf
                                      file (default 1)
  -H, --extended-help                 Display more extensive help, detailing
                                      less common command switches
  -g, --grayscale                     PDF will be generated in grayscale
  -h, --help                          Display help
      --license                       Output license information and exit
      --log-level <level>             Set log level to: none, error, warn or
                                      info (default info)
  -l, --lowquality                    Generates lower quality pdf/ps. Useful to
                                      shrink the result document space
  -O, --orientation <orientation>     Set orientation to Landscape or Portrait
                                      (default Portrait)
  -s, --page-size <Size>              Set paper size to: A4, Letter, etc.
                                      (default A4)
  -q, --quiet                         Be less verbose, maintained for backwards
                                      compatibility; Same as using --log-level
                                      none
      --read-args-from-stdin          Read command line arguments from stdin
      --title <text>                  The title of the generated pdf file (The
                                      title of the first document is used if not
                                      specified)
  -V, --version                       Output version information and exit

Reduced Functionality:
  This version of wkhtmltopdf has been compiled against a version of QT without
  the wkhtmltopdf patches. Therefore some features are missing, if you need
  these features please use the static version.

  Currently the list of features only supported with patch QT includes:

 * Printing more than one HTML document into a PDF file.
 * Running without an X11 server.
 * Adding a document outline to the PDF file.
 * Adding headers and footers to the PDF file.
 * Generating a table of contents.
 * Adding links in the generated PDF file.
 * Printing using the screen media-type.
 * Disabling the smart shrink feature of WebKit.

Contact:
  If you experience bugs or want to request new features please visit 
  <https://wkhtmltopdf.org/support.html>
```
![图 33](../assets/images/74fa23d746c9648fa45df54e35eb8c76f4a8596c4ba82d249f47e765cbe94382.png)  
![图 34](../assets/images/3a34a9d307570a86dfb9e9b1ac79d543749beeaac16a95caafc6799b31bc00bb.png)  

>我想到个点子，就是他如果能原格式输出文件可以进行文件覆盖
>
![图 35](../assets/images/300438ddcb06475dcaaa6b7fad641436a12473f1e704c055401fc61d619febec.png)  

>卡住了跑一下工具，看看是否有提示，找一下网页形式的文件这样可以pdf
>
![图 37](../assets/images/ed1951e5590bc17a99ded14a9110cefe47360b14ab554cebd08c15bb36937b1b.png)  

>这个是例子
>

![图 36](../assets/images/5671560b7bf3157ceca6c646b86381d4d983e3fa88182dc2b7a7e2ff945b7645.png)  

>存在定时任务，尝试获取一下
>
![图 38](../assets/images/482919e9153e02d807ea3308db1bbdf3c11e2d3b8d980d914b0dbe6ecdcf8c6e.png)  
>存在直接读取，不过需要传递出来看看pdf里面是什么
>
![图 39](../assets/images/b50c09859869a1ba38031701e6f39bf62575f5c9ae52f052cca3c15421429650.png)
![图 41](../assets/images/87e86e82b95ae80f1675e27055f371251d3265d1c5b781b6a22e72e4be796eba.png)  
![图 40](../assets/images/d0efdded3c6966f9920b5d0ac2ff427d134d299c18347e713a064f27f5d1e059.png)  

>存在私钥可以尝试一下，登录
>
![图 42](../assets/images/3c9b47993a02b81bac8d05316ebdd20a586765b7f2195c02506d9ff710fc19a4.png)  

>这里说一下这个两个pdf的原因，他是可以写/接绝对路径不用http://,http://127.0.0.1/connect.php,默认解析php，所以返回没有结果，root.txt失败的主要原因是它并不是root.txt
>
![图 43](../assets/images/ffeef3b994d8175f4a94ea31b806adde3ae3931f9fc2508b1aa36529889d54b2.png)  



>好了到这里靶场复盘就结束了
>
>userflag:f65335b64773d249e3f7372c0b79c2c6
>
>rootflag:89c441988949961e48d5085c3d70c9f1
>








