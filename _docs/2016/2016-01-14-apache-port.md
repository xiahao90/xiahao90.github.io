---
title: apache配置端口指向目录
category: 2016
order: 97
date: 2016-01-14
---
<div>
<div>&nbsp;<span style="font-family:Helvetica, Tahoma, Arial, sans-serif">Apache2.2\conf\httpd.conf与Apache2.2\conf\extra\httpd-vhosts.conf两个文件的操作</span><br/>
</div>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">httpd.conf下apache默认是80端口，找到 Listen 80就可以修改，想要增加新的端口直接添加一个Listen 8080之类的就OK了,如</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">Listen 8080</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">Listen 8081,如果没有为不同的端口指定目录，它们都访问DocumentRoot</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">修改目录：</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">apache默认访问的是安装目录下的htdocs,默认打开就出来It Works!，它由DocumentRoot定义如：</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">DocumentRoot "C:/Program Files/Apache Software Foundation/Apache2.2/htdocs"</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">如果你想将换个目录可以直接DocumentRoot "x:/workspace/project/www"</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">授权：修改目录后，还要给目录设置访问的权限，如果不给权限是不能访问的。如：</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;Directory "C:/Program Files/Apache Software Foundation/Apache2.2/htdocs"&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Options Indexes FollowSymLinks</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; AllowOverride None</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Order allow,deny</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Allow from all</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;/Directory&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">配置多目录：</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">当前项目有两个分支，一个在trunck/www下面，另一个在branch/www下面，于是想localhost:8080访问trunck/www,localhost:8081访问branch/www</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">首先要开启虚拟主机：找到httpd.conf中#Include conf/extra/httpd-vhosts.conf，去掉前面的#号</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">接着配置不同url对应的目录,找到extra/httpd-vhosts.conf</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">修改默认端口80与8080,并添加一个8081的端口</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">NameVirtualHost *:8080</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">NameVirtualHost *:8081</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">设置目录：</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;VirtualHost *:8080&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ServerAdmin webmaster@dummy-host.smiky</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; DocumentRoot "D:/work/trunk/www"</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ServerName dummy-host.<span style="font-size:12px">smiky</span></p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ServerAlias www.dummy-host.<span style="font-size:12px">smiky</span></p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ErrorLog "logs/dummy-host.smiky.log"</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; CustomLog "logs/dummy-host.smiky-access.log" common</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;/VirtualHost&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;VirtualHost *:8081&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ServerAdmin webmaster@dummy-host.smiky</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; DocumentRoot "D:/work/branch/www"</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ServerName dummy-host.<span style="font-size:12px">smiky</span></p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ServerAlias www.dummy-host.<span style="font-size:12px">smiky</span></p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; ErrorLog "logs/dummy-host.smiky.log"</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; CustomLog "logs/dummy-host.smiky-access.log" common</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;/VirtualHost&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">配好后，再回到httpd.conf中为这两个目录添加权限</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;Directory "D:/work/trunk/www"&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Options Indexes FollowSymLinks</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; AllowOverride None</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Order allow,deny</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Allow from all</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;/Directory&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;Directory "D:/work/branch/www"&gt;</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Options Indexes FollowSymLinks</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; AllowOverride None</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Order allow,deny</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&nbsp; &nbsp; Allow from all</p>
<p style="margin-top:0px;margin-bottom:0px;padding:0px;font-family:Helvetica, Tahoma, Arial, sans-serif">&lt;/Directory&gt;</p>
<br/>
</div>
