---
title: hackmyvm Controller靶机复盘
author: LingMj
data: 2025-05-21
categories: [hackmyvm]
tags: [upload]
description: 难度-Hard
---

## 网段扫描
```
Interface: eth0, type: EN10MB, MAC: 00:0c:29:d1:27:55, IPv4: 192.168.137.190
Starting arp-scan 1.10.0 with 256 hosts (https://github.com/royhills/arp-scan)
192.168.137.1	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.64	a0:78:17:62:e5:0a	Apple, Inc.
192.168.137.84	3e:21:9c:12:bd:a3	(Unknown: locally administered)
192.168.137.91	62:2f:e8:e4:77:5d	(Unknown: locally administered)

7 packets received by filter, 0 packets dropped by kernel
Ending arp-scan 1.10.0: 256 hosts scanned in 2.064 seconds (124.03 hosts/sec). 4 responded
```

## 端口扫描

```
root@LingMj:~# nmap -p- -sV -sC 192.168.137.84
Starting Nmap 7.95 ( https://nmap.org ) at 2025-05-21 00:45 EDT
Nmap scan report for controller.mshome.net (192.168.137.84)
Host is up (0.010s latency).
Not shown: 65520 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
22/tcp    open  ssh          OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 73:a1:2c:d9:47:5c:18:0b:68:60:02:58:f9:a2:c4:18 (RSA)
|   256 2d:51:0e:a5:af:b2:b1:36:5b:93:6c:d2:17:a3:39:4c (ECDSA)
|_  256 d0:bb:81:c4:16:aa:28:af:68:f5:38:7d:af:9f:4a:5b (ED25519)
80/tcp    open  http         Apache httpd 2.4.41 ((Ubuntu))
|_http-generator: WordPress 5.7.2
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: CONTROLLER &#8211; Otro sitio realizado con WordPress
88/tcp    open  kerberos-sec Heimdal Kerberos (server time: 2025-05-21 04:45:25Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Samba smbd 4
389/tcp   open  ldap         (Anonymous bind OK)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CONTROLLER.controller.local/organizationName=Samba Administration
| Not valid before: 2021-06-27T17:19:10
|_Not valid after:  2023-05-28T17:19:10
443/tcp   open  ssl/http     Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_ssl-date: TLS randomness does not represent time
|_http-title: 400 Bad Request
| tls-alpn: 
|_  http/1.1
| ssl-cert: Subject: organizationName=CONTROLLER/stateOrProvinceName=Some-State/countryName=AU
| Not valid before: 2021-06-27T17:44:27
|_Not valid after:  2022-06-27T17:44:27
445/tcp   open  netbios-ssn  Samba smbd 4
464/tcp   open  kpasswd5?
636/tcp   open  ssl/ldap     (Anonymous bind OK)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CONTROLLER.controller.local/organizationName=Samba Administration
| Not valid before: 2021-06-27T17:19:10
|_Not valid after:  2023-05-28T17:19:10
3268/tcp  open  ldap         (Anonymous bind OK)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=CONTROLLER.controller.local/organizationName=Samba Administration
| Not valid before: 2021-06-27T17:19:10
|_Not valid after:  2023-05-28T17:19:10
3269/tcp  open  ssl/ldap     (Anonymous bind OK)
| ssl-cert: Subject: commonName=CONTROLLER.controller.local/organizationName=Samba Administration
| Not valid before: 2021-06-27T17:19:10
|_Not valid after:  2023-05-28T17:19:10
|_ssl-date: TLS randomness does not represent time
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
MAC Address: 3E:21:9C:12:BD:A3 (Unknown)
Service Info: OSs: Linux, Windows; CPE: cpe:/o:linux:linux_kernel, cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: CONTROLLER, NetBIOS user: <unknown>, NetBIOS MAC: b0:ca:c0:8d:72:7f (unknown)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-05-21T04:46:18
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 79.29 seconds
```

## 获取webshell

![picture 0](../assets/images/9406cf2b7f12ff5005b75bb98480bc9532d22fdb519f3b1b1923735281c75c2f.png)  

>不是他域名是这个么老是跳这里
>

![picture 1](../assets/images/d0d4d0364b771a9e65c75b8504924091ed89e64e112de9b3b1b36c800a9c9321.png)  

