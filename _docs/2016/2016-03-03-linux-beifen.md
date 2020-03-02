---
title: linux-自动远程备份mysql与文件
category: 2016
order: 89
date: 2016-03-03
---
最近我遇到一个项目，甲方是政府的。他们要采用自购托管方式的服务器，以前都是用的云服务器系列。我做了一个项目的远程自动备份以防止服务器宕机。
<br>
具体方案是： 
<br>
1，写一个shell脚本，实现导出数据库，压缩项目文件。删除一段时间以前的备份
<br>
2，用crontab实现自动运行写脚本，将代码与数据库上传到备份的服务器，就算机房爆炸都不怕。
<br>
20 14 * * * /www/web/mycms/public_html/beifen/beifen.sh
<pre class="code">
#备份数据库
mysqldump -uroot -pLdbz_2016 -hlocalhost -R mycms > /www/web/mycms/public_html/beifen/mycms`date "+%Y%m%d"`.sql
#删除5天以前的备份
find /www/web/mycms/public_html/beifen/ -mtime +5 -name '*.sql' -exec rm -rf {} \;

#备份文件
tar zcvf /www/web/mycms/public_html/beifen/mycms`date "+%Y%m%d"`.tar.gz /www/web/mycms/public_html/mycms
#删除5天以前的备份代码文件
find /www/web/mycms/public_html/beifen/ -mtime +5 -name '*.tar.gz' -exec rm -rf {} \;

#上传到备份服务器
ftp -n< < !
#上面的小于符号与感叹号把空格删了
open 服务器ip
user 用户名 密码
binary
cd /public_html/ftp_bak
lcd /www/web/mycms/public_html/beifen
prompt
put mycms`date "+%Y%m%d"`.sql
put mycms`date "+%Y%m%d"`.tar.gz
delete mycms`date "+%Y%m%m" "5 days ago"`.sql
delete mycms`date "+%Y%m%m" "5 days ago"`.tar.gz
close
bye
!
</pre>

在其中遇到了好几个问题：<br>
<h2>问题1</h2>
,先写了导出数据库的脚本的时候，发现直接用命令 sh 脚本  这样不能运行成功，因为是在网上复制的代码。后台经过反复的修改测试发现，把头部的#!/bin/bash  
删掉就好了，这个是指此脚本使用的解释执行，说实话我并不知道为什么删了就可以运行成功，反正是成功，记得多测试。
<h2>问题2</h2>
，导出数据库的脚本写好了，直接sh 脚本文件，没有问题了，但是用crontab定时运行，导出的数据库为0kb。 经过查看crontan的日志文件提示：mysqldump: command not found。说明mysqldump 命令没找到，需要找出mysql的安装命令，将mysqldump注册到/usr/bin 下面。  <br>
<font color"red">注意：</font>网上有很多的这个问题的解决方法，他们说的都不是适用于我这台服务器。
首先得知道mysql命令或mysqldump命令的完整路径，可以使用find命令查找
除非你知道mysql安装路径可以略过这一步。<br>
<pre class="code">find  / -name mysql -print</pre><br>
例如我的mysql的路径是：/usr/local/mysql/bin/mysql,然后映射一个链接到/usr/bin目录下，相当于建立一个链接文件<br>
<pre class="code">ln -fs /usr/local/mysql/bin/mysql /usr/bin</pre><br>
上面这句就不对，应该是<br>
<pre class="code">ln -fs /usr/local/mysql/bin/mysql/mysqldump /usr/bin</pre><br>
这样才能将mysqldump注册到/usr/bin目录下<br>
<h2>问题3</h2>
,shell ftp上传的也有很多，我在网上复制代码怎么也运行不成功，也不知道为什么，最后参考网上的，用vi命令重新写了一篇才运行成功了。<br>
