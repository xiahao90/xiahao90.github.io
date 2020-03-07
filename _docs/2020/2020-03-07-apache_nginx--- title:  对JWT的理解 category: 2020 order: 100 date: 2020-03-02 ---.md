---
title:  lamp与lnmp的区别
category: 2020
order: 99
date: 2020-03-07
---

<h2>lamp与lnmp的区别</h2>

**1,LAMP的实现原理**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;浏览器向服务器发送http请求，服务器 (Apache) 接受请求,由于php作为Apache的组件模块也会一起启动，它们具有相同的生命周期。Apache会将一些静态资源保存，然后去调用php处理模块进行php脚本的处理。脚本处理完后，Apache将处理完的信息通过http response的方式发送给浏览器，浏览器解析，渲染等一系列操作后呈现整个网页。

**2,LNMP的实现原理**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;浏览器发送http request（请求）请求到服务器（Nginx）,服务器响应并处理web请求，将一些静态资源（CSS，图片，视频等）保存服务器上，然后将php脚本通过接口传输协议（网关协议）PHP-FCGI（fast-cgi）传输给PHP-FPM（进程管理程序）,PHP-FPM不做处理，然后PHP-FPM调用PHP解析器进程，PHP解析器解析php脚本信息。PHP解析器进程可以启动多个，进行并发执行。然后将解析后的脚本返回到PHP-FPM，PHP-FPM再通过fast-cgi的形式将脚本信息传送给Nginx.服务器再通过Http response（响应）的形式传送给浏览器。浏览器再进行解析与渲染然后进行呈现。

**3,两者的区别**

在LNMP中，Nginx本身对脚本不做任何的处理，而是去调用一个PHP-FPM的进程，nginx与php-fpm相互独立的。 
在LAMP中，PHP是Apache的一个模块，具有相同的生命周期。

**4,为什么nginx比apache更快？**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Apache+php_module 是父进程+子进程的模式，每个访问请求会生成子进程，就不会有阻塞了。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Nginx+PHP-FPM 的话感觉 Nginx 和 PHP 相互独立了， Nginx 遇到自己处理不了的请求就会给 PHP-FPM （ PHP FastCGI进程管理器）处理，每来一个请求就会交给一个子进程去处理（这里根据 PHP 的运行配置不同效果也不同，子进程的数量可以是固定的也可以动态生成），然后 Nginx 对交给 PHP-FPM 的请求进行轮训（ epoll 机制）。

[php、apache、nginx、线程、进程](http://blog.sina.com.cn/s/blog_15ebf299a0102xnml.html "php、apache、nginx、线程、进程")

[Apache与Nginx的优缺点比较](https://www.cnblogs.com/jingzhishen/p/3994217.html)