```
<!doctype html>
<html lang="es" >
<head>
	<meta charset="UTF-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1" />
	<title>Página no encontrada &#8211; CONTROLLER</title>
<meta name='robots' content='max-image-preview:large' />
<link rel='dns-prefetch' href='//192.168.0.25' />
<link rel='dns-prefetch' href='//s.w.org' />
<link rel="alternate" type="application/rss+xml" title="CONTROLLER &raquo; Feed" href="http://192.168.0.25/index.php/feed/" />
<link rel="alternate" type="application/rss+xml" title="CONTROLLER &raquo; Feed de los comentarios" href="http://192.168.0.25/index.php/comments/feed/" />
		<script>
			window._wpemojiSettings = {"baseUrl":"https:\/\/s.w.org\/images\/core\/emoji\/13.0.1\/72x72\/","ext":".png","svgUrl":"https:\/\/s.w.org\/images\/core\/emoji\/13.0.1\/svg\/","svgExt":".svg","source":{"concatemoji":"http:\/\/192.168.0.25\/wp-includes\/js\/wp-emoji-release.min.js?ver=5.7.2"}};
			!function(e,a,t){var n,r,o,i=a.createElement("canvas"),p=i.getContext&&i.getContext("2d");function s(e,t){var a=String.fromCharCode;p.clearRect(0,0,i.width,i.height),p.fillText(a.apply(this,e),0,0);e=i.toDataURL();return p.clearRect(0,0,i.width,i.height),p.fillText(a.apply(this,t),0,0),e===i.toDataURL()}function c(e){var t=a.createElement("script");t.src=e,t.defer=t.type="text/javascript",a.getElementsByTagName("head")[0].appendChild(t)}for(o=Array("flag","emoji"),t.supports={everything:!0,everythingExceptFlag:!0},r=0;r<o.length;r++)t.supports[o[r]]=function(e){if(!p||!p.fillText)return!1;switch(p.textBaseline="top",p.font="600 32px Arial",e){case"flag":return s([127987,65039,8205,9895,65039],[127987,65039,8203,9895,65039])?!1:!s([55356,56826,55356,56819],[55356,56826,8203,55356,56819])&&!s([55356,57332,56128,56423,56128,56418,56128,56421,56128,56430,56128,56423,56128,56447],[55356,57332,8203,56128,56423,8203,56128,56418,8203,56128,56421,8203,56128,56430,8203,56128,56423,8203,56128,56447]);case"emoji":return!s([55357,56424,8205,55356,57212],[55357,56424,8203,55356,57212])}return!1}(o[r]),t.supports.everything=t.supports.everything&&t.supports[o[r]],"flag"!==o[r]&&(t.supports.everythingExceptFlag=t.supports.everythingExceptFlag&&t.supports[o[r]]);t.supports.everythingExceptFlag=t.supports.everythingExceptFlag&&!t.supports.flag,t.DOMReady=!1,t.readyCallback=function(){t.DOMReady=!0},t.supports.everything||(n=function(){t.readyCallback()},a.addEventListener?(a.addEventListener("DOMContentLoaded",n,!1),e.addEventListener("load",n,!1)):(e.attachEvent("onload",n),a.attachEvent("onreadystatechange",function(){"complete"===a.readyState&&t.readyCallback()})),(n=t.source||{}).concatemoji?c(n.concatemoji):n.wpemoji&&n.twemoji&&(c(n.twemoji),c(n.wpemoji)))}(window,document,window._wpemojiSettings);
		</script>
		<style>
img.wp-smiley,
img.emoji {
	display: inline !important;
	border: none !important;
	box-shadow: none !important;
	height: 1em !important;
	width: 1em !important;
	margin: 0 .07em !important;
	vertical-align: -0.1em !important;
	background: none !important;
	padding: 0 !important;
}
</style>
	<link rel='stylesheet' id='wp-block-library-css'  href='http://192.168.0.25/wp-includes/css/dist/block-library/style.min.css?ver=5.7.2' media='all' />
<link rel='stylesheet' id='wp-block-library-theme-css'  href='http://192.168.0.25/wp-includes/css/dist/block-library/theme.min.css?ver=5.7.2' media='all' />
<link rel='stylesheet' id='twenty-twenty-one-style-css'  href='http://192.168.0.25/wp-content/themes/twentytwentyone/style.css?ver=1.3' media='all' />
<link rel='stylesheet' id='twenty-twenty-one-print-style-css'  href='http://192.168.0.25/wp-content/themes/twentytwentyone/assets/css/print.css?ver=1.3' media='print' />
<link rel="https://api.w.org/" href="http://192.168.0.25/index.php/wp-json/" /><link rel="EditURI" type="application/rsd+xml" title="RSD" href="http://192.168.0.25/xmlrpc.php?rsd" />
<link rel="wlwmanifest" type="application/wlwmanifest+xml" href="http://192.168.0.25/wp-includes/wlwmanifest.xml" /> 
<meta name="generator" content="WordPress 5.7.2" />
<style>.recentcomments a{display:inline !important;padding:0 !important;margin:0 !important;}</style></head>

<body class="error404 wp-embed-responsive is-light-theme no-js hfeed">
<div id="page" class="site">
	<a class="skip-link screen-reader-text" href="#content">Saltar al contenido</a>

	
<header id="masthead" class="site-header has-title-and-tagline" role="banner">

	

<div class="site-branding">

	
						<p class="site-title"><a href="http://192.168.0.25/">CONTROLLER</a></p>
			
			<p class="site-description">
			Otro sitio realizado con WordPress		</p>
	</div><!-- .site-branding -->
	

</header><!-- #masthead -->

	<div id="content" class="site-content">
		<div id="primary" class="content-area">
			<main id="main" class="site-main" role="main">

	<header class="page-header alignwide">
		<h1 class="page-title">No hay nada aquí</h1>
	</header><!-- .page-header -->

	<div class="error-404 not-found default-max-width">
		<div class="page-content">
			<p>Parece que no se ha encontrado nada en esta ubicación. ¿Quieres probar una búsqueda?</p>
			<form role="search"  method="get" class="search-form" action="http://192.168.0.25/">
	<label for="search-form-1">Buscar...</label>
	<input type="search" id="search-form-1" class="search-field" value="" name="s" />
	<input type="submit" class="search-submit" value="Buscar" />
</form>
		</div><!-- .page-content -->
	</div><!-- .error-404 -->

			</main><!-- #main -->
		</div><!-- #primary -->
	</div><!-- #content -->

	
	<aside class="widget-area">
		<section id="search-2" class="widget widget_search"><form role="search"  method="get" class="search-form" action="http://192.168.0.25/">
	<label for="search-form-2">Buscar...</label>
	<input type="search" id="search-form-2" class="search-field" value="" name="s" />
	<input type="submit" class="search-submit" value="Buscar" />
</form>
</section>
		<section id="recent-posts-2" class="widget widget_recent_entries">
		<h2 class="widget-title">Entradas recientes</h2><nav role="navigation" aria-label="Entradas recientes">
		<ul>
											<li>
					<a href="http://192.168.0.25/index.php/2021/06/27/hola-mundo/">CONTROLLER</a>
									</li>
					</ul>

		</nav></section><section id="recent-comments-2" class="widget widget_recent_comments"><h2 class="widget-title">Comentarios recientes</h2><nav role="navigation" aria-label="Comentarios recientes"><ul id="recentcomments"><li class="recentcomments"><span class="comment-author-link"><a href='https://wordpress.org/' rel='external nofollow ugc' class='url'>Un comentarista de WordPress</a></span> en <a href="http://192.168.0.25/index.php/2021/06/27/hola-mundo/#comment-1">CONTROLLER</a></li></ul></nav></section>	</aside><!-- .widget-area -->


	<footer id="colophon" class="site-footer" role="contentinfo">

				<div class="site-info">
			<div class="site-name">
																						<a href="http://192.168.0.25/">CONTROLLER</a>
																		</div><!-- .site-name -->
			<div class="powered-by">
				Funciona gracias a <a href="https://es.wordpress.org/">WordPress</a>.			</div><!-- .powered-by -->

		</div><!-- .site-info -->
	</footer><!-- #colophon -->

</div><!-- #page -->

<script>document.body.classList.remove("no-js");</script>	<script>
	if ( -1 !== navigator.userAgent.indexOf( 'MSIE' ) || -1 !== navigator.appVersion.indexOf( 'Trident/' ) ) {
		document.body.classList.add( 'is-IE' );
	}
	</script>
	<script id='twenty-twenty-one-ie11-polyfills-js-after'>
( Element.prototype.matches && Element.prototype.closest && window.NodeList && NodeList.prototype.forEach ) || document.write( '<script src="http://192.168.0.25/wp-content/themes/twentytwentyone/assets/js/polyfills.js?ver=1.3"></scr' + 'ipt>' );
</script>
<script src='http://192.168.0.25/wp-content/themes/twentytwentyone/assets/js/responsive-embeds.js?ver=1.3' id='twenty-twenty-one-responsive-embeds-script-js'></script>
<script src='http://192.168.0.25/wp-includes/js/wp-embed.min.js?ver=5.7.2' id='wp-embed-js'></script>
	<script>
	/(trident|msie)/i.test(navigator.userAgent)&&document.getElementById&&window.addEventListener&&window.addEventListener("hashchange",(function(){var t,e=location.hash.substring(1);/^[A-z0-9_-]+$/.test(e)&&(t=document.getElementById(e))&&(/^(?:a|select|input|button|textarea)$/i.test(t.tagName)||(t.tabIndex=-1),t.focus())}),!1);
	</script>
	
</body>
</html>
```

