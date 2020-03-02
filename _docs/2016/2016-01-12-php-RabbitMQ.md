---
title: php消息队列（RabbitMQ使用）
category: 2016
order: 98
date: 2016-01-12
---
<div>&nbsp;网上有很多安装教程：<br/>
<a href="http://blog.csdn.net/i_bruce/article/details/40300125" target="_blank" title="http://blog.csdn.net/i_bruce/article/details/40300125">http://blog.csdn.net/i_bruce/article/details/40300125</a><br/>
<br/>
<a href="http://www.vicenteforever.com/2012/04/rabbitmq/" target="_blank" title="http://www.vicenteforever.com/2012/04/rabbitmq/">http://www.vicenteforever.com/2012/04/rabbitmq/</a><br/>
<br/>
就不在这里详细解释了，经过我不深入的了解，要安装一个rabbitmq &nbsp;和一个<span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">AMPQ 就可以使用了。<br/>
<span><br/>
下面上我自己搞的代码&nbsp;<br/>
<span><span style="font-size:24px">生产者：&nbsp;</span><br/>
</span></span></span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><br/>

<div>
<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&lt;?php</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">if ( ! defined ( 'API_ROOT_PATH' ) )&nbsp;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">{</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>define ( 'API_ROOT_PATH', dirname( __FILE__));</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">class Producer{</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>protected $_host = 'localhost';//服务器地址</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>protected $_port = 5672;//端口</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>protected $_login = 'guest';//帐号</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>protected $_password = 'guest';//密码</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>protected $_vhost = '/';//权限</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>public function __construct ($host,$port,$login,$password,$vhost){</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$this-&gt;_host=$host;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$this-&gt;_port=$port;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$this-&gt;_login=$login;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$this-&gt;_password=$password;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$this-&gt;_vhost=$vhost;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>/**</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>*生产消息</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>*e_name 交换机</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>*k_route 路由key</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>*data 数据</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>*/</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>public function add($data,$e_name='e_linvo',$k_route='key_1'){</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$conn_args=array(</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>'host'<span style="white-space:pre"></span>=&gt;$this-&gt;_host,</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>'port'<span style="white-space:pre"></span>=&gt;$this-&gt;_port,</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>'login'<span style="white-space:pre"></span>=&gt;$this-&gt;_login,</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>'password'<span style="white-space:pre"></span>=&gt;$this-&gt;_password,</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>'vhost'<span style="white-space:pre"></span>=&gt;$this-&gt;_vhost,</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//------------------------------------------------------------------------------------------------</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//connection 与 channel ： &nbsp;连接与信道</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// connection是指物理的连接，一个client与一个server之间有一个连接；一个连接上可以建立多个channel，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 可以理解为逻辑上的连接。一般应用的情况下，有一个channel就够用了，不需要创建更多的channel。示例代码：</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$conn = new AMQPConnection($conn_args);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>if (!$conn-&gt;connect()) {</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span> &nbsp; &nbsp;die("Cannot connect to the broker!\n");</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$channel = new AMQPChannel($conn);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//------------------------------------------------------------------------------------------------</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// exchange 与 &nbsp;routingkey ： 交换机与路由键</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 为了将不同类型的消息进行区分，设置了交换机与路由两个概念。比如，将A类型的消息发送到名为‘C1’的交换机，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 将类型为B的发送到'C2'的交换机。当客户端连接C1处理队列消息时，取到的就只是A类型消息。进一步的，如果A类型消息也非常多，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 需要进一步细化区分，比如某个客户端只处理A类型消息中针对K用户的消息，routingkey就是来做这个用途的。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$ex = new AMQPExchange($channel);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$ex-&gt;setName($e_name);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$ex-&gt;setType(AMQP_EX_TYPE_DIRECT); //direct类型</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$ex-&gt;setFlags(AMQP_DURABLE); //持久化</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 由以上代码可以看到，发送消息时，只要有“交换机”就够了。至于交换机后面有没有对应的处理队列，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//发送方是不用管的。routingkey可以是空的字符串。在示例中，我使用了两个key交替发送消息，是为了下面更便于理解routingkey的作用。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 对于交换机，有两个重要的概念：</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// A，类型。有三种类型： Fanout类型最简单，这种模型忽略routingkey；Direct类型是使用最多的，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//使用确定的routingkey。这种模型下，接收消息时绑定'key_1'则只接收key_1的消息；最后一种是Topic，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//这种模式与Direct类似，但是支持通配符进行匹配，比如： 'key_*'，就会接受key_1和key_2。Topic貌似美好，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//但是有可能导致不严谨，所以还是推荐使用Direct。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// B，持久化。指定了持久化的交换机，在重新启动时才能重建，否则需要客户端重新声明生成才行。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>// 需要特别明确的概念：交换机的持久化，并不等于消息的持久化。只有在持久化队列中的消息，才能持久化；</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//如果没有队列，消息是没有地方存储的；消息本身在投递时也有一个持久化标志的，PHP中默认投递到持久化交换机就是持久的消息，不用特别指定。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//自我总结：</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//1，一条消息就相当于在mysql存了一个字段，增加了exchange与routingkey相当于又增加2个字段，方便条件查询</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//2，交换机类型，Fanout类似于mysql中只有一个字段，Direct类似于有2个字段可以用条件查询，Topic相当于有2个字段不光可以用条件查询还能用通配符查询</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//------------------------------------------------------------------------------------------------</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>//开始生产消息</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>if(is_array($data)){</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>foreach ($data as $key =&gt; $value) {</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; <span style="white-space:pre"></span>$i=$ex-&gt;publish($value,$k_route);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>}else{</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$i=$ex-&gt;publish($data,$k_route);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$conn-&gt;disconnect();</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>return $i;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$p=new Producer('localhost',5672,'guest','guest','/');</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//对一般PHP应用，不需要用户权限设定，直接使用默认就存在的"/"就可以了，用户可以使用默认就存在的"guest"。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$data=array();</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">for ($i=0; $i &lt;4 ; $i++) {&nbsp;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span style="white-space:pre"></span>$data[]='ok-'.$i;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">echo '&lt;pre&gt;';</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$p-&gt;add($data,'q1','q1');</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">?&gt;</span></div>
</div>
<span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span><span>&nbsp;<br/>
<span><br/>
</span></span></span></span><strong style="margin:0px;padding:0px;border:0px;font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif;color:rgb(51, 51, 51)"><span style="font-size:24px">接收者：<br/>
<span style="font-size:14px">&nbsp;</span></span></strong><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------</span><br/>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&lt;?php</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">require_once ('dbc.php');//一个数据库操作类</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//将接受到的数据存入mysql &nbsp;方便用浏览器条数 &nbsp;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$conn_args = array(</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; 'host' =&gt; 'localhost',</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; 'port' =&gt; '5672',</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; 'login' =&gt; 'guest',</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; 'password' =&gt; 'guest',</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; 'vhost'=&gt;'/'</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$e_name = 't1'; //交换机名</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$q_name = 'q_linvo'; //队列名</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$k_route = 't1'; //路由key</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//队列</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">// 队列仅是针对接收方（consumer）的，由接收方根据需求创建的。只有队列创建了，交换机才会将新接受到的消息送到队列中，</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">// 交换机是不会在队列创建之前的消息放进来的。换句话说，在建立队列之前，发出的所有消息都被丢弃了。</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//创建连接和channel</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$conn = new AMQPConnection($conn_args);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">// print_r($conn);exit;</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">if (!$conn-&gt;connect()) {</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; die("Cannot connect to the broker!\n");</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$channel = new AMQPChannel($conn);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//创建交换机</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$ex = new AMQPExchange($channel);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$ex-&gt;setName($e_name);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$ex-&gt;setType(AMQP_EX_TYPE_DIRECT); //direct类型</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$ex-&gt;setFlags(AMQP_DURABLE); //持久化</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">echo "Exchange Status:".$ex-&gt;declare()."\n";</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//创建队列</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$q = new AMQPQueue($channel);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$q-&gt;setName($q_name);</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$q-&gt;setFlags(AMQP_DURABLE); //持久化</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">echo "Message Total:".$q-&gt;declare()."\n";</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//绑定交换机与队列，并指定路由键</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">echo 'Queue Bind: '.$q-&gt;bind($e_name, $k_route)."\n";</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><br/>
</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">//阻塞模式接收消息</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">echo "Message:\n";</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">while(True){</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; $q-&gt;consume('processMessage');</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">$conn-&gt;disconnect();</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">/**</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">* 消费回调函数</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">* 处理消息</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">*/</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">function processMessage($envelope, $queue) {</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; $msg = $envelope-&gt;getBody();</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; $db = new dbc('user');</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; // echo $msg."\n"; //处理消息</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; $db-&gt;insert(array(</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; &nbsp; &nbsp; 'name'=&gt;'y',</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; &nbsp; &nbsp; 'code'=&gt;rand(100,999),</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; &nbsp; &nbsp; 'addtime'=&gt;time(),</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; &nbsp; &nbsp; ));</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp; &nbsp; $queue-&gt;ack($envelope-&gt;getDeliveryTag()); //手动发送ACK应答</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">}</span></div>

<div><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">--------------</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">&nbsp;<br/>
<span style="font-size:36px">所遇到问题与注意事项：</span></span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span><br/>
<span style="font-size:24px">1，</span></span></span><span style="color:rgb(51, 51, 51);font-family:'Microsoft Yahei', 微软雅黑, arial, 宋体, sans-serif;text-align:justify">RabbitMq是服务有自己命令，进入安装目录后就可以查看命令头了，我的默认的安装</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span><br/>
目录是<br/>
<img alt="图片" src="http://r.photo.store.qq.com/psb?/V117wgmo1XH269/Dn75QjXzuNmsr1.mwEAyTBe7iKTOyClv6KhtoKt0Z8g!/o/dAkAAAAAAAAA&amp;bo=.AF9APgBfQADACU!" style="width:504px;height:125px"><br/>
<br/>
<span style="font-size:24px">2，</span></span></span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif">guest账户只能在本地服务器用，外部服务器是不能用用的&nbsp;</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span><br/>
</span></span><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">相应的命令</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">(1) 新增一个用户</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">rabbitmqctl&nbsp; add_user&nbsp; Username&nbsp; Password</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">(2) 删除一个用户</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">rabbitmqctl&nbsp; delete_user&nbsp; Username</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">(3) 修改用户的密码</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">rabbitmqctl&nbsp; change_password&nbsp; Username&nbsp; Newpassword</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">(4) 查看当前用户列表</span><br style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px"><span style="color:rgb(75, 75, 75);font-family:Verdana, Geneva, Arial, Helvetica, sans-serif;font-size:13px">rabbitmqctl&nbsp; list_users</span><span style="color:rgb(51, 51, 51);font-family:doublevregular, 'Microsoft YaHei', 'Trebuchet MS', Verdana, Tahoma, Arial, sans-serif"><span><br/>
<span style="font-size:24px">3，</span>经过测试发现，我在接收端改过交换机名字后，用生产端发送以前的交换机名字，接收端还能收到，&nbsp;是因为我们每次在接收端创建交换机过后都保存在了接收端的服务器上面，虽然程序已经改变了，但是还是能接收到以前的队列的消息。可以用</span></span><span style="color:rgb(51, 51, 51);font-family:'Microsoft Yahei', 微软雅黑, arial, 宋体, sans-serif;font-size:14px;text-align:justify">管理RabbitMq的命令 rabbitmqctl list_exchanges 查看rabbitMQ里面已经声明的交换机，</span><br/>
</div>
<br/>
</div>
