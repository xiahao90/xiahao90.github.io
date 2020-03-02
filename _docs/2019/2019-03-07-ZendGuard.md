---
title: PHP文件进行加密——ZendGuard加密与ZendLoader解密
category: 2019
order: 99
date:2019-03-07
---
------
<div class="post">
		<div class="clear"></div>
		<div class="postBody">
			<div id="cnblogs_post_body" class="blogpost-body"><div>转载：http://blog.csdn.net/lamp_yang_3533/article/details/49563425</div>
<div>Zend Guard是目前市面上最成熟的PHP源码加密产品。</div>
<div>经过本人搜集资料，亲身测试后，总结了如何利用Zend Guard对PHP文件进行加密，以及如何利用Zend Loader对加密后的PHP文件进行解密。</div>
<div>&nbsp;</div>
<div>我使用的是<strong><a href="http://pan.baidu.com/s/1eQlqlbG" target="_blank">Wampserver2.2</a></strong>，其中php的版本是5.3.10。（注意：这个里面自带的php版本属于TS版本，即Thread safety线程安全）</div>
<div>&nbsp;</div>
<div><strong>Zend Guard的安装及破解</strong></div>
<div><strong>&nbsp;</strong></div>
<div>点击下载&nbsp;<strong><a href="http://pan.baidu.com/s/1o6rL3ku" target="_blank">Zend Guard5.5.0</a></strong>，下载完成后，请自行傻瓜式安装。</div>
<div><br>破解需要注意以下几点：<br>　　1、本KEY的有效时间为2010年7月10号，因此激活时，请将自己电脑的系统时间调整到这个时间之前，如：2009-01-01<br>　　2、本KEY激活的为试用版，加密过的文件只有14天有效时间，因此在加密文件时，请将自己电脑的系统时间向后调整几年，如：2020-01-01<br>　　3、点击下载授权文件&nbsp;<a href="http://pan.baidu.com/s/1hq4OXl6" target="_blank"><strong>zend_guard授权文件.zip</strong></a>，解压得到zend_guard.zl，即激活用的文件<br>　　4、打开Zend Guard 5.5.0，[Help] -&gt; [Register] -&gt; [Search for a license file on my disk]，选择zend_guard.zl授权文件激活即可</div>
<div>
<h2>如何使用Zend Guard进行加密？</h2>
<p>&nbsp;</p>

</div>
<p>　　1、打开Zend Guard 5.5.0，[File]-&gt;[New]-&gt;[Zend Guard Project]，新建项目。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 弹出如下的对话框：&nbsp; &nbsp;&nbsp;<img alt=""></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="{{ site.baseurl }}static/blog/20151101230835572.png" alt=""></p>
<p>&nbsp;</p>
<div>
<p>2、点击 Next ，下一步。弹出如下对话框，选择要进行加密的源文件或文件夹。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 本步骤是选择要加密的文件，可以是单个文件[Add File]或整个文件夹[Add Folder]，然后[Next]。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;（此处，我选择的是对整个文件夹进行加密。即 D:\wamp\www\demo&nbsp;里的所有文件进行加密。）</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="{{ site.baseurl }}static/blog/20151101231038631.png" alt=""></p>

</div>
<div>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img alt=""><br>　　3、接下来是选择<a class="replace_word" title="PHP知识库" href="http://lib.csdn.net/base/php" target="_blank">PHP</a>的版本[与你web服务器上PHP的版本相对照]，这里很重要，版本不对会出错，[Finish]完成项目的创建。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 注意： 对于Zend Guard 5.5.0这个版本的加密软件，最高只可支持5.3版本的PHP。如果您的PHP版本较高，请到Zend Guard官网下载对应的高版本加密软件。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; （由于，我的PHP版本是PHP 5.3.10，故这里我选择PHP 5.3，其他地方可以默认，直接点击完成）</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img alt=""><img src="{{ site.baseurl }}static/blog/20151101230529672.png" alt=""></p>
<p>&nbsp;</p>
<p>4、在Zend Guard左侧的Guard Explorer中，可以看到你新建的项目了，鼠标选中项目名称后，右键单击[Encode Project]，完成。</p>
<p>&nbsp;</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<img src="{{ site.baseurl }}static/blog/20151101231154978.png" alt=""></p>
<p>&nbsp;</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; 如此，就实现了对PHP源码的最简单的加密。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; 我们可以在产品的输出目录（D:\productDir）里，看到加密后的文件。</p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img alt=""><img src="{{ site.baseurl }}static/blog/20151101231401303.png" alt=""></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="{{ site.baseurl }}static/blog/20151101231442331.png" alt=""></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;</p>
<p>可以看出，产品输出目录里的PHP文件已被加密了。</p>
<p>&nbsp;
这种最简单的加密方式，我们并没有设置加密的有效期，也没有设置许可证支持（即解密时，是否需要许可证文件），默认是永不过期，不需要解密许可文件。</p>
<p>如果要设置解密时的许可证文件，可以点击 项目名称（project_test) ，再点击 项目主窗口中的 Overview 旁边的 Security 选项卡，就可以进行更加安全的加密设置了。 如下图：</p>
<p>&nbsp;
&nbsp;&nbsp;<img src="{{ site.baseurl }}static/blog/20151101231828492.png" alt=""></p>
<p>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp;</p>
<div>&nbsp;</div>