![picture 2](../assets/images/e3e2445f5876030de1f93ee8d6c33cbd1eaa6ffaa1a9dca6cecc8723c5b1ffe4.png)  

>这个wordpress用不了已经被焊死在某个ip无法操作，看smb了
>

![picture 3](../assets/images/c7955f3ec37cd4845f37666535f0be5aec8e51d7217050f1ceedddc79db0df21.png)  
![picture 4](../assets/images/903c708d826cc0106930e504ef2a9d5d724dbaa18f258c02f232a19f5f34c22c.png)  
![picture 5](../assets/images/007a6125565c3f03615094b917c082a047776851520b2911ec7fd0f57b1df711.png)  

>那就尝试上传看看是否存在定时任务操作
>

![picture 6](../assets/images/7831a18e665a35c32f2d991c789b657640f1c4c584adf12f7b922d4f5de95fbd.png)  
![picture 7](../assets/images/cb538c442f2a7dc1656f2d092f207a7283bf6e0fff897c2b6982fe7519cc9926.png)  

![picture 8](../assets/images/d99c951578140196b54d4aee08c15b7bc69f95995e35e8ead707ef8959a4bab4.png)  

>没有
>

![picture 9](../assets/images/d915952b65a3727e57aa860bb8a72222f0f9210e5e969a4abd44e9aa5bc98561.png)  
![picture 10](../assets/images/31afb5519d68806241ee39e57dc38a45e3dd71a25c7985ac82545f002041cc15.png)  

