---
title: Ubuntu 12.04安装NFS server
category: 2016
order: 85
date:2016-05-27
---
<br>
摘自网络：亲测<br>
本人配置此功能的目的是：实现局域网服务器的负载均衡，代码同步发布。
<br>
<p>首先安装nfs-kernel-server</p>
<p>&#65279;&#65279;&#65279;&#65279;apt-get install nfs-kernel-server&nbsp; </p>
<p>然后创建一个目录:</p>
<p>mkdir -p /opt/share&nbsp; </p>
<p>并赋予权限777:</p>
<p>chmod -R 777 /opt/share</p>
<p>在/etc/exports文件中添加配置：）</p>
<p>/opt/share *(rw,no_root_squash)&nbsp; </p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其中：/opt/share   是要共享的目录，</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*代表允许所有的网络段访问，</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;（如果填写了ip网段地址，有几台服务器则添加几条）</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rw是可读写权限,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sync是资料同步写入内存和硬盘，</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_root_squash是Ubuntu nfs客户端分享目录使用者的权限，如果客户端使用的是root用户，那么对于该共享目录而言，该客户端就具有root权限。
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;其它Ubuntu nfs常用的参数有：</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ro 只读访问</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;rw 读写访问sync 所有数据在请求时写入共享</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;async nfs在写入数据前可以响应请求</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;secure nfs通过1024以下的安全TCP/IP端口发送</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;insecure nfs通过1024以上的端口发送</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;wdelay 如果多个用户要写入nfs目录，则归组写入(默认)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_wdelay 如果多个用户要写入nfs目录，则立即写入，当使用async时，无需此设置。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;hide 在nfs共享目录中不共享其子目录</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_hide 共享nfs目录的子目录</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;subtree_check 如果共享/usr/bin之类的子目录时，强制nfs检查父目录的权限(默认)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_subtree_check 和上面相对，不检查父目录权限</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;all_squash 共享文件的UID和GID映射匿名用户anonymous，适合公用目录。</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_all_squash 保留共享文件的UID和GID(默认)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;root_squash root用户的所有请求映射成如anonymous用户一样的权限(默认)</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;no_root_squas root用户具有根目录的完全管理访问权限</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;anonuid=xxx 指定nfs服务器/etc/passwd文件中匿名用户的UID</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;anongid=xxx 指定nfs服务器/etc/passwd文件中匿名用户的GID</p>
<!-- <p>在/etc/hosts.allow中添加配置：</p>
<p>portmap:10.112.18.0/255.255.255.0&nbsp; </p>
<p>在/etc/hosts.deny中添加配置：</p>
<p>portmap:ALL&nbsp; </p> -->
<p>重新启动两个服务：</p>
<p>service portmap restart&nbsp; </p>
<p>service nfs-kernel-server restart</p>
<p>在另一台Ubuntu上安装nfs-common</p>
<p>apt-get install nfs-common&nbsp; </p>
<p>然后创建目录test</p>
<p>mkdir ~/test&nbsp; </p>
<p>然后运行测试命令看一下：</p>
<p># showmount -e nfs服务器ip&nbsp; </p>
<p>Export list for 10.112.18.158:&nbsp; </p>
<p>/opt/share 10.112.18.0&nbsp; </p>
<p>找到nfs server的挂载目录了。现在手工挂载：</p>
<p>mount -t nfs4 nfs服务器ip:/opt/share ~/test/&nbsp; </p>
<p>现在可以测试了，应该成功。<font>注：需要等待2分钟</font></p></div>