---
layout: post
title: Redis实践操作之—— keyspace notification（键空间通知）（key过期通知，定时通知功能）
tags:
- redis
- key过期通知
- 定时通知
description: redis key过期通知 定时通知
---
<div>
	<div>摘自网络<a href="http://www.cnblogs.com/tinywan/p/5903988.html">http://www.cnblogs.com/tinywan/p/5903988.html</a></div>
<div class="postBody">
			<div id="cnblogs_post_body"><h1><span style="font-size: 16px;"><a href="https://github.com/Tinywan/PHP_Experience" target="_blank">源码地址：https://github.com/Tinywan/PHP_Experience</a></span></h1>
<hr>
<h1><span style="font-size: 16px;">&nbsp;</span></h1>
<h2><span style="font-size: 16px;">一、需求分析：</span></h2>
<ol>
<li><span style="font-size: 16px;">设置了生存时间的Key，在过期时能不能有所提示？</span></li>
<li><span style="font-size: 16px;">如果能对过期Key有个监听，如何对过期Key进行一个回调处理？</span></li>
<li><span style="font-size: 16px;">如何使用 Redis 来实现定时任务？</span></li>
</ol>
<h2><span style="font-size: 16px;">二、序言：</span></h2>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp; &nbsp;本文所说的定时任务或者说计划任务并不是很多人想象中的那样，比如说每天凌晨三点自动运行起来跑一个脚本。这种都已经烂大街了，随便一个 Crontab 就能搞定了。</span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp; &nbsp;这里所说的定时任务可以说是计时器任务，比如说用户触发了某个动作，那么从这个点开始过二十四小时我们要对这个动作做点什么。那么如果有 1000 个用户触发了这个动作，就会有 1000 个定时任务。于是这就不是 Cron 范畴里面的内容了。</span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp; &nbsp;举个最简单的例子，一个用户推荐了另一个用户，我们定一个二十四小时之后的任务，看看被推荐的用户有没有来注册，如果没注册就给他搞一条短信过去。</span></p>
<h2><span style="font-size: 16px;">三、Redis介绍</span></h2>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp; &nbsp;在 Redis 的 2.8.0 版本之后，其推出了一个新的特性——键空间消息（Redis Keyspace Notifications），它配合 2.0.0 版本之后的 SUBSCRIBE 就能完成这个定时任务</span></p>
<p><span style="font-size: 16px;">的操作了，不过定时的单位是秒。</span></p>
<p><span style="font-size: 16px;"><strong>（1）Publish / Subscribe&nbsp;</strong></span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp;Redis 在 2.0.0 之后推出了 Pub / Sub 的指令，大致就是说一边给 Redis 的特定频道发送消息，另一边从 Redis 的特定频道取值——形成了一个简易的消息队列。</span></p>
<p><span style="font-size: 16px;"><strong>（2）Redis Keyspace Notifications</strong></span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp; 在 Redis 里面有一些事件，比如键到期、键被删除等。然后我们可以通过配置一些东西来让 Redis 一旦触发这些事件的时候就往特定的 Channel 推一条消息。</span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp;大致的流程就是我们给 Redis 的某一个 db 设置过期事件，使其键一旦过期就会往特定频道推消息，我在自己的客户端这边就一直消费这个频道就好了。</span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp;以后一来一条定时任务，我们就把这个任务状态压缩成一个键，并且过期时间为距这个任务执行的时间差。那么当键一旦到期，就到了任务该执行的时间，Redis 自然会把过期消息推去，我们的客户端就能接收到了。这样一来就起到了定时任务的作用。</span></p>
<h2><span style="font-size: 16px;">&nbsp;四、Key过期事件的Redis配置</span></h2>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp;<strong>&nbsp;<span style="color: #339966;">这里需要配置 notify-keyspace-events 的参数为 “Ex”。x 代表了过期事件。notify-keyspace-events "Ex" 保存配置后，重启Redis服务，使配置生效</span>。</strong></span><br><span style="font-size: 16px;">重启Reids服务器：</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">root@iZ23s8agtagZ:/etc/redis<span style="color: #008000;">#</span><span style="color: #008000;"> service redis-server restart redis.conf</span>
Stopping redis-server: redis-server.<span style="color: #000000;">
Starting redis</span>-server: redis-server.</span></pre>
</div>
<p><span style="font-size: 16px;">添加过期事件订阅 开启一个终端，redis-cli 进入 redis 。开始订阅所有操作，等待接收消息。</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">tinywan@iZ23a7607jaZ:~$ redis-cli -h 127.0.01.4 -p 63789
127.0.0.1:63789&gt; psubscribe __keyevent@0__:<span style="color: #000000;">expired
Reading messages</span>... (press Ctrl-<span style="color: #000000;">C to quit)
</span>1) "psubscribe"
2) "__keyevent@0__:expired"
3) (<span style="color: #0000ff;">integer</span>) 1</span></pre>
</div>
<p><span style="font-size: 16px;">再开启一个终端，redis-cli 进入 redis，新增一个 20秒过期的键：</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">1270.01.1.1:63789&gt; SETEX coolName 123 20<span style="color: #000000;">
OK
</span>121.41.188.109:63789&gt;<span style="color: #000000;"> get coolName
</span>"20"
121.41.188.109:63789&gt;<span style="color: #000000;"> ttl coolName
(</span><span style="color: #0000ff;">integer</span>) 104</span></pre>
</div>
<p><span style="font-size: 16px;">另外一边执行了阻塞订阅操作后的终端，20秒过期后有如下信息输出：</span></p>
<div class="cnblogs_code"><div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div>
<pre><span style="color: #000000; font-size: 16px;">121.141.188.209:63789&gt; psubscribe __keyevent@0__:expired
Reading messages... (press Ctrl-C to quit)
1) "psubscribe"
2) "__keyevent@0__:expired"
3) (integer) 1
1) "pmessage"
2) "__keyevent@0__:expired"
3) "__keyevent@0__:expired"
4) "coolName"</span></pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div></div>
<p><span style="font-size: 16px;">说明：说明对过期Key信息的订阅是成功的。</span></p>
<h2><span style="font-size: 16px;">&nbsp;五、PHPREDIS实现订阅Keyspace notification</span></h2>
<p><span style="font-size: 16px;">&nbsp;Redis实例化类：(RedisInstance.class.php)</span></p>
<div class="cnblogs_code"><div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div>
<pre><span style="font-size: 16px;">&lt;?<span style="color: #000000;">php