>目前os和subprocess
>

```
root@LingMj:~/xxoo/jarjar# curl -s http://192.168.137.84|html2text|uniq 
Saltar al contenido
****** CONTROLLER ******
Otro sitio realizado con WordPress
***** CONTROLLER *****
A domain controller (DC) is a server computer that responds to security
authentication requests within a computer network domain. It is
a network server that is responsible for allowing host access to domain
resources. It authenticates users, stores user account information and
enforces security policy for a domain. It is most commonly implemented
in Microsoft Windows environments (see Domain controller (Windows)), where it
is the centerpiece of the Windows Active Directory service.… Seguir leyendo
CONTROLLER
Publicada el 27 de junio de 2021
Categorizado como Sin categoría
Buscar...[Unknown INPUT type][Buscar]
***** Entradas recientes *****
    * CONTROLLER
***** Comentarios recientes *****
    * Un comentarista de WordPress en CONTROLLER
CONTROLLER
Funciona gracias a WordPress.
                                                                                                                                                                                                        
root@LingMj:~/xxoo/jarjar# curl -s http://192.168.137.84/index.php/2021/06/27/hola-mundo/|html2text|uniq
Saltar al contenido
CONTROLLER
Otro sitio realizado con WordPress
****** CONTROLLER ******
A domain controller (DC) is a server computer that responds to security
authentication requests within a computer network domain. It is
a network server that is responsible for allowing host access to domain
resources. It authenticates users, stores user account information and
enforces security policy for a domain. It is most commonly implemented
in Microsoft Windows environments (see Domain controller (Windows)), where it
is the centerpiece of the Windows Active Directory service. However, non-
Windows domain controllers can be established via identity management software
such as Samba and Red HatFreeIPA.
From controller we want to announce that our services are going to change to
the python 3 programming language which stands out mainly for its portability.
Due to the termination of python 2, there are still tools that use this
language but we still offer support for it. If you want to support our projects
or help to improve them you can upload them and our experts will test your
utilities for you.
Publicada el 27 de junio de 2021Por control
Categorizado como Sin categoría
***** 1 comentario *****
   1. Un comentarista de WordPress dice:
      27 de junio de 2021 a las 18:36
      Hola, esto es un comentario.
      Para empezar a moderar, editar y borrar comentarios, por favor, visita la
      pantalla de comentarios en el escritorio.
      Los avatares de los comentaristas provienen de Gravatar.
      Responder
***** Dejar un comentario Cancelar la respuesta *****
Tu dirección de correo electrónico no será publicada. Los campos obligatorios
están marcados con *
Nombre *[author                        ]
Correo electrónico *[Unknown INPUT type]
Web[Unknown INPUT type]
[ ]Guarda mi nombre, correo electrónico y web en este navegador para la próxima
vez que comente.
[Publicar el comentario]
Buscar...[Unknown INPUT type][Buscar]
***** Entradas recientes *****
    * CONTROLLER
***** Comentarios recientes *****
    * Un comentarista de WordPress en CONTROLLER
CONTROLLER
Funciona gracias a WordPress.
```