</div>
<p>
<strong>Zend&nbsp;Loader&nbsp;解密</strong></p>
<div>&nbsp;</div>
<div>上面我们已经对PHP代码进行了最简单的加密（编码），加密后生成的PHP源代码，就不能再被web服务器上的PHP模块解析了。</div>
<div>当我们将加密后的php文件放到web服务器上执行时，会显示如下信息：</div>
<div>&nbsp;</div>
<div>Zend Guard Run-time support missing!<br>One more more files on this web site were encoded by ZendGuard and the required run-time support is not installed orproperly configured.<br>......</div>
<div>&nbsp;</div>
<div>原来，加密后的php代码需要ZendGuardLoader模块才能正常运行。<br>因为我的php版本是5.3.10的，所以我这里只提供了ZendGuardLoader-php-5.3-Windows，如果是其他版本的php，请自行百度。<br>点击下载<strong><a href="http://pan.baidu.com/s/1jGMvLH0" target="_blank">&nbsp;</a></strong><strong><a href="http://pan.baidu.com/s/1jGMvLH0" target="_blank">ZendGuardLoader-php-5.3-Windows</a></strong><br>下载后，解压压缩包，找到目录里的 ZendLoader.dll 文件，将它复制到你的php目录里的ext目录（PHP的扩展库目录）下，再编辑PHP的配置文件php.ini，添加如下代码：</div>
<div>&nbsp;</div>
<div>
<div>[Zend.loader],</div>
<div>zend_loader.enable=1</div>
<div>zend_loader.disable_licensing=1</div>
<div>zend_loader.obfuscation_level_support=3</div>
<div>zend_loader.license_path=</div>
<div>zend_extension="d:\wamp\bin\php\php5.3.10\ext\ZendLoader.dll"</div>

</div>
<div>&nbsp;</div>
<div>注：</div>
<div>zend_loader.enable &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 表示是否启用zend loader，1表示启用，0表示禁用</div>
<div>zend_loader.disable_licensing &nbsp; &nbsp; &nbsp; &nbsp;表示是否禁用许可证，1表示禁用，0表示不禁用 &nbsp; &nbsp;（由于上面我加密php文件的时候，没有设置许可证支持，故解密时，禁用许可证）</div>
<div>zend_loader.obfuscation_level_support &nbsp; &nbsp; 表示代码混淆级别</div>
<div>zend_loader.license_path &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;指定许可证文件的路径</div>
<div>zend_extension &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;指定zend loader 扩展文件 的路径</div>
<div>&nbsp;</div>
<div>配置完成后，重启wampserver，如果您的php的版本是NTS（非线程安全的话），就可以正常执行加密后的PHP文件了。</div>
<div>但是，多数情况下的php版本都是TS（线程安全）的，比如，我这个wampserver集成环境中的php是php-5.3.10-ts，它是没有办法支持Zend Guard Loader扩展文件的。故还会报出上面的错误提示信息。</div>
<div>因为，Zend Guard Loader 只能支持 NTS版本的php，终极解决办法是下载安装 NTS 版本的php。</div>
<div>&nbsp;</div>
<div>为了实现PHP源文件的 Zend 解密测试，可以下载安装&nbsp;<strong><a href="http://pan.baidu.com/s/1c0foFHe" target="_blank">php-5.3.29-nts-Win32-VC9-x86</a></strong>，再来配置 Zend Loader 扩展支持。</div>
<div>&nbsp;</div>
<div>如何查看安装好的PHP的版本及PHP是否已经成功支持Zend Loader？</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;
通过查看&nbsp;phpinfo() 函数的输出信息，利用 Ctrl + F 快速查找关键字Thread Safety 和&nbsp;Zend&nbsp;Guard&nbsp;Loader。 &nbsp;&nbsp;</div>
<div>&nbsp; &nbsp; &nbsp;&nbsp;&nbsp;如本人安装好php-5.3.29-nts版本的php后，phpinfo()的输出信息如下：</div>
<div>&nbsp;</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<img src="{{ site.baseurl }}static/blog/20151101232047295.png" alt=""></div>
<div>&nbsp;</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;可以看到，Thread Safety 对应的值为disabled，就说明该php的版本是 NTS （非线程安全）的，否则就是TS版本的。</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;输出信息中，还可以看到&nbsp;Zend&nbsp;Guard&nbsp;Loader&nbsp;v3.3 字样，说明 Zend Guard Loader 也安装成功了。</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;再往下面看，还可以查看&nbsp;Zend&nbsp;Guard&nbsp;Loader 的配置信息，如下：</div>
<div>&nbsp; &nbsp; &nbsp; &nbsp;<img src="{{ site.baseurl }}static/blog/20151101232133583.png" alt=""></div>
<div>&nbsp; &nbsp; &nbsp; 到此，整个&nbsp;Zend Guard 加密和&nbsp;&nbsp;Zend Guard Loader 解密，就介绍完毕了。</div>
<div>&nbsp;</div>
<div>&nbsp; &nbsp; &nbsp; 温馨提示： 为了顺利实现PHP代码的zend加密和解密，建议使用PHP官网上主流的PHP的NTS版和Zend官网上对应的Zend Guard加密软件、&nbsp;Zend Guard Loader解密插件。</div>
<div>&nbsp;</div>
<div>&nbsp; &nbsp; &nbsp; 相关附件：</div>
<div><strong><a href="http://pan.baidu.com/s/1eQlqlbG" target="_blank">Wampserver2.2</a></strong></div>
<div><a href="http://pan.baidu.com/s/1sjIdKPf" target="_blank"><strong>Zend
 Guard5.5.0</strong></a></div>
<div><strong><a href="http://pan.baidu.com/s/1sjpD5Vn" target="_blank">php-5.3.29-nts-Win32-VC9-x86</a></strong></div>
<div><strong><a href="http://pan.baidu.com/s/1c0G2zmg" target="_blank">mod_fcgid-2.3.6-win32-x86.zip</a></strong></div></div><div id="MySignature"></div>
<div class="clear"></div>
<div id="blog_post_info_block">
