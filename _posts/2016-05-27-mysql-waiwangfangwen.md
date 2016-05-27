---
layout: post
title: 怎样设置才能允许外网访问MySQL
tags:
- mysql
- 外网访问muysql
- 共享mysql
- 共享数据库
- 限制登陆
description: mysql 外网访问muysql 共享mysql 共享数据库 限制登陆
---
<br>
摘自网络：<br>
大多数情况下，mysql数据库只要本机访问就可以了，这样的话，默认安装就OK，但是如果需要外网访问mysql数据库的话，应该如何操作呢，想知道的话，就好好看看下面的介绍吧
<br>
<div id="content">
<p>设置mysql服务允许外网访问，修改mysql的配置文件，有的是my.ini，有的是my.cnf【linux】.</p>
<p><strong>1：设置mysql的配置文件<br>
</strong>&nbsp;&nbsp;&nbsp;&nbsp; /etc/mysql/my.cnf<br>
&nbsp;&nbsp;&nbsp;&nbsp; 找到 bind-address&nbsp; =127.0.0.1&nbsp; 将其注释掉；//作用是使得不再只允许本地访问；<br>
&nbsp;<br>
　　重启mysql：/etc/init.d/mysql restart;<br>
&nbsp;<br>
</p>
<p><strong>2：登录mysql数据库</strong>：mysql -u root -p<br>
　　mysql&gt; use mysql;<br>
&nbsp;<br>
　　查询host值：<br>
mysql&gt; select user,host from user;<br>
&nbsp;<br>
如果没有"%"这个host值,就执行下面这两句:<br>
mysql&gt; update user set host='%' where user='root';<br>
mysql&gt; flush privileges;<br>
或者也可以执行：<br>
mysql&gt;grand all privileges on&nbsp; *.*&nbsp; to <a href="mailto:root@'%'" rel="external nofollow">root@'%'</a>&nbsp; identifies&nbsp; by ' xxxx';<br>
其中 第一个*表示数据库名；第二个*表示该数据库的表名；如果像上面那样 *.*的话表示所有到数据库下到所有表都允许访问；<br>
‘%'：表示允许访问到mysql的ip地址；当然你也可以配置为具体到ip名称；%表示所有ip均可以访问；<br>
&nbsp;后面到‘xxxx'为root 用户的password；<br>
&nbsp;<br>
</p>
<p>举例：<br>
</p>
<p>任意主机以用户root和密码mypwd连接到mysql服务器<br>
mysql&gt; GRANT ALL PRIVILEGES ON *.* TO <a href="mailto:'root'@'%'" rel="external nofollow">'root'@'%'</a> IDENTIFIED BY 'mypwd' WITH GRANT OPTION;<br>
mysql&gt; flush privileges;</p>
<p>IP为192.168.1.102的主机以用户myuser和密码mypwd连接到mysql服务器<br>
mysql&gt; GRANT ALL PRIVILEGES ON *.* TO <a href="mailto:'myuser'@'192.168.1.102'" rel="external nofollow">'myuser'@'192.168.1.102'</a> IDENTIFIED BY 'mypwd' WITH GRANT OPTION; <br>
mysql&gt; flush privileges;</p>


</div>


