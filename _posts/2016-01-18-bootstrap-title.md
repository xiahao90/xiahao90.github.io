---
layout: post
title: 玩转Bootstrap-标题
tags:
- soap
- php
description: Bootstrap html css
---

<div id="js-aticle-container" class="cwrap-autoheight aticle-container" style="width: 400px;">
        <div class="code-panel" id="J_PanelCode">
            <h2 class="code-head" id="J_CodeLang" data-lang="HTML">标题（一）</h2>
            <div class="code-description" id="J_CodeDescr">
              <div class="code-desc co">
                <p><span style="font-size: 13px; line-height: 1.6em;">Bootstrap和普通的HTML页面一样，定义标题都是使用标签&lt;h1&gt;到&lt;h6&gt;,只不过Bootstrap覆盖了其默认的样式，使用其在所有浏览器下显示的效果一样，具体定义的规则可以如下表所示：</span></p>

<p><span style="font-size: 13px; line-height: 1.6em;"><a href="http://img.mukewang.com/53acce330001429807730337.jpg"><img alt="" src="http://img.mukewang.com/53acce330001429807730337.jpg" style="width: 350px;"></a></span></p>

<p>通过比较可以发现，Bootstrap标题样式进行了以下显著的优化重置：</p>

<p>1、重新设置了<strong>margin-top</strong>和<strong>margin-bottom</strong>的值， &nbsp;<strong>h1~h3</strong>重置后的值都是<strong>20px</strong>；<strong>h4~h6</strong>重置后的值都是<strong>10px。</strong><br>
2、所有标题的行高都是<strong>1.1</strong>（也就是font-size的1.1倍）,而且文本颜色和字体都继承父元素的颜色和字体。<br>
3、固定不同级别标题字体大小，<strong>h1=36px，h2=30px，h3=24px，h4=18px，h5=14px</strong>和<strong>h6=12px。</strong></p>

<p>标题的具体运用非常简单，和我们平时运用是一样的，使用&lt;h1&gt;~&lt;h6&gt;标签，分别表示标题一至标题六，h 后面的数字越大，表示级别越小，文本也越小。来看一个简单的效果：右侧代码编辑器中的10-16行的代码。</p>

<p>在Bootstrap中为了让非标题元素和标题使用相同的样式，还特意定义了<strong>.h1~.h6</strong>六个类名。<strong>如右侧代码编辑器中 &nbsp; 18-23行代码所示：</strong></p>

<p><span style="font-size: 13px; line-height: 1.6em;">对比两个示例的效果图，可以说他们的效果是一模一样的。</span></p>

<p>&nbsp;</p>

              </div>
              <div class="code-tips">
                <h3 class="code-tips-title">任务</h3>
                <div class="co"><p>我也来试一试：在右侧代码编辑器中输入：</p>

<pre class="code">&lt;h1&gt;我的第一个bootstrap标题&lt;/h1&gt;</pre>
</div>
                
              </div>
            </div>
        </div>
    </div>