---
layout: post
title: Linux中设置服务自启动的三种方式（摘自网络）
tags:
- linux
- 开机启动
description: linux 开机启动
---
<div id="cnblogs_post_body"><p>有时候我们需要Linux系统在开机的时候自动加载某些脚本或系统服务</p>
<p>主要用三种方式进行这一操作：</p>
<p><span style="color: #008000;">ln -s&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; 在/etc/rc.d/rc*.d目录中建立/etc/init.d/服务的软链接(*代表0～6七个运行级别之一）</span></p>
<p><span style="color: #008000;">chkonfig&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 命令行运行级别设置</span></p>
<p><span style="color: #008000;">ntsysv&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 伪图形运行级别设置</span></p>
<p>&nbsp;</p>
<p><strong>注意：</strong>1.这三种方式主要用于以redhat为基础的发行版</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2.如果还不知道运行级别是什么，那么最好先看看相关资料再实验</p>
<p>&nbsp;</p>
<h4>第一种方式：ln -s 建立启动软连接</h4>
<p>在Linux中有7种运行级别（可在<em><span style="color: #ff0000;">/etc/inittab</span></em>文件设置），每种运行级别分别对应着<span style="color: #ff0000;"><em>/etc/rc.d/rc[0~6].d</em></span>这7个目录</p>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18151041-096d449a91fa48f2bf12c4a50d6d2d74.png" alt=""></p>
<p><strong>Tips：</strong><em><span style="color: #ff0000;">/etc/rc[0~6].d</span></em>其实是<em><span style="color: #ff0000;">/etc/rc.d/rc[0~6].d</span></em>的软连接，主要是为了保持和Unix的兼容性才做此策</p>
<p>&nbsp;</p>
<p>这7个目录中，每个目录分别存放着对应运行级别加载时需要关闭或启动的服务</p>
<p>由详细信息可以知道，其实每个脚本文件都对应着<em><span style="color: #ff0000;">/etc/init.d/</span></em>目录下具体的服务</p>
<p>K开头的脚本文件代表运行级别加载时需要关闭的，S开头的代表需要执行</p>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18151333-85c421f973864253b1e32f892fe8ec6f.png" alt=""></p>
<p>因此，当我们需要开机启动自己的脚本时，只需要将可执行脚本丢在<em><span style="color: #ff0000;">/etc/init.d</span></em>目录下，然后在<em><span style="color: #ff0000;">/etc/rc.d/rc*.d</span></em><span style="color: #000000;">中建立</span>软链接即可</p>
<div class="cnblogs_code">
<pre>[root@localhost ~]# <span style="color: #0000ff;">ln</span> -s /etc/init.d/sshd /etc/rc.d/rc3.d/S100ssh</pre>
</div>
<p>此处sshd是具体服务的脚本文件，S100ssh是其软链接，S开头代表加载时自启动</p>
<p>如果需要在多个运行级别下设置自启动，则需建立多个软链接</p>
<p>这种方式比较繁琐，适用于自定义的服务脚本</p>
<p>如果系统中已经存在某些服务（比如安装apache时就会有httpd服务项），可以使用下面的两种方式</p>
<p>&nbsp;</p>
<h4>第二种方式：chkconfig </h4>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18152738-449524068e104327bc9e3e61d1049bdb.png" alt=""></p>
<p>如果需要自启动某些服务，只需使用<em><span style="color: #ff0000;">chkconfig 服务名 on</span></em>即可，若想关闭，<em><span style="color: #ff0000;">将on改为off</span></em></p>
<p>在默认情况下，<em><span style="color: #ff0000;">chkconfig</span></em>会自启动<em><span style="color: #ff0000;">2345</span></em>这四个级别，如果想自定义可以加上<em><span style="color: #ff0000;">--level</span></em>选项</p>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18153144-0a443c680bad4964925caa140ce131e1.png" alt=""></p>
<p>上面我们先将sshd服务的所有启动级别关闭，然后使用<em><span style="color: #ff0000;">--level</span></em>选项启动自定义级别</p>
<p><strong>Tips：</strong><em><span style="color: #ff0000;">--list</span></em>选项可查看指定服务的启动状态，<em><span style="color: #ff0000;">chkconfig</span></em>不带任何选项则查看所有服务状态</p>
<p>&nbsp;</p>
<h4>第三种方式：ntsysv 伪图形</h4>
<p><em><span style="color: #ff0000;">ntsysv</span></em>和<span style="color: #ff0000;"><em>chkconfig</em></span>其实是一样的，只不过加上了图形而已</p>
<p>启动<em><span style="color: #ff0000;">ntsysv</span></em>有两种方式，一是直接在命令行中输入<em><span style="color: #ff0000;">ntsysv</span></em>，二是使用<em><span style="color: #ff0000;">setup</span></em>命令，然后选择系统服务</p>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18153614-7c42c0df7eec4c04b75578a21f83fc37.png" alt=""></p>
<p>默认情况下，当前运行级别为多少，在<em><span style="color: #ff0000;">ntsysv</span></em>中设置的启动服务的级别便是多少</p>
<p>比如，我当前的运行级别是3,那么我在伪图形界面中选择启动服务后，它的运行级别也会是3</p>
<p>如果想自定义运行级别可使用<em><span style="color: #ff0000;">ntsysv --level</span></em>方式</p>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18154058-87a724a3037c4870a48e66434caaa20f.png" alt=""></p>
<p>&nbsp;</p>
<p>以上三种操作需要保证服务脚本文件可执行，并且要有root权限</p>
<p>其中，第一种方式多用于自定义脚本，第二、三种多用于系统已存在的服务</p>
<p>比如ftp、samba、ssh、httpd等等</p>
<p>并且，要做相关设置需要弄清楚运行级别的问题</p>
<p>&nbsp;</p>
<p><strong>Tips：</strong>如果想手动启动某服务，传统的方式是 <em><span style="color: #ff0000;">/etc/init.d</span> <span style="color: #ff0000;">服务名 start</span></em></p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp; 实际上还可以这样，<em><span style="color: #ff0000;">service 服务名 start</span></em></p>
<p><img src="http://images.cnitblog.com/blog/476392/201301/18155018-03a4fcd414b543c2b06de3209d0866a2.png" alt=""></p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p>
<p>&nbsp;</p></div>