>只能这样才能获得信息，需要python代码在smb
>

![picture 11](../assets/images/8264b700632a86679e13efb17388bc965d944e2afc452b52c802930df4bf043b.png)  
![picture 12](../assets/images/3a0067075926d60b357a602e5576841dc431aba879d8a7b90b168fa9ed444df7.png)  
![picture 13](../assets/images/53785c9d9c5bcb87a9f58a02a6300648f9d1c4feaf249904ba487f3c3af77715.png)  
![picture 14](../assets/images/f80e2c2dbcf8e2c42bf36780fbb6264c973dcac88ff8782f7f9630f07e731e31.png)  
![picture 15](../assets/images/b7c09394b807dcb1845038fd772fcc7293becd99eda043c040cc070446ba564f.png)  
![picture 16](../assets/images/74a42dfe4cf4769a8b0930671c86189c39812f3477797a9b5b2e4ec558ac9139.png)  
![picture 17](../assets/images/bdc64627b6a82521b489492c85b0487863da9b3b0262f5d667ce7bde7222d686.png)  

>没弹回来
>

![picture 18](../assets/images/8de8e2bb5ec264ad0caa1e94d24d133733e8189fde8050929b755aaf4dfe990d.png)  
![picture 19](../assets/images/6c187a02f12cb3605daaff3e0b7e354dd986e4327dc02a86ebff88e2861cdb86.png)  

>还是这个方案好使
>



## 提权

![picture 21](../assets/images/bfb1cea3eeee54c51b95aec139f36059dd8fa3155d06948a2d5808f8d237476a.png)  
![picture 22](../assets/images/0aa17478c68aa2544057baeda030126f8076b7daa5eaeeba7cdc61b1bbc67f79.png)  
![picture 23](../assets/images/4236167cbd6233cf165f7df2dd791508ff6ace21359e07fb8747d4ea65f72504.png)  

>还有定时任务
>

![picture 24](../assets/images/b8d23c64264f82420abc67078e200f296218dc86e7883624e1fd3769ab8b7e79.png)  
![picture 25](../assets/images/ce9b76b750aed5425660c5ad2d21edd6eaa2720915d0f2ebe853198c3814d29b.png)  
![picture 26](../assets/images/82430d82edb835d4d2ab14993c6efdffd515c2b7809b41298d9bbe94ea689e22.png)  
![picture 27](../assets/images/81cdbca0a4882365ac1078ccf37315bfc66e9448b1fc2a997580f6fb3b9eb75f.png)  
![picture 28](../assets/images/f7686d538b3a3004a5b5b0f053e2523cc48bb60c4c568af2012d7034ba70eb33.png)  

>还不给看
>

![picture 29](../assets/images/eb164eabcce05ba6a2276348699f9d49b161e2214cb9d1ebbfb57a3452ed5d16.png)  
![picture 30](../assets/images/8ace998663bb0be1affc0ad50f82c14c77591451ea4a35461ef93bdcb81e3c1d.png)  

>用@eval好了
>

