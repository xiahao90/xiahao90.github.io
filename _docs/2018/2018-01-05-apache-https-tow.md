---
title: Apache httpd设置HTTPS双向认证
category: 2018
order: 100
date:2018-01-05
---
<div>原文地址<a href="http://blog.csdn.net/agonie201218/article/details/54866268">http://blog.csdn.net/agonie201218/article/details/54866268</a></div>
<div id="article_content" class="article_content tracking-ad" data-mod="popu_307" data-dsm="post">

<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">一、环境&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">httpd: 2.4.4&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">openssl：1.0.1&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">os：ubuntu 12.04 LTS&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">二、场景&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">我准备在httpd上配置一个HTTPS双向认证，既向客户端表明自己的身份，也只允许特定的客户端访问。本文说的主要是作为server的角色的配置，至于作为client的配置，最后也会稍微介绍一下，但是不会详细说明&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">一般来说，互联网站不会去配置双向认证，因为客户端证书的分发和管理会比较麻烦，会把用户挡在门外，所以一般是不能这么做的。当然，像银行等对安全要求很高的网站，也会采用双向认证，比如U盾、安全控件什么的，其实就是固化的客户端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">但是对于企业应用来说，客户一般是固定的，比如两个已知的系统对接、内部系统集成等。所以在企业应用领域，双向认证还是比较常见的&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">三、背景知识&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">证书（Certificate）是HTTPS的核心，但是其实证书并不是一个单一的东西，而是几种技术的综合&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">为了网络传输的安全，有很多种技术，最主要的是以下3种：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">1、加密/解密&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">避免消息明文传输，对消息进行加密。早期一般是用对称加密算法，现在一般都是不对称加密，最常见的算法就是RSA。在后面的介绍中，也会多次看到RSA这个词&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">2、消息摘要&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这个技术主要是为了避免消息被篡改。消息摘要是把一段信息，通过某种算法，得出一串字符串。这个字符串就是消息的摘要。如果消息被篡改（发生了变化），那么摘要也一定会发生变化（一般是这样的。如果2个不同的消息生成的摘要是一样的，那么这就叫发生了碰撞）&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">消息摘要的算法主要有MD5和SHA，在证书领域，一般都是用SHA（安全哈希算法）&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">3、数字签名&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">数字签名是为了验证双方的身份，避免身份伪造&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">以上三个技术结合起来，就是在HTTPS中广泛应用的证书（certificate），证书本身携带了加密/解密的信息，并且可以标识自己的身份，也自带消息摘要&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">四、在httpd中配置单向HTTPS&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">首先在%HTTPD_HOME%/conf/目录下，修改httpd.conf文件，加载必要的模块&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Httpd代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">LoadModule&nbsp;socache_shmcb_module&nbsp;modules/mod_socache_shmcb.so&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">LoadModule&nbsp;socache_dbm_module&nbsp;modules/mod_socache_dbm.so&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">LoadModule&nbsp;socache_memcache_module&nbsp;modules/mod_socache_memcache.so&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">LoadModule&nbsp;ssl_module&nbsp;modules/mod_ssl.so&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里的前提是，在编译httpd的时候，已经编译了ssl模块。这个步骤看另一篇文档：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<a target="_blank" href="http://kyfxbl.iteye.com/blog/1902299" style="color:rgb(16,138,198); font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">http://kyfxbl.iteye.com/blog/1902299</a><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后再导入默认的SSL配置文件，当然也可以选择不导入，在httpd.conf直接配置。但是导入默认的可以节省很多时间，并且默认的文件是用vhost配置的，不会跟main server冲突，可以算是一种最佳实践&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Httpd代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">#&nbsp;Secure&nbsp;(SSL/TLS)&nbsp;connections&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">Include&nbsp;conf/extra/httpd-ssl.conf&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">&lt;IfModule&nbsp;ssl_module&gt;&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLRandomSeed&nbsp;startup&nbsp;builtin&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLRandomSeed&nbsp;connect&nbsp;builtin&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">&lt;/IfModule&gt;&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后打开%HTTPD_HOME%/conf/extra/目录，看一下httpd-ssl.conf，主要有以下几个配置&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Httpd代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLEngine&nbsp;on&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLCertificateFile&nbsp;<span class="string" style="color:blue">"/usr/local/httpd/conf/server.cer"</span>&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLCertificateKeyFile&nbsp;<span class="string" style="color:blue">"/usr/local/httpd/conf/server.key.pem"</span>&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">#SSLCACertificateFile&nbsp;<span class="string" style="color:blue">"/usr/local/httpd/conf/ca.cer"</span>&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">#SSLVerifyClient&nbsp;require&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">#SSLVerifyDepth&nbsp;&nbsp;<span class="number" style="color:rgb(192,0,0)">10</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">只要开启前3个，单向的HTTPS认证就配置好了。后面3个目前先注释掉，是后面双向认证才用到&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后重启一下httpd，会发现报错：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">AH00526: Syntax error on line 106 of /usr/local/httpd/conf/extra/httpd-ssl.conf:&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">SSLCertificateFile: file '/usr/local/httpd/conf/server.cer' does not exist or is empty&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这是因为httpd需要一个服务端的私钥（.key.pem），和一个服务端证书（.cer）。前面已经配置了这2个文件的路径，但是还没有创建。下一步就要创建这些文件&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">五、创建CA（Certificate Authority）&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这个CA，也叫“根证书”&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">服务端做了一个证书，但是这是没有法律效力的，谁都可以自己做证书，就根本达不到安全的目的。所以就要有一个机构，负责来确认服务端的身份，然后统一的签发证书。这样才能有权威性&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">当浏览器通过HTTPS协议访问一个网站，网站首先会发过来一个自己的证书（certificate）。接下来浏览器就会到权威机构（CA），去验证一下这个证书是不是它签发的。如果是的话，就信任这个网站的证书，继续访问;如果不是的话，要怎么处理就依赖于实现了。一般的浏览器会弹出一个警告，让用户自己决定要不要继续访问。当然直接拒绝也是可以的&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">现在国际上有3大CA机构，如果是要自己做一个网站的话，如上所述，一般是需要请这些权威机构帮忙签发证书的。现在所有的主流浏览器，默认都安装了这些CA的根证书，所以如果网站的证书是这些权威机构签发的，浏览器就不会发出警告了。比如支付宝，它的证书是由VeriSign签发的，所以访问支付宝，浏览器不会发出警告&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2055/314c0d76-aee5-3465-a278-082aa63b6b8c.png" title="点击查看原始大小图片" class="magplus" width="700" height="436" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里还有一个链条的关系，比如我有10个子网站，如果每个都要去找CA签发证书，就很麻烦。我可以找CA给我签发一个次级根证书，然后再用这个次级根证书给自己签发10个证书。那么只要客户的浏览器里有CA根证书就可以了，这10个证书都可以通过认证，不要求客户安装次级根证书，原文见下：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="quote_title" style="font-weight:bold; padding:5px; margin:5px 0px 0px 15px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
引用</div>
<div class="quote_div" style="border:1px solid rgb(204,204,204); margin:0px 5px 5px 15px; padding:3px; background:rgb(250,250,250); font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
Intermediate CA certificates lie between the root CA certificate (which is installed in the browsers) and the server certificate (which you installed on the server).</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">如果是企业应用，那完全可以自己给自己当CA，因为可以要求目标用户（系统）安装自己的CA根证书，效果是一样的，还可以省下请权威CA签发证书的费用（互联网应用分发自己的CA到无数的互联网用户上，难度很大）&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">下面就介绍如何创建自己的CA&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">1、准备工作&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">先在随便一个目录，创建以下几个子目录：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">/private&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">/certificates&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">其中private放的是私钥和CSR（后面会介绍），certificates里放的就是证书了&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">2、创建CA私钥&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;genrsa&nbsp;-aes256&nbsp;-out&nbsp;private/ca.key.pem&nbsp;<span class="number" style="color:rgb(192,0,0)">2048</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">最后的参数是RSA密钥的长度，默认是512。2048其实长了一点，老的浏览器稍后会不支持，不过现在的主流浏览器都是支持的，所以问题不大&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">通过这个命令，私钥就创建好了，文件名是ca.key.pem&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">用这个命令，可以看一下刚才创建的这个私钥的信息&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;rsa&nbsp;-noout&nbsp;-text&nbsp;-in&nbsp;private/ca.key.pem&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">不过基本上，看也是白看，反正我是看不懂。只知道私钥里其实有2组数字，是用来形成公钥的，最后也会包含在证书里&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="quote_title" style="font-weight:bold; padding:5px; margin:5px 0px 0px 15px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
引用</div>
<div class="quote_div" style="border:1px solid rgb(204,204,204); margin:0px 5px 5px 15px; padding:3px; background:rgb(250,250,250); font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br>
A private key contains a series of numbers. Two of these numbers form the "public key", the others are part of the "private key". The "public key" bits are included when you generate a CSR, and subsequently form part of the associated Certificate.&nbsp;<br>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">另外，最后的.pem扩展名，是表示该私钥用PEM编码。实际上私钥和证书都是用PEM编码的，PEM只是一种编码格式，不需要太在意。httpd可以直接处理这种编码格式，但是浏览器和JAVA都不行，所以在需要的时候，会把编码从PEM改成PKCS，后面会介绍。只要知道证书和私钥都有编码，只是编码是PEM还是PKCS的区别而已。就像"你好"可以用UTF-8编码，也可以用GBK编码一样，内容是不变的&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">3、创建CA签名请求&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;req&nbsp;-new&nbsp;-key&nbsp;private/ca.key.pem&nbsp;-out&nbsp;private/ca.csr&nbsp;-subj&nbsp;<span class="string" style="color:blue">"/C=CN/ST=SZ/L=SZ/O=kyfxbl/OU=kyfxbl/CN=*.kyfxbl.net"</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里要注意的是，如果不用-subj参数，那么就会在命令行交互输入签发目标的身份识别信息，这叫DN（Distinguished Name）。其中别的都不要紧，最重要的是CN那一行，因为我这里是根证书，所以我设置为*.kyfxbl.net，这样我后面用这个CA签发的www.kyfxbl.net、game.kyfxbl.net、news.kyfxbl.net……，全都是有效的&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">生成的签名请求文件，是ca.csr&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;req&nbsp;-noout&nbsp;-text&nbsp;-in&nbsp;private/ca.csr&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">同上，看不懂&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">4、自己签发CA根证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;x509&nbsp;-req&nbsp;-days&nbsp;<span class="number" style="color:rgb(192,0,0)">3650</span>&nbsp;-sha1&nbsp;-extensions&nbsp;v3_ca&nbsp;-signkey&nbsp;private/ca.key.pem&nbsp;-in&nbsp;private/ca.csr&nbsp;-out&nbsp;certificates/ca.cer&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里参数很复杂，我也不太清楚准确的意思是什么，可以用openssl x509 -help自己研究一下&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">生成的ca.cer，就是最终的根证书了！这个文件非常重要，因为后续的服务端证书、客户端证书，都是用这个CA签发的，也要把它分发给客户，让他们导入到自己的浏览器或者系统中&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">查看的命令是：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;x509&nbsp;-noout&nbsp;-text&nbsp;-in&nbsp;certificates/ca.cer&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">5、把根证书从PEM编码转为PKCS编码&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这步其实不是必选的，但是前面说过，JAVA环境是不能直接用PEM编码的证书的，很多浏览器也不行，所以有时候也需要转一下编码&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;pkcs12&nbsp;-export&nbsp;-cacerts&nbsp;-inkey&nbsp;private/ca.key.pem&nbsp;-in&nbsp;certificates/ca.cer&nbsp;-out&nbsp;certificates/ca.p12&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">得到的ca.p12就是转码后的CA根证书，在不能直接用ca.cer的时候，就用ca.p12代替&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">六、签发服务端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">现在CA根证书和私钥都有了，就可以开始签发服务端证书了（签发请求ca.csr是过程文件，有了cer就不再需要它了，要删掉也可以）。下面的命令和签发CA证书时都差不多，但是参数上有区别&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">1、创建服务端私钥&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;genrsa&nbsp;-aes256&nbsp;-out&nbsp;private/server.key.pem&nbsp;<span class="number" style="color:rgb(192,0,0)">2048</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">2、创建服务端证书签发请求&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;req&nbsp;-new&nbsp;-key&nbsp;private/server.key.pem&nbsp;-out&nbsp;private/server.csr&nbsp;-subj&nbsp;<span class="string" style="color:blue">"/C=CN/ST=SZ/L=SZ/O=kyfxbl/OU=kyfxbl/CN=www.kyfxbl.net"</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">和ca.csr的区别在于，这里的CN不是*.kyfxbl.net，而是www.kyfxbl.net，因为我现在是在为www.kyfxbl.net申请证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">3、利用CA根证书，签发服务端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;x509&nbsp;-req&nbsp;-days&nbsp;<span class="number" style="color:rgb(192,0,0)">3650</span>&nbsp;-sha1&nbsp;-extensions&nbsp;v3_req&nbsp;-CA&nbsp;certificates/ca.cer&nbsp;-CAkey&nbsp;private/ca.key.pem&nbsp;-CAserial&nbsp;ca.srl&nbsp;-CAcreateserial&nbsp;-in&nbsp;private/server.csr&nbsp;-out&nbsp;certificates/server.cer&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里和前面自己签发CA证书时，参数区别就比较大了，最后得到的server.cer，就是服务端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">七、测试单向认证&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">把server.key.pem和server.cer拷贝到%HTTPD_HOME%/conf/目录下，然后重新启动httpd，会要求输入一个密码</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2068/fd6cca1b-9ab5-3a61-b31f-969f3cfe5c22.png" title="点击查看原始大小图片" class="magplus" width="700" height="222" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后访问http://localhost:443/，会报400错误：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2072/f0c04923-c698-3d72-93db-ceee008a6abe.png" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">接下来用https://localhost:443来访问，浏览器报警：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2074/ba212ed2-db82-3990-8a4f-e6997ea76abc.png" title="点击查看原始大小图片" class="magplus" width="700" height="252" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里就是前面创建CSR时，输入的CN的作用，这个证书是为www.kyfxbl.net申请的，这里请求的地址却是localhost，不匹配所以报错。为了能用www.kyfxbl.net这个主机名来访问，就需要改一下/etc/hosts文件：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">127.0.0.1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; localhost&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">192.168.1.102&nbsp;&nbsp; www.kyfxbl.net&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后就可以用www.kyfxbl.net来访问了，再试一下：https://www.kyfxbl.net/&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这次浏览器还是告警，但是告警信息变了：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2078/f4cd8970-5662-36ad-992d-1d4249f2088d.png" title="点击查看原始大小图片" class="magplus" width="700" height="256" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">证书信息如下：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2080/ca5043ac-f257-3943-8831-f00168054cc8.png" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">可以看到这个证书是由*.kyfxbl.net这个CA颁发的，浏览器不认识，所以不信任由这个CA签发的所有证书。接下来就需要把ca.cer导入浏览器。这里直接导入server.cer也是可以的，但是后面如果又创建一个网站比如说www2.kyfxbl.net，那么又不行了。所以最好的办法是直接导入CA根证书，那么后续只要是用这个根证书签发的证书，浏览器都会信任&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">导入前：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2082/40e17f84-1979-3381-be72-ca67e0ae5ee9.png" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">导入后：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2084/cb7d3556-5bb4-3e97-b6e5-6e4165713577.png" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">再次访问，可以看到成功了，浏览器不告警，并且URL栏前面打了一个绿勾&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2086/7bad23b2-60a6-363b-9022-591e9170f035.png" title="点击查看原始大小图片" class="magplus" width="700" height="126" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">八、配置双向认证&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">如果要配置服务器只允许合法的用户访问，就需要配置双向认证&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">配置为双向认证之后，除了服务端要发证书给客户端之外，客户端也要发客户端证书到服务端，服务端认证通过，才允许访问&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Httpd代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLCACertificateFile&nbsp;<span class="string" style="color:blue">"/usr/local/httpd/conf/ca.cer"</span>&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLVerifyClient&nbsp;require&nbsp;&nbsp;</span></li><li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">SSLVerifyDepth&nbsp;&nbsp;<span class="number" style="color:rgb(192,0,0)">10</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">在单项认证的基础上，再配置以上3个参数&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">SSLCACertificateFile，这个意思是当客户端发来客户端证书的时候，httpd用哪个CA根证书校验它&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">配置好了，还不能重启，因为现在客户端证书还没做好&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里要说明一下，客户端证书是怎么来的&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">有2种方式：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">第一种，客户端也自己CA，然后签发证书给自己。把客户端的CA根证书发过来，配置成SSLCACertificateFile。这在互联网应用里基本是不可能的，安全和管理都是问题。但是在企业应用里，还是比较常见的，双方互相交换CA根证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">第二种，就用刚才服务端的CA根证书，签发一个客户端证书，发给用户，用户每次用这个证书来发请求，像银行，支付宝等等，用的是这种方式&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">当然理论上其实还有一种办法，就是客户自己去找权威CA签证书，但是这个是不可能的，因为很麻烦，找CA签也非常贵&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">本文用的是第2种方法。其实都是一样的。关键还是在CA根证书上，所以前面也说过了，CA根证书非常重要&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">九、签发客户端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">1、创建客户端私钥&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;genrsa&nbsp;-aes256&nbsp;-out&nbsp;private/client.key.pem&nbsp;<span class="number" style="color:rgb(192,0,0)">2048</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">2、创建客户端证书签发请求&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;req&nbsp;-new&nbsp;-key&nbsp;private/client.key.pem&nbsp;-out&nbsp;private/client.csr&nbsp;-subj&nbsp;<span class="string" style="color:blue">"/C=CN/ST=SZ/L=SZ/O=kyfxbl/OU=kyfxbl/CN=kyfxbl"</span>&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里的不同在于，这里的CN不是*.kyfxbl.net，也不是www.kyfxbl.net，随便填一个kyfxbl就好了，或者干脆叫user都没问题，反正是一个客户端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">3、利用CA根证书，签发客户端证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;x509&nbsp;-req&nbsp;-days&nbsp;<span class="number" style="color:rgb(192,0,0)">3650</span>&nbsp;-sha1&nbsp;-extensions&nbsp;v3_req&nbsp;-CA&nbsp;certificates/ca.cer&nbsp;-CAkey&nbsp;private/ca.key.pem&nbsp;-CAserial&nbsp;ca.srl&nbsp;-CAcreateserial&nbsp;-in&nbsp;private/client.csr&nbsp;-out&nbsp;certificates/client.cer&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里和签发server.cer基本是一样的&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">4、把客户端证书转换成p12格式&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<div class="dp-highlighter" id="" style="font-family:Monaco,&quot;DejaVu Sans Mono&quot;,&quot;Bitstream Vera Sans Mono&quot;,Consolas,&quot;Courier New&quot;,monospace; width:679px; overflow:auto; margin-left:9px; padding:1px; word-break:break-all; word-wrap:break-word">
<div class="bar">
<div class="tools" style="padding:3px; margin:0px; font-weight:bold">Openssl代码&nbsp;&nbsp;<a target="_blank" title="收藏这段代码" style="color:rgb(16,138,198); text-decoration:underline"><img class="star" src="http://kyfxbl.iteye.com/images/icon_star.png" alt="收藏代码" style="border:0px"></a></div>
</div>
<ol start="1" class="dp-default" style="font-size:1em; line-height:1.4em; margin:0px 0px 1px; padding:2px 0px; border:1px solid rgb(209,215,220); list-style-position:initial; color:rgb(43,145,175)">
<li style="font-size:1em; margin:0px 0px 0px 38px; padding:0px 0px 0px 10px; border-left:1px solid rgb(209,215,220); background-color:rgb(250,250,250); line-height:18px">
<span style="color:black">openssl&nbsp;pkcs12&nbsp;-export&nbsp;-clcerts&nbsp;-inkey&nbsp;private/client.key.pem&nbsp;-in&nbsp;certificates/client.cer&nbsp;-out&nbsp;certificates/client.p12&nbsp;&nbsp;</span></li></ol>
</div>
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这步是必须的，因为稍后就需要把客户端证书导入到浏览器里，但是一般浏览器都不能直接使用PEM编码的证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">十、测试双向认证&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">把ca.cer拷贝到%HTTPD_HOME%/conf/目录下，重启httpd&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后再次访问https://www.kyfxbl.net/，结果这次不是警告，而是直接报错：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2111/6ff294e7-2108-3bba-b776-b5f9fbc87ad7.png" title="点击查看原始大小图片" class="magplus" width="700" height="193" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">接下来要把client.p12导入到浏览器里&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">导入前：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2113/0145ed2a-a10c-3c33-9c28-8c5600fa2dd6.png" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">导入的时候会要求输入密码，这是为了避免有人偷偷拷贝了别人的客户端证书，伪装成合法用户：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2115/c0b0c8a3-245c-314d-b14d-a68911d32cb1.png" title="点击查看原始大小图片" class="magplus" width="700" height="488" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">导入后：&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2117/e0ac270a-78c2-3205-98f2-0c7e42a0d2ab.png" title="点击查看原始大小图片" class="magplus" width="700" height="498" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">然后再次访问，浏览器会要求选择证书。这个步骤是通过双向认证访问网站时必须的，但是平时访问银行、支付宝的时候貌似没有，这是因为这些网站为了简化用户的操作，都会要求用户安装什么“安全控件”，控件自动选择了证书&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<img src="http://dl2.iteye.com/upload/attachment/0087/2119/06467b06-86aa-3b0d-b26e-d8681e19d51b.png" title="点击查看原始大小图片" class="magplus" width="700" height="479" alt="" style="border:0px; font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px"></span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">点击确定，访问成功！&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">十一、JKS等&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">到这里，用httpd配置双向HTTPS认证就完成了&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">本文说的是作为server的角色，要如何配置。但是在JAVA环境下，如果要以client的角色，通过双向认证发起请求，则还有些不同&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">这里实际上是要充当一个类似浏览器的角色，需要校验server certificate，还需要在发起请求的时候，把client certificate发过去&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">在JAVA里，是通过keystore和truststore来实现的，不在本文的讨论范围内。可以看一下其他的几篇博客，在HTTPS分类里&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">十二、参考资料&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<a target="_blank" href="http://httpd.apache.org/docs/2.4/en/ssl/" style="color:rgb(16,138,198); font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">http://httpd.apache.org/docs/2.4/en/ssl/</a><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<a target="_blank" href="http://linux.chinaunix.net/techdoc/net/2008/01/08/976172.shtml" style="color:rgb(16,138,198); font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">http://linux.chinaunix.net/techdoc/net/2008/01/08/976172.shtml</a><span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">&nbsp;</span><br style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">
<span style="font-family:Helvetica,Tahoma,Arial,sans-serif; font-size:14px">《JAVA加密与解密的艺术》——作者梁栋</span>
</div>