---
layout: post
title: Ubuntu环境下挂载新硬盘  （摘自网络）
tags:
- ubuntu
- 挂载
description: 硬盘挂载 ubuntu
---
<div>
<div>&nbsp;<span style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">Ubuntu系统的硬盘空间不够用了，需要增加新的硬盘扩容。将硬盘分区、格式化、自动挂载配置的整个过程记下来，备忘。</span><br/>
</div>
<br style="color:rgb(82, 82, 82);font-family:'Hiragino Sans GB W3', 'Hiragino Sans GB', Arial, Helvetica, simsun, u5b8bu4f53;font-size:16px"><h2 style="color:rgb(50, 62, 50);border-bottom-color:rgb(60, 120, 181);font-size:21px;text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;border-width:0px 0px 1px;margin:13px 0px 8px;font-family:Arial, Helvetica, simsun, u5b8bu4f53;padding:6px 0.5ex 4px">一、硬盘分区 | Hard disk add new partition<br/>
</h2><span style="color:rgb(50, 62, 50);font-weight:bold;word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">1、显示硬盘及所属分区情况。在终端窗口中输入如下命令：</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;color:rgb(50, 62, 50);margin-top:5px;font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo fdisk -lu<br/>
</span></div>
<span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">显示当前的硬盘及所属分区的情况。如下图所示：</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">系统提示：DIsk /dev/sdb doesn't contain a valid partition table。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img2.ph.126.net/7jsa20uwo1E7wd4r9sp8_w==/2051671105344452615.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>
<span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">２</span><b style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">、对硬盘进行分区。</b><span style="color:rgb(50, 62, 50);font-weight:bold;word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">在终端窗口中输入如下命令：</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;color:rgb(50, 62, 50);margin-top:5px;font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo fdisk /dev/sdb<br/>
</span></div>
<span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">如下图所示：</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">在Command (m for help)提示符后面输入m显示一个帮助菜单。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img2.ph.126.net/bKjCY_3yQzrL6b2tG1R9Xw==/2454743271993873614.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>
<span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">&nbsp;在Command (m for help)提示符后面输入n，执行 add a new partition 指令给硬盘增加一个新分区。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">&nbsp;出现Command action时，输入e，指定分区为扩展分区（extended）。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">&nbsp;出现Partition number(1-4)时，输入１表示只分一个区。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">&nbsp;后续指定起启柱面（cylinder）号完成分区。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img1.ph.126.net/uwZJxe0S9jFDqHOKaTvP0Q==/1254533971299502156.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>
<span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">在Command (m for help)提示符后面输入p，显示分区表。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">系统提示如下：</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">Device Boot&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Start&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;End&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Blocks&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Id&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;System</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">/dev/sdb1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;26108&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;209712478+&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Extended</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img1.ph.126.net/1a-F5DeVST66ZqM-vy3cUA==/2075877953341246066.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>
<span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">在Command (m for help)提示符后面输入w，保存分区表。</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="color:rgb(50, 62, 50);word-wrap:normal;font-family:Arial, Helvetica, simsun, u5b8bu4f53;word-break:normal">系统提示：The partition table has been altered!</span><br style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div style="color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img2.ph.126.net/cAx1rtEpRD00epKCQn7PZg==/4860228422963141285.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>

<div style="font-size:16px;color:rgb(50, 62, 50);font-family:Arial, Helvetica, simsun, u5b8bu4f53"><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">在终端窗口中输<b>入</b>如下命令：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo fdisk -lu<br/>
</span></div>
<span style="font-size:14px">如下图所示：</span><br/>
<span style="font-size:14px">系统已经识别了硬盘 /dev/sdb 的分区。</span><br/>

<div style="font-size:14px">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img1.ph.126.net/eT8uKnfQyB76J0wrIYLl6w==/3315775225751770357.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>
<h2 style="border-bottom-color:rgb(60, 120, 181);text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;font-size:21px;border-width:0px 0px 1px;margin:13px 0px 8px;padding:6px 0.5ex 4px">二、硬盘格式化 | Format hard disk<br/>
</h2><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">1、显示硬盘及所属分区情况。在终端窗口中输入如下命令</span><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo mkfs -t ext4 /dev/sdb<br/>
</span></div>
<span style="font-size:14px">说明：</span><br/>
<span style="font-size:14px">-t ext4 表示将分区格式化成ext4文件系统类型。</span><br/>