![picture 31](../assets/images/0bcbbad591e0cbd66711df94222ff575f4a67c3d5119af22f4cc4f5d718d0090.png)  
![picture 32](../assets/images/13962c62cca3b58da0e2dd945d60a1d5061974b6b55d19be470c5499a179e19c.png)  
![picture 33](../assets/images/ca87cab3d054001d20d62abdf0534a1c45f7f74510ed6e2204fa316e5f703262.png)  

>问题是他在那
>

![picture 34](../assets/images/adb020233323e2b8a3c51ad6911b3900a58ce908e94f01b9266e84795aa14c35.png)  

>直接靶机找地址
>

![picture 35](../assets/images/9ebf9298ddcb52d343dbda19288c24359e1dca324226d9fea35eb2858f870f6b.png)  
![picture 36](../assets/images/015f5e78aa90d6ebdcbe3ef3ba1ba38c3a7de554a076e4e0eac48fb52f1cc147.png)  
![picture 37](../assets/images/142a3662aed27ea3a56d9e5b831fc33f8e1b10b226b44e6d7fc3f14fd67976c7.png)  
![picture 38](../assets/images/c6d9e2eafa10ab4f37b74cf3d3d6bb38cd1e597531dece24af6621e927365a52.png)  

>需要函数绕过的话
>

![picture 39](../assets/images/fc70d39e08766be863bbe6dcf6b0f3e8e453965cf0e62256734293980be7e8ce.png)  
![picture 40](../assets/images/1a27f6803bc384d6c1ff4e03c2c5bde874bdb5e7ca8df6eb24ef7743e250ca4c.png)  
![picture 41](../assets/images/7d5559431348f65e41d73eb86fd12c007181d525cadab8b7c8d9b5bfd35992da.png)  

>这样吧
>

![picture 42](../assets/images/f354a067e636b81ed7fbc8a154e4c4b56e29fec0331c4c6a456e529d4dc0a0c8.png)  
![picture 43](../assets/images/93ae265d6dc5163959171bd0f1db838a4088fc5129c0de6fee508c7368ea7839.png)  
![picture 44](../assets/images/734067473d98841a3e894b4dea21f50935e68f8f12a27d5e741f93831227975b.png)  
![picture 45](../assets/images/14b617570d7d741a7e968f82d3c9d117286d4851b731e58ded245bb129b69d32.png)  

>目前没定时任务了应该怎么继续提权呢
>

![picture 46](../assets/images/d01b7a76479294e8f40058e43736db917825682dd1041854df5c6e29be53cad5.png)  

>那直接改就王炸了
>

![picture 47](../assets/images/75c58842c42f3a3f18da850151b6cb2cc3e6e86f092b991cbd8be4ee24633e37.png)  
![picture 48](../assets/images/01ac63891dfc73151d1e2f5ee4512c22cc500275fcfe9060a3ddb3011839a784.png)  

![picture 49](../assets/images/96df5f27095ed45730b011787b4d670920a54c82582a4acf223cc68414d91921.png)  
![picture 50](../assets/images/ec3892baade87c6dac8a776653726594bfda5fe272ba592ed7b790a01fe0b51c.png)  
![picture 51](../assets/images/62566481d0970be40ade1ddf5923c225ed9ab806420c2a80dfa5526ec58eeabe.png)  
![picture 52](../assets/images/67ae0120591431671b732841b2eb8e11e8dca0033e196693673d363b3856dd7c.png)  
![picture 53](../assets/images/44681997b16fe4247c4eee9b2b1004a6af543ed538bafda41397d2db7305b87f.png)  
![picture 54](../assets/images/e2d228878398aee53720542fab79b00544cab27e639abf14cd247a667889526d.png)  
![picture 55](../assets/images/cef3620a2ac08bbb2b48fdddcd6c571fc30523ac00454dbc5e1e1c0018b21863.png)  
![picture 56](../assets/images/a3de3b722cae7a786a9060d18581b8d0c9c2f108f9dd93b4930014aac8dc618e.png)  
![picture 57](../assets/images/f905bc6ebb4d1ea2cb1b8cd317b9459e41f7b774fdb2c68bf600a77ac71f642b.png)  
![picture 58](../assets/images/7828210851e7f7f548e8ee956f6a62084bbb3ee18df1b750d69a604cf0842fe3.png)  

>原来那个deb坏了换了个新的
>

>userflag:K1ng0F3V4S10n
>
>rootflag:DpKg1sB3tt3rTh4nPyth0n?
>
