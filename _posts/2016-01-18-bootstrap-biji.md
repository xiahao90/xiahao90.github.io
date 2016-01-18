---
layout: post
title: Bootstrap-个人基础笔记
tags:
- html
- css
- Bootstrap
description: Bootstrap html css
---
<code>
    <p>b 粗体</p>
    <p>i 斜体</p>
    <p>.text-muted：提示，使用浅灰色（#999）</p>
    <p>.text-primary：主要，使用蓝色（#428bca）</p>
    <p>.text-success：成功，使用浅绿色(#3c763d)</p>
    <p>.text-info：通知信息，使用浅蓝色（#31708f）</p>
    <p>.text-warning：警告，使用黄色（#8a6d3b）</p>
    <p>.text-danger：危险，使用褐色（#a94442）</p>
    <p>.text-left：左对齐</p>
    <p>.text-center：居中对齐</p>
    <p>.text-right：右对齐</p>
    <p>.text-justify：两端对齐</p>
    --------------------------------------------------------------------------
    <p>ul li 有序列表</p>
    <p>ol li 无需列表</p>
    <p>dl dt dd 定义列表</p>
    <p>.list-unstyled 去除列表的风格（前面的符号与数字）</p>
    <p>.list-inline 把垂直列表换成水平列表，而且去掉项目符号（编号），保持水平显示。也可以说内联列表就是为制作水平导航而生。</p>
    <dl class="dl-horizontal">给定义列表实现水平显示效果,用户双排表单提交用。
    标签code：来显示单行内联代码。粉红色底纹
    <pre></pre>来显示多行块代码，灰色底纹一块
    <pre class="pre-scrollable"> 可以控制代码块区域最大高度为340px，一旦超出这个高度，就会在Y轴出现滚动条
    <kbd></kbd>来显示用户输入代码，黑色背景白色文字
    --------------------------------------------------------------------------
    <table>
       <thead>
         <tr>
           <th>表格标题</th>
         </tr>
       </thead>
       <tbody>
         <tr>
           <td>表格单元格</td>
         </tr>
       </tbody>
     </table>

    table的class:

    .table：基础表格
    .table-striped：斑马线表格 一排一个颜色
    .table-bordered：带边框的表格  
    .table-hover：鼠标悬停高亮的表格 移入变色
    .table-condensed：紧凑型表格 就是单元格没内距或者内距较其他表格的内距更小。
    .table-responsive：响应式表格 当你的浏览器可视区域小于768px时，表格底部会出现水平滚动条。

    tr的class  背景颜色
    .active：提示，使用浅灰色（#999）
    .success：成功，使用浅绿色(#3c763d)
    .info：通知信息，使用浅蓝色（#31708f）
    .warning：警告，使用黄色（#8a6d3b）
    .danger：危险，使用褐色（#a94442）
    --------------------------------------------------------------------------
    表单：
    from
    .form-horizontal 实现水平效果（标签居左，表单控件居右）
    .form-inline 将表单的控件都在一行内显示

    input select textaread的输入框样式
    .form-control  
    .input-sm:让控件比正常大小更小
    .input-lg:让控件比正常大小更大
    .col-xs-6：设置输入框的宽度 后面的数字可以改变

    <select multiple class="form-control">  多选下拉框

    .checkbox与radio   字父级别div 上面添加该名字的class则会对齐
    <div class="checkbox">
        <label>
          <input type="checkbox" value="">
          记住密码
        </label>
      </div>
      <div class="radio">
        <label>
          <input type="radio" name="optionsRadios" id="optionsRadios1" value="love" checked>
          喜欢
        </label>
      </div>
        <div class="radio">
        <label>
          <input type="radio" name="optionsRadios" id="optionsRadios2" value="hate">
          不喜欢
        </label>
      </div>



    .checkbox-inline checkbox需要水平排列，只需要在label标签上添加类
    <form role="form">
      <div class="form-group">
        <label class="checkbox-inline">
          <input type="checkbox"  value="option1">游戏
        </label>
        <label class="checkbox-inline">
          <input type="checkbox"  value="option2">摄影
        </label>
        <label class="checkbox-inline">
        <input type="checkbox"  value="option3">旅游
        </label>
      </div>
      <div class="form-group">
        <label class="radio-inline">
          <input type="radio"  value="option1" name="sex">男性
        </label>
        <label class="radio-inline">
          <input type="radio"  value="option2" name="sex">女性
        </label>
        <label class="radio-inline">
          <input type="radio"  value="option3" name="sex">中性
        </label>
      </div>
    </form>

    button按钮的样式
    .btn--primary：主要，背景蓝色（#428bca）
    .btn--success：成功，背景浅绿色(#3c763d)
    .btn--info：通知信息，背景浅蓝色（#31708f）
    .btn--warning：警告，背景黄色（#8a6d3b）
    .btn--danger：危险，背景褐色（#a94442）


    .disabled 表单禁用

    在制作表单时，不免要做表单验证。同样也需要提供验证状态样式，在Bootstrap框架中同样提供这几种效果。
    1、.has-warning:警告状态（黄色）
    2、.has-error：错误状态（红色）
    3、.has-success：成功状态（绿色）
    使用的时候只需要在form-group容器上对应添加状态类名。
    <form role="form">
      <div class="form-group has-success has-feedback">
        <label class="control-label" for="inputSuccess1">成功状态</label>
        <input type="text" class="form-control" id="inputSuccess1" placeholder="成功状态" >
        <span class="help-block">你输入的信息是正确的</span>
        <span class="glyphicon glyphicon-ok form-control-feedback"></span>
      </div>
      <div class="form-group has-warning has-feedback">
        <label class="control-label" for="inputWarning1">警告状态</label>
        <input type="text" class="form-control" id="inputWarning1" placeholder="警告状态">
        <span class="help-block">请输入正确信息</span>
        <span class="glyphicon glyphicon-warning-sign form-control-feedback"></span>
      </div>
      <div class="form-group has-error has-feedback">
        <label class="control-label" for="inputError1">错误状态</label>
        <input type="text" class="form-control" id="inputError1" placeholder="错误状态">
        <span class="help-block">你输入的信息是错误的</span>
        <span class="glyphicon glyphicon-remove form-control-feedback"></span>  
      </div>
    </form>
    -------------------------------------------------------------------------------------------------
    按钮
       <button class="btn" type="button">基础按钮.btn</button>  
       <button class="btn btn-default" type="button">默认按钮.btn-default</button> 
       <button class="btn btn-primary" type="button">主要按钮.btn-primary</button> 
       <button class="btn btn-success" type="button">成功按钮.btn-success</button> 
       <button class="btn btn-info" type="button">信息按钮.btn-info</button> 
       <button class="btn btn-warning" type="button">警告按钮.btn-warning</button> 
       <button class="btn btn-danger" type="button">危险按钮.btn-danger</button> 
       <button class="btn btn-link" type="button">链接按钮.btn-link</button> 

       <button class="btn btn-primary btn-lg" type="button">大型按钮.btn-lg</button>
       <button class="btn btn-primary" type="button">正常按钮</button>
       <button class="btn btn-primary btn-sm" type="button">小型按钮.btn-sm</button>
       <button class="btn btn-primary btn-xs" type="button">超小型按钮.btn-xs</button>
    可用于a标签，span 标签
    . btn-block 按钮宽度充满整个父容器（width:100%）
    .disabled 或者 disabled=“disabled” ，将按钮禁用，后者禁用按钮的行为
    --------------------------------------------------------------------------------------------------
    图像
    <div class="row">
        <div class="col-sm-4">
          <img   alt="140x140" src="http://placehold.it/140x140">
            <div>默认图片</div>
        </div>
        <div class="col-sm-4">
          <img  class="img-rounded" alt="140x140" src="http://placehold.it/140x140"> 
            <div>圆角图片</div>
        </div>
        <div class="col-sm-4">
          <img  class="img-circle" alt="140x140" src="http://placehold.it/140x140">
            <div>圆形图片</div>
        </div>
          <div class="row">
            <div class="col-sm-6">
              <img  class="img-thumbnail" alt="140x140" src="http://placehold.it/140x140"> 
                <div>缩略图</div>
            </div>
            <div class="col-sm-6">
              <img  class="img-responsive" alt="140x140" src="http://placehold.it/140x140" /> 
              <div>响应式图片</div>
            </div>
          </div>
      </div>
    图标
        <span class="glyphicon glyphicon-ok"></span>
        <span class="glyphicon glyphicon-remove">
    -------------------------------------------------------------------------------------------------------
    页面布局排版

    需要在.container下的.row下
    .col-xs-数字   auto
    .col-sm-数字   750px
    .col-md-数字   970px
    .col-lg-数字   1170px

    .col-md-offset-数字 向右偏移多少个，总数不能超过12
    .col-md-push-8向后推8个 用于交换位置
    .col-md-pull-4向前拉8个 用于交换位置
    ——————————————————————————————————————————————————————————————————
    下拉菜单

     <div class="dropdown">
      <button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu1" data-toggle="dropdown">
        下拉菜单
        <span class="caret"></span>
      </button>
      <ul class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu1">
        <li role="presentation" class="dropdown-header">第一部分菜单头部</li>
        <li role="presentation" class="active"><a role="menuitem" tabindex="-1" href="#">当前选项</a></li>
        <li role="presentation" class="disabled"><a role="menuitem" tabindex="-1" href="#">禁用选项</a></li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">下拉菜单项</a></li>
        <li role="presentation" class="divider">分割线</li>
        <li role="presentation"><a role="menuitem" tabindex="-1" href="#">下拉菜单项</a></li>
      </ul>
    </div> 
    .dropdown-menu加上.dropdown-menu-right  可实现右对齐
    .active 当前的选项
    .disabled 禁用选项

    顶部导航案例
    <div class="btn-group">
      <button class="btn btn-default" type="button">首页</button>
      <div class="btn-group">
          <button class="btn btn-default dropdown-toggle" data-toggle="dropdown" type="button">产品展示
              <span class="caret"></span>
          </button>
          <ul class="dropdown-menu">
            <li><a href="##">收割机</a></li>
            <li><a href="##">电动机</a></li>
            <li><a href="##">拖拉机</a></li>
            <li><a href="##">电动车</a></li>
        </ul>
      </div>
      <button class="btn btn-default" type="button">案例分析</button>
      <button class="btn btn-default" type="button">联系我们</button>
      <div class="btn-group">
          <button class="btn btn-default dropdown-toggle" data-toggle="dropdown" type="button">关于我们
          <span class="caret"></span>
          </button>
        <ul class="dropdown-menu">
            <li><a href="##">公司简介</a></li>
            <li><a href="##">企业文化</a></li>
            <li><a href="##">组织结构</a></li>
            <li><a href="##">客服服务</a></li>
        </ul>
      </div>
    </div>

    垂直导航：将<div class="btn-group">换成<div class="btn-group-vertical">
    等分按钮：要在按钮组“btn-group”上追加一个“btn-group-justified”类
    菜单向上弹出：要在按钮组“btn-group”上追加一个“dropup”类


    导航(基础样式)
     <ul class="nav nav-tabs">
        <li class=""><a href="##">Home</a></li>
        <li class="active"><a href="##">CSS3</a></li>
        <li><a href="##">Sass</a></li>
        <li class="active"><a href="##">jQuery</a></li>
        <li class="disabled"><a href="##">Responsive</a></li>
     </ul>


    胶囊形(pills)导航
    <ul class="nav nav-pills">
        <li class="active"><a href="##">Home</a></li>
        <li><a href="##">CSS3</a></li>
        <li><a href="##">Sass</a></li>
        <li><a href="##">jQuery</a></li>
            <li class="divider"><a href="##">分割线</a></li>
            <li class="disabled"><a href="##">Responsive</a></li>
    </ul>

    胶囊形(pills)导航竖排版：<ul class="nav nav-pills nav-stacked">
    .nav-justified  导航自适应宽度排版



    面包屑式导航
    <ol class="breadcrumb">
      <li><a href="#">首页</a></li>
      <li><a href="#">我的书</a></li>
      <li class="active">《图解CSS3》</li>
    </ol> 
    --------------------------------------------------------------------------------------------
    相应式导航条
    <div class="navbar navbar-default" role="navigation">
      <div class="navbar-header">
         　<!-- .navbar-toggle样式用于toggle收缩的内容，即nav-collapse collapse样式所在元素 -->
           <button class="navbar-toggle" type="button" data-toggle="collapse" data-target=".navbar-responsive-collapse">
             <span class="sr-only">Toggle Navigation</span>
             <span class="icon-bar"></span>
             <span class="icon-bar"></span>
             <span class="icon-bar"></span>
           </button>
           <!-- 确保无论是宽屏还是窄屏，navbar-brand都显示 -->
           <a href="##" class="navbar-brand">慕课网</a>
      </div>
      <!-- 屏幕宽度小于768px时，div.navbar-responsive-collapse容器里的内容都会隐藏，显示icon-bar图标，当点击icon-bar图标时，再展开。屏幕大于768px时，默认显示。 -->
      <div class="collapse navbar-collapse navbar-responsive-collapse">
            <ul class="nav navbar-nav">
                <li class="active"><a href="##">网站首页</a></li>
                <li><a href="##">系列教程</a></li>
                <li><a href="##">名师介绍</a></li>
                <li><a href="##">成功案例</a></li>
                <li><a href="##">关于我们</a></li>
            </ul>
      </div>
    </div>
    .navbar-inverse 反色效果


    分页导航：
    <ul class="pagination pagination-s">
      <li><a href="#">&laquo;第一页</a></li>
      <li><a href="#">11</a></li>
      <li><a href="#">12</a></li>
      <li class="active"><a href="#">13</a></li>
      <li><a href="#">14</a></li>
      <li><a href="#">15</a></li>
      <li class="disabled"><a href="#">最后一页&raquo;</a></li>
    </ul>   

    <ul class="pager">
      <li class="disabled"><span>&laquo;上一页</span></li>
      <li><a href="#">下一页&raquo;</a></li>
    </ul>  

    .<span class="badge">23</span>  徽章，用于信息数量提示
    ---------------------------------------------------------------------------------


    复杂的缩略图
    <h1>复杂的缩略图</h1>
    <div class="container">
        <div class="row">
            <div class="col-xs-6 col-md-3">
                <a href="#" class="thumbnail">
                    <img src="http://img.mukewang.com/5434eba100014fe906000338.png" style="height: 180px; width: 100%; display: block;" alt="">
                </a>
                <div class="caption">
                    <h3>Bootstrap框架系列教程</h3>
                    <p>Bootstrap框架是一个优秀的前端框，就算您是一位后端程序员或者你是一位不懂设计的前端人员，你也能依赖于Bootstrap制作做优美的网站...</p>
                    <p>
                        <a href="##" class="btn btn-primary">开始学习</a>
                        <a href="##" class="btn btn-info">正在学习</a>
                    </p>
                </div>  
            </div>
            <div class="col-xs-6 col-md-3">
                <a href="#" class="thumbnail">
                    <img alt="100%x180" src="http://img.mukewang.com/5434eba100014fe906000338.png" style="height: 180px; width: 100%; display: block;">
                </a>
                <div class="caption">
                    <h3>Bootstrap框架系列教程</h3>
                    <p> Bootstrap框架是一个优秀的前端框，就算您是一位后端程序员或者你是一位不懂设计的前端人员，你也能依赖于Bootstrap制作做优美的网站...</p>
                    <p>
                        <a href="##" class="btn btn-primary">开始学习</a>
                        <a href="##" class="btn btn-info">正在学习</a>
                    </p>
                </div>   
            </div>
            <div class="col-xs-6 col-md-3">
                <a href="#" class="thumbnail">
                    <img alt="100%x180" src="http://img.mukewang.com/5434eba100014fe906000338.png" style="height: 180px; width: 100%; display: block;">
                </a>
                <div class="caption">
                    <h3>Bootstrap框架系列教程</h3>
                        <p>Bootstrap框架是一个优秀的前端框，就算您是一位后端程序员或者你是一位不懂设计的前端人员，你也能依赖于Bootstrap制作做优美的网站...</p>
                        <p>
                            <a href="##" class="btn btn-primary">开始学习</a>
                            <a href="##" class="btn btn-info">正在学习</a>
                        </p>
                </div>   
            </div>
            <div class="col-xs-6 col-md-3">
                <a href="#" class="thumbnail">
                    <img alt="100%x180" src="http://img.mukewang.com/5434eba100014fe906000338.png" style="height: 180px; width: 100%; display: block;">
                </a>
                <div class="caption">
                    <h3>Bootstrap框架系列教程</h3>
                    <p>Bootstrap框架是一个优秀的前端框，就算您是一位后端程序员或者你是一位不懂设计的前端人员，你也能依赖于Bootstrap制作做优美的网站...</p>
                    <p>
                        <a href="##" class="btn btn-primary">开始学习</a>
                        <a href="##" class="btn btn-info">正在学习</a>
                    </p>
                </div>   
            </div>
        </div>
    </div>
    -------------------------------------------------------------------------------------------------

    进度条
    <h2>基本进度条</h2>
    <div class="progress">
         <div class="progress-bar" style="width:40%">
        </div>
    </div> 
    <h2>彩色进度条</h2>
    <div class="progress">
        <div class="progress-bar progress-bar-success" style="width:40%"></div>
    </div> 
    <div class="progress">
         <div class="progress-bar progress-bar-info" style="width:60%"></div>
    </div> 
    <div class="progress">
        <div class="progress-bar progress-bar-warning" style="width:80%"></div>
    </div> 
    <div class="progress">
        <div class="progress-bar progress-bar-danger" style="width:50%"></div>
    </div> 
    <h2>条纹进度条</h2>
    <div class="progress progress-striped">
        <div class="progress-bar progress-bar-success" style="width:40%"></div>
    </div>
    <div class="progress progress-striped">
        <div class="progress-bar progress-bar-info" style="width:60%"></div>
    </div>
    <div class="progress progress-striped">
        <div class="progress-bar progress-bar-warning" style="width:80%"></div>
    </div>
    <div class="progress progress-striped">
        <div class="progress-bar progress-bar-danger" style="width:50%"></div>
    </div>
    <h2>动态条纹进度条</h2>
    <div class="progress progress-striped active">
        <div class="progress-bar progress-bar-success" style="width:40%"></div>
    </div> 
    <div class="progress progress-striped active">
        <div class="progress-bar progress-bar-info" style="width:60%"></div>
    </div> 
    <div class="progress progress-striped active">
        <div class="progress-bar progress-bar-warning" style="width:80%"></div>
    </div> 
    <div class="progress progress-striped active">
        <div class="progress-bar progress-bar-danger" style="width:50%"></div>
    </div>
    <h2>层叠进度条</h2>
    <h5>正常层叠进度条</h5>
    <div class="progress">
        <div class="progress-bar progress-bar-success" style="width:20%"></div>
        <div class="progress-bar progress-bar-info" style="width:10%"></div>
        <div class="progress-bar progress-bar-warning" style="width:30%"></div>
        <div class="progress-bar progress-bar-danger" style="width:15%"></div>
    </div> 
    <h5>不良效果层叠进度条</h5> 
    <div class="progress">
        <div class="progress-bar progress-bar-success" style="width:20%"></div>
        <div class="progress-bar progress-bar-info" style="width:40%"></div>
        <div class="progress-bar progress-bar-warning" style="width:30%"></div>
        <div class="progress-bar progress-bar-danger" style="width:45%"></div>
    </div> 
    <h5>层叠条纹进度条</h5>
    <div class="progress">
        <div class="progress-bar progress-bar-success" style="width:20%"></div>
        <div class="progress-bar progress-bar-info" style="width:20%"></div>
        <div class="progress-bar progress-bar-warning" style="width:30%"></div>
        <div class="progress-bar progress-bar-danger" style="width:15%"></div>
    </div>  
    <div class="progress">
        <div class="progress-bar progress-bar-success progress-bar-striped" style="width:20%">1</div>
        <div class="progress-bar progress-bar-info progress-bar-striped" style="width:20%">2</div>
        <div class="progress-bar progress-bar-striped progress-bar-warning" style="width:30%">3</div>
        <div class="progress-bar progress-bar-danger progress-bar-striped" style="width:15%">4</div>
    </div> 
    <div class="progress">
        <div class="progress-bar progress-bar-success" style="width:20%"></div>
        <div class="progress-bar progress-bar-info progress-bar-striped" style="width:20%"></div>
        <div class="progress-bar progress-bar-warning" style="width:30%"></div>
        <div class="progress-bar progress-bar-danger progress-bar-striped" style="width:15%"></div>
    </div>
    <h2>带Label的进度条</h2>
    <h5>进度条1</h5>
    <div class="progress">
        <div class="progress-bar progress-bar-success"  role="progressbar" aria-valuenow="20" aria-valuemin="0" aria-valuemax="100" style="width:20%">20%</div>  
    </div>  
    <div class="progress">
        <div class="progress-bar progress-bar-info" role="progressbar" aria-valuenow="70" aria-valuemin="0" aria-valuemax="100"   style="width:70%">70%</div>
    </div>
    <div class="progress">
        <div class="progress-bar progress-bar-warning"  role="progressbar" aria-valuenow="30" aria-valuemin="0" aria-valuemax="100" style="width:30%">30%</div>
    </div>
    <div class="progress">
        <div class="progress-bar progress-bar-danger" role="progressbar" aria-valuenow="15" aria-valuemin="0" aria-valuemax="100" style="width:15%">15%</div>
    </div>
    -------------------------------------------------------------------------------------------------------
    媒体对象--媒体对象的嵌套
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    </head>
    <body>
    <div class="media">
        <a class="pull-left" href="#">
            <img class="media-object" src="http://a.disquscdn.com/uploads/users/3740/2069/avatar92.jpg?1406972031" alt="...">
        </a>
        <div class="media-body">
            <h4 class="media-heading">我是大漠</h4>
            <div>我是W3cplus站长大漠，我在写Bootstrap框中的媒体对象测试用例</div>
            <div class="media">
                <a class="pull-left" href="#">
                    <img class="media-object" src="http://tp2.sinaimg.cn/3306361973/50/22875318196/0" alt="...">
                </a>
                <div class="media-body">
                    <h4 class="media-heading">慕课网</h4>
                    <div>大漠写的《玩转Bootstrap》系列教程即将会在慕课网上发布</div>
                    <div class="media">
                        <a class="pull-left" href="#">
                            <img class="media-object" src="http://tp4.sinaimg.cn/1167075935/50/22838101204/1" alt="...">
                        </a>
                        <div class="media-body">
                            <h4 class="media-heading">W3cplus</h4>
                            <div>W3cplus站上还有很多教程....</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    -----------------------------------------------------------------------------------------------------------
    基础列表组
    <ul class="list-group">
        <li class="list-group-item">揭开CSS3的面纱</li>
        <li class="list-group-item">CSS3选择器</li>
        <li class="list-group-item">CSS3边框</li>
        <li class="list-group-item">CSS3背景</li>
        <li class="list-group-item">CSS3文本</li>
    </ul>
    <h3>带徽章的列表组</h3>
    <ul class="list-group">
        <li class="list-group-item">
            <span class="badge">13</span>揭开CSS3的面
        </li>
        <li class="list-group-item">
            <span class="badge">456</span>CSS3选择器
        </li>
        <li class="list-group-item">
            <span class="badge">892</span>CSS3边框
        </li>
        <li class="list-group-item">
            <span class="badge">90</span>CSS3背景
        </li>
        <li class="list-group-item">
            <span class="badge">1290</span>CSS3文本
        </li>
    </ul>
    <h3>带链接的列表组</h3>
    <ul class="list-group">
        <li class="list-group-item">
            <a href="##">揭开CSS3的面</a>
        </li>
        <li class="list-group-item">
            <a href="##">CSS3选择器</a>
        </li>
        <li class="list-group-item">
            <a href="##">CSS3边框</a>
        </li>
        <li class="list-group-item">
            <a href="##">CSS3背景</a>
        </li>
        <li class="list-group-item">
            <a href="##">CSS3文本</a>
        </li>
    </ul>
    <h3>自定义列表组</h3>
    <div class="list-group">
        <a href="##" class="list-group-item">
            <h4 class="list-group-item-heading">图解CSS3</h4>
            <p class="list-group-item-text">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性...</p>
        </a>
        <a href="##" class="list-group-item">
            <h4 class="list-group-item-heading">Sass中国</h4>
            <p class="list-group-item-text">致力于为中国开发者提供最全面，最具影响力，最前沿的Sass相关技术与教程...</p>
        </a>
    </div>
    <h3>组合列表项的状态</h3>
    <div class="list-group">
        <a href="##" class="list-group-item active"><span class="badge">5902</span>图解CSS3</a>
        <a href="##" class="list-group-item"><span class="badge">15902</span>W3cplus</a>
        <a href="##" class="list-group-item"><span class="badge">59020</span>慕课网</a>
        <a href="##" class="list-group-item disabled"><span class="badge">0</span>Sass中国</a>
    </div>
    <h3>多彩列表组</h3>
    <div class="list-group">
        <a href="##" class="list-group-item active"><span class="badge">5902</span>图解CSS3</a>
        <a href="##" class="list-group-item list-group-item-success"><span class="badge">15902</span>W3cplus</a>
        <a href="##" class="list-group-item list-group-item-info"><span class="badge">59020</span>慕课网</a>
        <a href="##" class="list-group-item list-group-item-warning"><span class="badge">0</span>Sass中国</a>
        <a href="##" class="list-group-item list-group-item-danger"><span class="badge">10</span>Mobile教程</a>
    </div>
    ------------------------------------------------------------------------------------------------------
    面板
    <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    </head>
    <body>
    <h3>基础面板</h3>
    <div class="panel panel-default">
        <div class="panel-body">我是一个基础面板，带有默认主题样式风格</div>
    </div>
    <h3>带有头和尾的面板</h3>
    <div class="panel panel-default">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性
        </div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <h3>彩色面板</h3>
    <div class="panel panel-default">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">            详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性
        </div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <div class="panel panel-primary">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性</div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <div class="panel panel-success">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性</div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <div class="panel panel-info">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性</div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <div class="panel panel-warning">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性</div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <div class="panel panel-danger">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性</div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    <h3>面板中嵌套表格</h3>
    <div class="panel panel-default">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">
            <p>详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性
            </p>
            <table class="table table-bordered">
                <thead>
                    <tr>
                        <th>＃</th>
                        <th>我的书</th>
                        <th>发布时间</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>1</td>
                        <td>《图解CSS3》</td>
                        <td>2014-07-10</td>
                    </tr>
                </tbody>
            </table>
        </div>
        <div class="panel-footer">作者：大漠</div>
    </div>
    -------------------------------------------
    面板中嵌套列表组（一）
    <div class="panel panel-default">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">
            <p>详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性
            </p>
            <ul class="list-group">
                <li class="list-group-item">我是列表项</li>
                <li class="list-group-item">我是列表项</li>
                <li class="list-group-item">我是列表项</li>
            </ul>
        </div>
        <div class="panel-footer">作者：大漠</div>
    </div>

    <h3>面板中嵌套列表组（二）</h3>
    <div class="panel panel-default">
        <div class="panel-heading">图解CSS3</div>
        <div class="panel-body">
            <p>详细讲解了选择器、边框、背景、文本、颜色、盒模型、伸缩布局盒模型、多列布局、渐变、过渡、动画、媒体、响应Web设计、Web字体等主题下涵盖的所有CSS3新特性
            </p>
        </div>
        <ul class="list-group">
            <li class="list-group-item">我是列表项</li>
            <li class="list-group-item">我是列表项</li>
            <li class="list-group-item">我是列表项</li>
        </ul>
        <div class="panel-footer">作者：大漠</div>
    </div>

    -----------------------------------------------------------------------------------------------------------------------------------
    js
    弹窗效果：
    <!-- data-target触发模态弹出窗元素 -->
    <button class="btn btn-primary" type="button" data-toggle="modal" data-target="#mymodal1">通过data-target触发</button>
    <!-- 模态弹出窗内容 -->
    <div class="modal fade" id="mymodal1" id="mymodal-data" tabindex="-1" role="dialog" aria-labelledby="mySmallModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal"><span aria-hidden="true">&times;</span><span class="sr-only">Close</span></button>
                    <h4 class="modal-title">模态弹出窗标题</h4>
                </div>
                <div class="modal-body">
                    <p>模态弹出窗主体内容</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">关闭</button>
                    <button type="button" class="btn btn-primary">保存</button>
                </div>
            </div>
        </div>
    </div>
</code>