<div style="font-size:14px">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img1.ph.126.net/17pLjlhlq1uWegjUzKj0Tw==/837669530791079719.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;&nbsp;</div>
<h2 style="border-bottom-color:rgb(60, 120, 181);text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;font-size:21px;border-width:0px 0px 1px;margin:13px 0px 8px;padding:6px 0.5ex 4px">三、挂载硬盘分区 | Mount hard disk partition<br/>
</h2><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">1、显示硬盘挂载情况。在终端窗口中输入如下命令</span><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo df -l<br/>
</span></div>
<span style="font-size:14px">新硬盘分区没有挂载，无法进入和查看。</span><br/>
<br/>
<span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">在终端窗口中输入如下命令</span><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo mount -t ext4 /dev/sdb /devdata<br/>
</span></div>
<span style="font-size:14px">说明：</span><br/>
<span style="font-size:14px">指定硬盘分区文件系统类型为ext4 ，同时将 /dev/sdb 分区挂载到目录 /devdata。</span><br/>
<br/>
<span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">再次在终端窗口中输入如下命令</span><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><span style="font-weight:bold;word-wrap:normal;word-break:normal">sudo df -l<br/>
</span></div>
<br/>
<span style="font-size:14px">新硬盘分区已经挂载，如下图最下面的红色方框内容。</span><br/>

<div style="font-size:14px">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img1.ph.126.net/W-aEpu0IER3f1_uEhm2uCg==/6598281428472181297.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>
&nbsp;</div>
<span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">２、配置硬盘在系统启动自动挂载。在文件 /etc/fstab 中加入如下配置</span><span style="font-weight:bold;font-size:14px;word-wrap:normal;word-break:normal">：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><span style="font-weight:bold;word-wrap:normal;word-break:normal"># /devdata was on /dev/sdb<br/>
UUID=37eaa526-5d96-4237-8468-603df5216ce9&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/devdata&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ext4&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;defaults&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3<br/>
</span></div>

<div>
<div style="font-size:14px"><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img0.ph.126.net/GpPTqXmp_-mgkwcyIrqc0A==/6599278685516925642.jpg" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>

<div style="font-size:14px"><br/>
</div>

<div><b><span style="font-size:14px">==========================&nbsp;</span><font size="3">&nbsp;/etc/fstab 配置 注意！！</font><span style="font-size:14px">==========================</span></b></div>

<div style="font-size:14px">这一部分<span>我</span><span>的设置与原作者不同，用上面无法成功自动挂载的话，请尝试下面的配置：</span></div>

<div style="font-size:14px">
<div><img alt="Ubuntu环境下挂载新硬盘 - 乂乂 - 分享，态度 ·~～" src="http://img0.ph.126.net/d2Hz1QvMjheL00IOQJzBpg==/4939322891418764424.png" style="border:0px;max-width:100%;height:auto;margin:0px 10px 0px 0px"></div>

<div>其中，“/Projects”与“/AMAX”都是挂载点，可以根据需要自定义。</div>

<div>上面是ext4格式硬盘为例子。如果是ntfs，则“ext4”要修改为“auto”，“defaults”要修改为“nosuid,nodev,nofail”。</div>

<div>在Ubuntu，可以使用更直观的方式：用系统自带的Disks或者Disk Utility界面化工具。</div>
</div>

<div style="font-size:14px"><b>=========================================================================</b></div>
<span style="font-size:14px">&nbsp;</span></div>
<h2 style="border-bottom-color:rgb(60, 120, 181);text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;font-size:21px;border-width:0px 0px 1px;margin:13px 0px 8px;padding:6px 0.5ex 4px">附录1：fdisk命令详解 | Appendix part 1:&nbsp;&nbsp;fdisk command syntax<br/>
</h2><span style="font-size:14px">fdisk 命令的语法如下：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><b>fdisk [-b sectorsize] device&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br/>
fdisk -l [-u] [device...]<br/>
fdisk -s partition...<br/>
fdisk -v</b>&nbsp;</div>
<span style="font-size:14px">说明：</span><br/>
<ul style="margin:5px 0px 5px 40px;padding:0px;border:0px;list-style:none;font-size:14px"><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-b &nbsp;&nbsp;&nbsp;指定每个分区的大小。也可以执行fdisk device（如：fdisk /dev/sdb）后，在系统提示时指定。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-l&nbsp;&nbsp;&nbsp;&nbsp;列出指定的外围设备的分区表状况。如果仅执行 fdisk -l ，系统会列出已知的分区。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-u&nbsp;&nbsp;&nbsp;搭配"-l"参数列表，会用分区数目取代柱面数目，来表示每个分区的起始地址。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-s &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;将指定的分区的大小输出到标准输出上，单位为区块。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-v&nbsp;&nbsp;&nbsp;显示fdisk的版本信息。</li></ul><br/>
<h2 style="border-bottom-color:rgb(60, 120, 181);text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;font-size:21px;border-width:0px 0px 1px;margin:13px 0px 8px;padding:6px 0.5ex 4px">附录2：mkfs命令详解 | Appendix part 2:&nbsp;&nbsp;mkfs command syntax<br/>
</h2><span style="font-size:14px">mkfs 命令的语法如下：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><b>mkfs [-V] [-t fstype] [fs-options] filesys</b></div>
<span style="font-size:14px">说明：</span><br/>
<ul style="margin:5px 0px 5px 40px;padding:0px;border:0px;list-style:none;font-size:14px"><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-V&nbsp;&nbsp;&nbsp;显示简要的使用方法。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-t &nbsp;&nbsp;&nbsp;指定要建立何种文件系统，如：ext3, ext4。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">fs&nbsp;&nbsp;&nbsp;指定建立文件系统时的参数。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-v&nbsp;&nbsp;&nbsp;显示版本信息与详细的使用方法。</li></ul><br/>
<h2 style="border-bottom-color:rgb(60, 120, 181);text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;font-size:21px;border-width:0px 0px 1px;margin:13px 0px 8px;padding:6px 0.5ex 4px">附录3：mount命令详解 | Appendix part 3:&nbsp;&nbsp;mount command syntax<br/>
</h2><span style="font-size:14px">mkfs 命令的语法如下：</span><br/>

