---
layout: post
title: 玩转Bootstrap-标题
tags:
- 摘自慕课网
- html
- css
- Bootstrap
description: Bootstrap html css
---
<div id="js-aticle-container" class="cwrap-autoheight aticle-container" style="width: 400px;">
    <div class="code-panel" id="J_PanelCode">
        <h2 class="code-head" id="J_CodeLang" data-lang="HTML">
            标题（一）
        </h2>
        <div class="code-description" id="J_CodeDescr">
            <div class="code-desc co">
                <p>
                    <span style="font-size: 13px; line-height: 1.6em;">Bootstrap和普通的HTML页面一样，定义标题都是使用标签&lt;h1&gt;到&lt;h6&gt;,只不过Bootstrap覆盖了其默认的样式，使用其在所有浏览器下显示的效果一样，具体定义的规则可以如下表所示：</span>
                </p>
                <p>
                    <span style="font-size: 13px; line-height: 1.6em;"><a href="http://img.mukewang.com/53acce330001429807730337.jpg"><img alt="" src="http://img.mukewang.com/53acce330001429807730337.jpg" style="width: 350px;"></a></span>
                </p>
                <p>
                    通过比较可以发现，Bootstrap标题样式进行了以下显著的优化重置：
                </p>
                <p>
                    1、重新设置了<strong>margin-top</strong>和<strong>margin-bottom</strong>的值， &nbsp;<strong>h1~h3</strong>重置后的值都是<strong>20px</strong>；<strong>h4~h6</strong>重置后的值都是<strong>10px。</strong><br>
                    2、所有标题的行高都是<strong>1.1</strong>（也就是font-size的1.1倍）,而且文本颜色和字体都继承父元素的颜色和字体。<br>
                    3、固定不同级别标题字体大小，<strong>h1=36px，h2=30px，h3=24px，h4=18px，h5=14px</strong>和<strong>h6=12px。</strong>
                </p>
                <p>
                    标题的具体运用非常简单，和我们平时运用是一样的，使用&lt;h1&gt;~&lt;h6&gt;标签，分别表示标题一至标题六，h 后面的数字越大，表示级别越小，文本也越小。来看一个简单的效果：右侧代码编辑器中的10-16行的代码。
                </p>
                <p>
                    在Bootstrap中为了让非标题元素和标题使用相同的样式，还特意定义了<strong>.h1~.h6</strong>六个类名。<strong>如右侧代码编辑器中 &nbsp; 18-23行代码所示：</strong>
                </p>
                <p>
                    <span style="font-size: 13px; line-height: 1.6em;">对比两个示例的效果图，可以说他们的效果是一模一样的。</span>
                </p>
                <p>
                    &nbsp;
                </p>
            </div>
        </div>
    </div>
</div>
<div id="js-aticle-container" class="cwrap-autoheight aticle-container" style="width: 400px;">
    <div class="code-panel" id="J_PanelCode">
        <h2 class="code-head" id="J_CodeLang" data-lang="HTML">
            标题（二）
        </h2>
        <div class="code-description" id="J_CodeDescr">
            <div class="code-desc co">
                <p>
                    <span style="line-height: 1.6em;">除此之外，我们在Web的制作中，常常会碰到在一个标题后面紧跟着一行小的副标题。在Bootstrap中他也考虑了这种排版效果，使用了</span><strong style="line-height: 1.6em;">&lt;small&gt;</strong><span style="line-height: 1.6em;">标签来制作副标题。这个副标题具有其自己的一些独特样式：</span>
                </p>
                <p>
                    <strong>1、</strong>行高都是<strong>1</strong>，而且<strong>font-weight</strong>设置了<strong>normal</strong>变成了常规效果（不加粗），同时颜色被设置为<strong>灰色（#999）。</strong><br>
                    <strong>2、</strong>由于<strong>&lt;small&gt;</strong>内的文本字体在<strong>h1~h3</strong>内，其大小都设置为当前字号的<strong>65%；</strong>而在<strong>h4~h6</strong>内的字号都设置为当前字号的<strong>75%；</strong><br>
                    详细代码请参阅<strong>bootstrap.css</strong>文件中第<strong>407行~第443</strong>行。
                </p>
                <pre class="code">
						h1 small,
				        .h1 small,
				        h2 small,
				        .h2 small,
				        h3 small,
				        .h3 small,
				        h1 .small,
				        .h1 .small,
				        h2 .small,
				        .h2 .small,
				        h3 .small,
				        .h3 .small {
				          font-size: 65%;
				        }
				        h4,
				        .h4,
				        h5,
				        .h5,
				        h6,
				        .h6 {
				          margin-top: 10px;
				          margin-bottom: 10px;
				        }
				        h4 small,
				        .h4 small,
				        h5 small,
				        .h5 small,
				        h6 small,
				        .h6 small,
				        h4 .small,
				        .h4 .small,
				        h5 .small,
				        .h5 .small,
				        h6 .small,
				        .h6 .small {
				          font-size: 75%;
				        }
				        
				</pre>
                <p>
                    来简单看其使用方法与最终效果：<strong>如代码所示：</strong>
                    <h1>Bootstrap标题一<small>我是副标题</small></h1>
					<h2>Bootstrap标题二<small>我是副标题</small></h2>
					<h3>Bootstrap标题三<small>我是副标题</small></h3>
					<h4>Bootstrap标题四<small>我是副标题</small></h4>
					<h5>Bootstrap标题五<small>我是副标题</small></h5>
					<h6>Bootstrap标题六<small>我是副标题</small></h6>
                </p>
            </div>
        </div>
    </div>
</div>
