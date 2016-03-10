---
layout: post
title: 多服务器session共享
tags:
- linux
- php
- session
- session共享
description: linux php session session共享
---
此方法实用于多服务器在局域网络环境下，如是多服务器在多地区下，则研究其他方法。
<br>
方案流程是：在其中一台服务器建立共享目录，让其他的服务器都能访问到，在把全部服务器的session的存储目录指向到该目录则可以实现。 
<br>
1，session默认是保存到c:\windows\temp目录下，但是通过修改php.ini中的session.save_path值可以改变session的保存路径。 
如：session.save_path = "d:/wamp/tmp" 
执行该代码后，就会在d:/wamp/tmp目录下，新增一个文件名为：sess_***的文件，打开之后，内容如下：name|s:8:"marcofly"; 
2，将session的默认保存目录改变到共享目录下则可以实现。
3，缺点是如果这台共享服务器宕机了，就挂了。
<br>