<div style="border:1px solid rgb(60, 120, 181);margin-bottom:5px;font-size:14px;margin-top:5px"><b>mount [-afFnrsvw] [-t vfstype]&nbsp;</b><b>[-L label]&nbsp;&nbsp;</b><b>[-o options] device dir<br/>
mount [-lhv]</b></div>
<span style="font-size:14px">说明：</span><br/>
<ul style="margin:5px 0px 5px 40px;padding:0px;border:0px;list-style:none;font-size:14px"><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-a &nbsp;&nbsp;&nbsp;加载文件/etc/fstab中设置的所有设备。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-f&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;不实际加载设备。可与-v等参数同时使用以查看mount的执行过程。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-F&nbsp;&nbsp;&nbsp;&nbsp;需与-a参数同时使用。所有在/etc/fstab中设置的设备会被同时加载，可加快执行速度。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-t vfstype&nbsp;&nbsp;&nbsp;指定加载的文件系统类型，如：ext3, ext4。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-L label&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;给挂载点指定一个标签名称。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-l&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;显示分区的label。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-h&nbsp;&nbsp;&nbsp;&nbsp;显示帮助信息。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">-v&nbsp;&nbsp;&nbsp;&nbsp;显示mount的版本信息。<br/>
</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">device&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;要挂载的分区或文件。如果device是一个文件，挂载时须加上 -o loop参数。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">dir&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;分区的挂载点。</li></ul><br/>
<h2 style="border-bottom-color:rgb(60, 120, 181);text-shadow:rgb(153, 153, 153) 0.2em 0.2em 3px;list-style:none;border-bottom-style:solid;font-size:21px;border-width:0px 0px 1px;margin:13px 0px 8px;padding:6px 0.5ex 4px">附录4：fstab配置详解 | Appendix part 4:&nbsp;&nbsp;fstab detail configuration<br/>
</h2><span style="font-size:14px">/etc/fstab 中一共有６列：</span><br/>
<ul style="margin:5px 0px 5px 40px;padding:0px;border:0px;list-style:none;font-size:14px"><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">file system：指定要挂载的文件系统的设备名称（如：/dev/sdb）。也可以采用UUID，UUID可以通过使用blkid命令来查看（如：blkid&nbsp;&nbsp;/dev/sdb）指定设备的UUID号。<br/>
</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">mount point：挂载点。就是自己手动创建一个目录，然后把分区挂载到这个目录下。</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">type：用来指定文件系统的类型。如：ext3, ext4, ntfs等。<br/>
</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">option dump：０表示不备份；１表示要将整个中的内容备份。此处建议设置为０。<br/>
</li><li style="border:0px;list-style:disc;margin:0px 0px 0px 30px;padding:0px">pass：用来指定fsck如何来检查硬盘。０表示不检查；挂载点为分区／（根分区）必须设置为１，其他的挂载点不能设置为１；如果有挂载ass设置成大于1的值，则在检查完根分区后，然后按pass的值从小到大依次检查，相同数值的同时检查。如：/home　和 /boot　的pass 设置成2，/devdata 的pass 设置成3，则系统在检查完根分区，接着同时检查/boot和/home，再检查/devdata。</li></ul></div>
<br/>
</div>
