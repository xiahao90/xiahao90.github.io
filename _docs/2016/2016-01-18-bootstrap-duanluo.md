---
title: 玩转Bootstrap-段落
category: 2016
order: 91
date: 2016-01-18
---
  <div id="js-aticle-container" class="cwrap-autoheight aticle-container" style="width: 400px;">
            <div class="code-panel" id="J_PanelCode">
                <h2 class="code-head" id="J_CodeLang" data-lang="HTML">
                    段落（正文文本）
                </h2>
                <div class="code-description" id="J_CodeDescr">
                    <div class="code-desc co">
                        <p>
                            段落是排版中另一个重要元素之一。在Bootstrap中为文本设置了一个全局的文本样式（这里所说的文本是指正文文本）：
                        </p>
                        <p>
                            1、全局文本字号为<strong>14px(font-size)</strong>。
                        </p>
                        <p>
                            2、行高为<strong>1.42857143（line-height）</strong>，大约是<strong>20px</strong>(大家看到一串的小数或许会有疑惑，其实他是通过LESS编译器计算出来的，当然Sass也有这样的功能)。
                        </p>
                        <p>
                            3、颜色为<strong>深灰色（#333）</strong>；
                        </p>
                        <p>
                            4、字体为<strong>"Helvetica Neue", Helvetica, Arial, sans-serif;（font-family）</strong>，或许这样的字体对我们中文并不太合适，但在实际项目中，大家可以根据自己的需求进行重置，在此我们不做过多阐述，我们回到这里。该设置都定义在<strong>&lt;body&gt;</strong>元素上，由于这几个属性都是<strong>继承属性</strong>，所以Web页面中文本（包括段落p元素）如无重置都会具有这些样式效果。
                        </p>
                        <p>
                            /*源码请查看bootstrap.css文件中第274行~280行*/
                        </p>
                        <pre class="code">
body {
                font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
                font-size: 14px;
                line-height: 1.42857143;
                color: #333;
                background-color: #fff;
                }
</pre>
                        <p>
                            <br>
                            另外在Bootstrap中，为了让段落<strong>p</strong>元素之间具有一定的间距，便于用户阅读文本，特意设置了<strong>p</strong>元素的<strong>margin</strong>值（默认情况之下，<strong>p</strong>元素具有一个上下外边距，并且保持一个行高的高度）：<br>
                            /*源码请查看bootstrap.css文件中第467行~469行*/
                        </p>
                        <pre class="code">
p {
                 margin: 0 0 10px;
                }
</pre>
                        <p>
                            如果你对CSS预处理器有所了解，那么你完全可以根据Bootstrap提供的预编译版本LESS(或者Sass)进行自定义排版设置。在Bootstrap中，排版设置的默认值都存在<strong>variables.less</strong>文件中(Sass版本存在<strong>_variables.scss中</strong>)的两个变量：
                        </p>
                        <p>
                            <strong>LESS版本：</strong>
                        </p>
                        <pre class="code">
@font-size-base: 14px; @line-height-base: 1.428571429; // 20/14
</pre>
                        <p>
                            <strong>Sass版本：</strong>
                        </p>
                        <pre class="code">
$font-size-base: 14px !default; $line-height-base: 1.428571429 !default; // 20/14
</pre>
                        <p>
                            <span style="line-height: 1.6em;">第一条语句用于设置<strong>字体大小</strong>，第二条语句用于设置<strong>行高</strong>。系统默认使用这两个值产生整个页面相应的<strong>margin、padding</strong>和<strong>line-height</strong>的值。换句话说，你只需要修改这两个变量的值，然后重新编译，就可以自定义自己的Bootstrap排版样式。（有兴趣的同学可以尝试一下，此处对于LESS或Sass版本运用不做过多阐述）。</span>
                        </p>
                    </div>
                </div>
            </div>
        </div>