---
layout: post
title: php极验滑动验证码
tags:
- 极验
- php
- 验证码
- 滑动验证码
description: 极验 php 验证码 滑动验证码
---
在滑动验证码刚刚出现的时候，我了解了此项技术。
<br>
一般网上有一些帖子也写了滑动验证码。但是有的试试基于前端用jqury做的。我们做验证码的目的是为了放置黑客的欺骗请求和暴力破解 。如果只是基于前端做的话，毫无意义。
<br>
此贴说的是基于<a href="http://www.geetest.com/">极验平台</a>做的图片滑动验证。
<br>
1，下载sdk ：git clone https://github.com/GeeTeam/gt-php-sdk.git
<br>
2，初始化验证，就是显示验证码这一步，用ajax调用。
<br>
<div class="highlight-php"><div class="highlight"><pre><span></span><span class="cp">&lt;?php</span>
<span class="k">require_once</span> <span class="nb">dirname</span><span class="p">(</span><span class="nb">dirname</span><span class="p">(</span><span class="k">__FILE__</span><span class="p">))</span> <span class="o">.</span> <span class="s1">'/lib/class.geetestlib.php'</span><span class="p">;</span>
<span class="k">require_once</span> <span class="nb">dirname</span><span class="p">(</span><span class="nb">dirname</span><span class="p">(</span><span class="k">__FILE__</span><span class="p">))</span> <span class="o">.</span> <span class="s1">'/config/config.php'</span><span class="p">;</span>
<span class="nv">$GtSdk</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">GeetestLib</span><span class="p">(</span><span class="nx">CAPTCHA_ID</span><span class="p">,</span> <span class="nx">PRIVATE_KEY</span><span class="p">);</span>
<span class="nb">session_start</span><span class="p">();</span>
<span class="nv">$user_id</span> <span class="o">=</span> <span class="s2">"test"</span><span class="p">;</span>
<span class="nv">$status</span> <span class="o">=</span> <span class="nv">$GtSdk</span><span class="o">-&gt;</span><span class="na">pre_process</span><span class="p">(</span><span class="nv">$user_id</span><span class="p">);</span>
<span class="nv">$_SESSION</span><span class="p">[</span><span class="s1">'gtserver'</span><span class="p">]</span> <span class="o">=</span> <span class="nv">$status</span><span class="p">;</span>
<span class="nv">$_SESSION</span><span class="p">[</span><span class="s1">'user_id'</span><span class="p">]</span> <span class="o">=</span> <span class="nv">$user_id</span><span class="p">;</span>
<span class="k">echo</span> <span class="nv">$GtSdk</span><span class="o">-&gt;</span><span class="na">get_response_str</span><span class="p">();</span>
<span class="cp">?&gt;</span><span class="x"></span>
</pre></div>
</div>
<br>
2，前端代码
<br>
<div class="code highlight-python"><div class="highlight"><pre><span></span>var handler = function (captchaObj) {
    // captchaObj为验证码对象
            // 将验证码加到id为captcha的元素里

    captchaObj.appendTo("#captcha");
};
$.ajax({

    // 获取id，challenge，success（是否启用failback）
    url: "StartCaptchaServlet?t=" + (new Date).getTime(), // 添加时间戳防止浏览器缓存
    type: "get",
    dataType: "json", // 使用json格式，如果不支持可换成jsonp等其它格式，需用户后台配合
    success: function (data) {

        // 使用initGeetest接口
        // 参数1：配置参数，与创建Geetest实例时接受的参数一致
        // 参数2：回调，回调的第一个参数验证码对象，之后可以使用它做appendTo之类的事件
        initGeetest({
            gt: data.gt,
            challenge: data.challenge,
            product: "popup", // 产品形式，可选择：float，embed，popup，仅PC版验证码有效，见上文配置参数小节
            offline: !data.success
        }, handler);
    }
});
</pre></div>
</div>
<br>
3，二次验证，就是提交后验证是否正确。
<div class="highlight-php"><div class="highlight"><pre><span></span><span class="cp">&lt;?php</span>
<span class="k">require_once</span> <span class="nb">dirname</span><span class="p">(</span><span class="nb">dirname</span><span class="p">(</span><span class="k">__FILE__</span><span class="p">))</span> <span class="o">.</span> <span class="s1">'/lib/class.geetestlib.php'</span><span class="p">;</span>
<span class="k">require_once</span> <span class="nb">dirname</span><span class="p">(</span><span class="nb">dirname</span><span class="p">(</span><span class="k">__FILE__</span><span class="p">))</span> <span class="o">.</span> <span class="s1">'/config/config.php'</span><span class="p">;</span>
<span class="nb">session_start</span><span class="p">();</span>
<span class="nv">$GtSdk</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">GeetestLib</span><span class="p">(</span><span class="nx">CAPTCHA_ID</span><span class="p">,</span> <span class="nx">PRIVATE_KEY</span><span class="p">);</span>
<span class="nv">$user_id</span> <span class="o">=</span> <span class="nv">$_SESSION</span><span class="p">[</span><span class="s1">'user_id'</span><span class="p">];</span>
<span class="k">if</span> <span class="p">(</span><span class="nv">$_SESSION</span><span class="p">[</span><span class="s1">'gtserver'</span><span class="p">]</span> <span class="o">==</span> <span class="mi">1</span><span class="p">)</span> <span class="p">{</span>
    <span class="nv">$result</span> <span class="o">=</span> <span class="nv">$GtSdk</span><span class="o">-&gt;</span><span class="na">success_validate</span><span class="p">(</span><span class="nv">$_POST</span><span class="p">[</span><span class="s1">'geetest_challenge'</span><span class="p">],</span> <span class="nv">$_POST</span><span class="p">[</span><span class="s1">'geetest_validate'</span><span class="p">],</span> <span class="nv">$_POST</span><span class="p">[</span><span class="s1">'geetest_seccode'</span><span class="p">],</span> <span class="nv">$user_id</span><span class="p">);</span>
    <span class="k">if</span> <span class="p">(</span><span class="nv">$result</span><span class="p">)</span> <span class="p">{</span>
        <span class="k">echo</span> <span class="s1">'Yes!'</span><span class="p">;</span>
    <span class="p">}</span> <span class="k">else</span><span class="p">{</span>
        <span class="k">echo</span> <span class="s1">'No'</span><span class="p">;</span>
    <span class="p">}</span>
<span class="p">}</span><span class="k">else</span><span class="p">{</span>
    <span class="k">if</span> <span class="p">(</span><span class="nv">$GtSdk</span><span class="o">-&gt;</span><span class="na">fail_validate</span><span class="p">(</span><span class="nv">$_POST</span><span class="p">[</span><span class="s1">'geetest_challenge'</span><span class="p">],</span><span class="nv">$_POST</span><span class="p">[</span><span class="s1">'geetest_validate'</span><span class="p">],</span><span class="nv">$_POST</span><span class="p">[</span><span class="s1">'geetest_seccode'</span><span class="p">]))</span> <span class="p">{</span>
        <span class="k">echo</span> <span class="s2">"yes"</span><span class="p">;</span>
    <span class="p">}</span><span class="k">else</span><span class="p">{</span>
        <span class="k">echo</span> <span class="s2">"no"</span><span class="p">;</span>
    <span class="p">}</span>
<span class="p">}</span>
<span class="cp">?&gt;</span><span class="x"></span>
</pre></div>
</div>