</span><span style="color: #0000ff;">class</span><span style="color: #000000;"> RedisInstance
{
    </span><span style="color: #0000ff;">private</span> <span style="color: #800080;">$redis</span><span style="color: #000000;">;

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">function</span> __construct(<span style="color: #800080;">$host</span> = '121.41.88.209', <span style="color: #800080;">$port</span> = 63789<span style="color: #000000;">)
    {
        </span><span style="color: #800080;">$this</span>-&gt;redis = <span style="color: #0000ff;">new</span><span style="color: #000000;"> Redis();
        </span><span style="color: #800080;">$this</span>-&gt;redis-&gt;connect(<span style="color: #800080;">$host</span>, <span style="color: #800080;">$port</span><span style="color: #000000;">);
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">function</span> expire(<span style="color: #800080;">$key</span> = <span style="color: #0000ff;">null</span>, <span style="color: #800080;">$time</span> = 0<span style="color: #000000;">)
    {
        </span><span style="color: #0000ff;">return</span> <span style="color: #800080;">$this</span>-&gt;redis-&gt;expire(<span style="color: #800080;">$key</span>, <span style="color: #800080;">$time</span><span style="color: #000000;">);
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">function</span> psubscribe(<span style="color: #800080;">$patterns</span> = <span style="color: #0000ff;">array</span>(), <span style="color: #800080;">$callback</span><span style="color: #000000;">)
    {
        </span><span style="color: #800080;">$this</span>-&gt;redis-&gt;psubscribe(<span style="color: #800080;">$patterns</span>, <span style="color: #800080;">$callback</span><span style="color: #000000;">);
    }

    </span><span style="color: #0000ff;">public</span> <span style="color: #0000ff;">function</span><span style="color: #000000;"> setOption()
    {
        </span><span style="color: #800080;">$this</span>-&gt;redis-&gt;setOption(\Redis::OPT_READ_TIMEOUT,-1<span style="color: #000000;">);
    }

}</span></span></pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div></div>
<p><span style="font-size: 16px;">过期事件的订阅：（psubscribe.php）</span></p>
<div class="cnblogs_code"><div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div>
<pre><span style="font-size: 16px;">&lt;?<span style="color: #000000;">php
</span><span style="color: #0000ff;">require_once</span> './RedisInstance.class.php'<span style="color: #000000;">;
</span><span style="color: #800080;">$redis</span> = <span style="color: #0000ff;">new</span><span style="color: #000000;"> \RedisInstance();<br>// 解决Redis客户端订阅时候超时情况
</span><span style="color: #800080;">$redis</span>-&gt;<span style="color: #000000;">setOption();
</span><span style="color: #800080;">$redis</span>-&gt;psubscribe(<span style="color: #0000ff;">array</span>('__keyevent@0__:expired'), 'psCallback'<span style="color: #000000;">);
</span><span style="color: #008000;">//</span><span style="color: #008000;"> 回调函数,这里写处理逻辑</span>
<span style="color: #0000ff;">function</span> psCallback(<span style="color: #800080;">$redis</span>, <span style="color: #800080;">$pattern</span>, <span style="color: #800080;">$chan</span>, <span style="color: #800080;">$msg</span><span style="color: #000000;">)
{
    </span><span style="color: #0000ff;">echo</span> "Pattern: <span style="color: #800080;">$pattern</span>\n"<span style="color: #000000;">;
    </span><span style="color: #0000ff;">echo</span> "Channel: <span style="color: #800080;">$chan</span>\n"<span style="color: #000000;">;
    </span><span style="color: #0000ff;">echo</span> "Payload: <span style="color: #800080;">$msg</span>\n\n"<span style="color: #000000;">;
}</span></span></pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div></div>
<p><span style="font-size: 16px;">说明：psCallback 函数为订阅事件后的回调函数。<span class="MathJax_Preview" style="color: inherit;"></span><span class="MathJax" id="MathJax-Element-1-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>r</mi><mi>e</mi><mi>d</mi><mi>i</mi><mi>s</mi><mo>,</mo></math>" role="presentation" style="position: relative;"><nobr aria-hidden="true"><span class="math" id="MathJax-Span-1" role="math" style="width: 3.202em; display: inline-block;"><span style="display: inline-block; position: relative; width: 2.552em; height: 0px; font-size: 125%;"><span style="position: absolute; clip: rect(1.453em 1002.5em 2.652em -999.998em); top: -2.298em; left: 0.003em;"><span class="mrow" id="MathJax-Span-2"><span class="mi" id="MathJax-Span-3" style="font-family: MathJax_Math-italic;">r</span><span class="mi" id="MathJax-Span-4" style="font-family: MathJax_Math-italic;">e</span><span class="mi" id="MathJax-Span-5" style="font-family: MathJax_Math-italic;">d<span style="display: inline-block; overflow: hidden; height: 1px; width: 0.003em;"></span></span><span class="mi" id="MathJax-Span-6" style="font-family: MathJax_Math-italic;">i</span><span class="mi" id="MathJax-Span-7" style="font-family: MathJax_Math-italic;">s</span><span class="mo" id="MathJax-Span-8" style="font-family: MathJax_Main;">,</span></span><span style="display: inline-block; width: 0px; height: 2.302em;"></span></span></span><span style="display: inline-block; overflow: hidden; vertical-align: -0.309em; border-left: 0px solid; width: 0px; height: 1.253em;"></span></span></nobr><span class="MJX_Assistive_MathML" role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi>r</mi><mi>e</mi><mi>d</mi><mi>i</mi><mi>s</mi><mo>,</mo></math></span></span><script type="math/tex" id="MathJax-Element-1">redis, </script>pattern, <span class="MathJax_Preview" style="color: inherit;"></span><span class="MathJax" id="MathJax-Element-2-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>c</mi><mi>h</mi><mi>a</mi><mi>n</mi><mo>,</mo></math>" role="presentation" style="position: relative;"><nobr aria-hidden="true"><span class="math" id="MathJax-Span-9" role="math" style="width: 3.002em; display: inline-block;"><span style="display: inline-block; position: relative; width: 2.402em; height: 0px; font-size: 125%;"><span style="position: absolute; clip: rect(1.453em 1002.35em 2.652em -999.998em); top: -2.298em; left: 0.003em;"><span class="mrow" id="MathJax-Span-10"><span class="mi" id="MathJax-Span-11" style="font-family: MathJax_Math-italic;">c</span><span class="mi" id="MathJax-Span-12" style="font-family: MathJax_Math-italic;">h</span><span class="mi" id="MathJax-Span-13" style="font-family: MathJax_Math-italic;">a</span><span class="mi" id="MathJax-Span-14" style="font-family: MathJax_Math-italic;">n</span><span class="mo" id="MathJax-Span-15" style="font-family: MathJax_Main;">,</span></span><span style="display: inline-block; width: 0px; height: 2.302em;"></span></span></span><span style="display: inline-block; overflow: hidden; vertical-align: -0.309em; border-left: 0px solid; width: 0px; height: 1.253em;"></span></span></nobr><span class="MJX_Assistive_MathML" role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi>c</mi><mi>h</mi><mi>a</mi><mi>n</mi><mo>,</mo></math></span></span><script type="math/tex" id="MathJax-Element-2">chan, </script>msg 四个参数为回调时返回的参数。 详细说明见下面 Redis 官方文档对 psubscribe 的说明。</span></p>
<p><span style="font-size: 16px;">因为订阅事件启动后是阻塞执行的，所以我们尝试在终端执行 psubscribe.php 这个脚本。</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">D:\wamp\www\redistest&gt;php psubscribe.<span style="color: #000000;">php<br></span></span></pre>
</div>
<p><span style="font-size: 16px;">设置一个过期事件：</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">121.41.188.109:63789&gt; SETEX username123 20</span></pre>
</div>
<p><span style="font-size: 16px;">20秒过期时间到时，另外一边执行了脚本被阻塞的终端，有如下信息输出订阅结果：</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">D:\wamp\www\redistest&gt;php psubscribe.<span style="color: #000000;">php
Pattern</span>: __keyevent@0__:<span style="color: #000000;">expired
Channel</span>: __keyevent@0__:<span style="color: #000000;">expired
Payload</span>: username123</span></pre>
</div>
<p><span style="font-size: 16px;">以上PHP操作Reids是成功的</span></p>
<h2><span style="font-size: 16px;">&nbsp;六、使监听后台始终运行（订阅）</span></h2>
<p><span style="font-size: 16px;">&nbsp; &nbsp; &nbsp;有个问题 做到这一步，利用 phpredis 扩展，成功在代码里实现对过期 Key 的监听，并在 psCallback()里进行回调处理。 开头提出的两个需求已经实现。 可是这里有个问题：redis 在执行完订阅操作后，终端进入阻塞状态，需要一直挂在那。且此订阅脚本需要人为在命令行执行，不符合实际需求。</span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; 实际上，我们对过期监听回调的需求，是希望它像守护进程一样，在后台运行，当有过期事件的消息时，触发回调函数。 使监听后台始终运行 希望像守护进程一样在后台一样，</span></p>
<p><span style="font-size: 16px;">我是这样实现的。</span></p>
<p><span style="font-size: 16px;">&nbsp; &nbsp; Linux中有一个nohup命令。功能就是不挂断地运行命令。 同时nohup把脚本程序的所有输出，都放到当前目录的nohup.out文件中，如果文件不可写，则放到&lt;用户主目录&gt;/nohup.out 文件中。那么有了这个命令以后，不管我们终端窗口是否关闭，都能够让我们的php脚本一直运行。</span></p>
<p><span style="font-size: 16px;">编写PHP脚本文件：</span></p>
<div class="cnblogs_code"><div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div>
<pre><span style="font-size: 16px;">&lt;?<span style="color: #000000;">php
</span><span style="color: #008000;">#</span><span style="color: #008000;">! /usr/local/php/bin/php</span>
<span style="color: #0000ff;">require_once</span> './redis.class.php'<span style="color: #000000;">;
</span><span style="color: #800080;">$redis</span> = <span style="color: #0000ff;">new</span><span style="color: #000000;"> MyRedis();
</span><span style="color: #800080;">$redis</span>-&gt;<span style="color: #000000;">setOption();
</span><span style="color: #800080;">$redis</span>-&gt;psubscribe(<span style="color: #0000ff;">array</span>('__keyevent@0__:expired'), 'psCallback'<span style="color: #000000;">);
</span><span style="color: #008000;">//</span><span style="color: #008000;"> 回调函数,这里写处理逻辑</span>
<span style="color: #0000ff;">function</span> psCallback(<span style="color: #800080;">$redis</span>, <span style="color: #800080;">$pattern</span>, <span style="color: #800080;">$chan</span>, <span style="color: #800080;">$msg</span><span style="color: #000000;">)
{
    </span><span style="color: #0000ff;">echo</span> "Pattern: <span style="color: #800080;">$pattern</span>\n"<span style="color: #000000;">;
    </span><span style="color: #0000ff;">echo</span> "Channel: <span style="color: #800080;">$chan</span>\n"<span style="color: #000000;">;
    </span><span style="color: #0000ff;">echo</span> "Payload: <span style="color: #800080;">$msg</span>\n\n"<span style="color: #000000;">;
}</span></span></pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a href="javascript:void(0);" onclick="copyCnblogsCode(this)" title="复制代码"><img src="//common.cnblogs.com/images/copycode.gif" alt="复制代码"></a></span></div></div>
<p><span style="font-size: 16px;">注意：不过我们在开头，需要申明 php 编译器的路径：#! /usr/local/php/bin/php 。 这是执行 php 脚本所必须的。</span></p>
<p><span style="color: #ff0000; font-size: 16px;">然后，nohup 不挂起执行 nohupRedisNotify.php，注意 末尾的 &amp;</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">[root@chokingwin HiGirl]<span style="color: #008000;">#</span><span style="color: #008000;"> nohup ./nohupRedisNotify.php &amp; [1] 4456 nohup: ignoring input and appending output to `nohup.out'</span></span></pre>
</div>
<p><span style="font-size: 16px;">确认一下脚本是否已在后台运行。查看进程结果如下：&nbsp;</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">[root@chokingwin HiGirl]<span style="color: #008000;">#</span><span style="color: #008000;"> ps PID TTY TIME CMD 3943 pts/2 00:00:00 bash 4456 pts/2 00:00:00 nohupRedisNotif 4480 pts/2 00:00:00 </span></span></pre>
</div>
<p><span style="font-size: 16px;">说明：脚本确实已经在 4456 号进程上跑起来。&nbsp;</span></p>
<p><span style="font-size: 16px;">最后在查看下nohup.out cat 一下 nohuo.out，看下是否有过期输出：</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">[root@chokingwin HiGirl]<span style="color: #008000;">#</span><span style="color: #008000;"> cat nohup.out [root@chokingwin HiGirl]#</span></span></pre>
</div>
<p><span style="font-size: 16px;">并没有。我们还是老样子，新增一个10秒过期的的键 name。10秒后，我们再 cat 一次。&nbsp;</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">[root@chokingwin HiGirl]<span style="color: #008000;">#</span><span style="color: #008000;"> cat nohup.out Pattern: __keyevent@0__:expired Channel: __keyevent@0__:expired Payload: name</span></span></pre>
</div>
<p><span style="font-size: 16px;">说明监听过期事件并回调成功。</span></p>
<h1><span style="color: #ff0000; font-size: 16px;">nohup命令：记录详情：</span></h1>
<p><span style="font-size: 16px;"><strong>nohup没有输出的情况：</strong></span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">sudo nohup nohupRedisNotify.php &gt; /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>&gt;&amp;<span style="color: #800080;">1</span> &amp;</span></pre>
</div>
<p><span style="font-size: 16px;"><strong>查看jobs进程ID：[&nbsp;</strong>jobs -l&nbsp;<strong>]命令</strong></span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">www@iZ232eoxo41Z:~/tinywan $ jobs -<span style="color: #000000;">l
[</span><span style="color: #800080;">1</span>]-  <span style="color: #800080;">1365</span> Stopped (tty output)    sudo nohup nohupRedisNotify.php &gt; /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>&gt;&amp;<span style="color: #800080;">1</span><span style="color: #000000;">
[</span><span style="color: #800080;">2</span>]+  <span style="color: #800080;">1370</span> Stopped (tty output)    sudo nohup nohupRedisNotify.php &gt; /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>&gt;&amp;<span style="color: #800080;">1</span></span></pre>
</div>
<p><span style="font-size: 16px;"><strong>linux&nbsp;kill进程杀不掉（解决办法）：</strong></span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;"><span style="color: #000000;">kill </span>-<span style="color: #800080;">9</span> PID</span></pre>
</div>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">www@iZ232eoxo41Z:~/tinywan $ sudo kill -<span style="color: #800080;">9</span> <span style="color: #800080;">1370</span><span style="color: #000000;">
[</span><span style="color: #800080;">2</span>]+  Killed                  sudo nohup nohupRedisNotify.php &gt; /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>&gt;&amp;<span style="color: #800080;">1</span></span></pre>
</div>
<h2><span style="color: #ff0000; font-size: 16px;">&nbsp;如果是以后运行的后台程序的,命令【jobs -l】是没办法察觉任务以及PID的，这时候可以借助：ps aux | grep nohup 筛选哦,在使用 kill PID 铲除即可</span></h2>
<p><span style="color: #ff0000; font-size: 16px;">在这里是www用户已root运行的任务，用jobs -l</span></p>
<h2><span style="color: #008000; font-size: 16px;">jobs -l</span></h2>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">www@iZ232eoxo41Z:~ $ jobs -<span style="color: #000000;">l
www@iZ232eoxo41Z:</span>~ $ </span></pre>
</div>
<h2><span style="color: #008000; font-size: 16px;">ps aux | grep nohup&nbsp;</span></h2>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">www@iZ232eoxo41Z:~ $ ps aux |<span style="color: #000000;"> grep nohup
root     </span><span style="color: #800080;">18448</span>  <span style="color: #800080;">0.0</span>  <span style="color: #800080;">0.2</span>  <span style="color: #800080;">65160</span>  <span style="color: #800080;">2088</span> ?        S    <span style="color: #800080;">14</span>:<span style="color: #800080;">44</span>   <span style="color: #800080;">0</span>:<span style="color: #800080;">00</span> sudo nohup php ./<span style="color: #000000;">nohupRedisNotify.php
root     </span><span style="color: #800080;">18449</span>  <span style="color: #800080;">0.0</span>  <span style="color: #800080;">1.5</span> <span style="color: #800080;">283368</span> <span style="color: #800080;">15956</span> ?        S    <span style="color: #800080;">14</span>:<span style="color: #800080;">44</span>   <span style="color: #800080;">0</span>:<span style="color: #800080;">00</span> php ./<span style="color: #000000;">nohupRedisNotify.php
www      </span><span style="color: #800080;">18605</span>  <span style="color: #800080;">0.0</span>  <span style="color: #800080;">0.0</span>  <span style="color: #800080;">11740</span>   <span style="color: #800080;">928</span> pts/<span style="color: #800080;">1</span>    S+   <span style="color: #800080;">14</span>:<span style="color: #800080;">50</span>   <span style="color: #800080;">0</span>:<span style="color: #800080;">00</span> grep --color=auto nohup</span></pre>
</div>
<h2><span style="font-size: 16px;">&nbsp;经验分享环节：</span></h2>
<p><span style="font-size: 16px;">1、今天在Linux服务器执行一个php脚本nohup.php 挂起一个Redis订阅事件的时候，发现每次都不会调用API接口传递参数。但是直接执行的话（php nohup.php）的时候是 可以调用接口的，经过检查发现是权限的问题：</span></p>
<p><span style="font-size: 16px;">分析：当前登录用户为www用户：但是启动脚本的时候确实这样的（Root身份执行）：（可以在命令行直接执行，打印过期的事件key,并且输出可以作为调试哦！）</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">sudo nohup php nohupRedisNotify.php &gt; /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>&gt;&amp;<span style="color: #800080;">1</span> &amp;</span></pre>
</div>
<p><span style="font-size: 16px;">修改后的：</span></p>
<div class="cnblogs_code">
<pre><span style="font-size: 16px;">nohup php nohupRedisNotify.php &gt; /dev/<span style="color: #0000ff;">null</span> <span style="color: #800080;">2</span>&gt;&amp;<span style="color: #800080;">1</span> &amp;</span></pre>
</div>
<p><span style="font-size: 16px;">这样的话就直接可以回调自己写的API接口啦！</span></p>



		</div>