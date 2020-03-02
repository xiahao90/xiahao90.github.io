---
title: python-一个简单的爬网站咨询程序
category: 2017
order: 98
date:2017-07-04
---
<div>
	<div>一直使用的php语言开发程序。但是这两年python的爬取程序挺火，周围行业的朋友都在使用。我也来摸索一下。花了点时间弄了一个程序试试效果。勉强能把栏目下所有的文章爬去下来。</div>
	<div>废话少说，直接上代码+注释</div>
	<div>次程序只是把网站的栏目下面的所有资讯的连接，抓下来了存入了json格式的文件，</div>
	<div>准备下一步，在解析json文件，挨个挨个抓去内容。</div>
	<pre>
	<code>
		#!/usr/bin/python
		# -*- coding: UTF-8 -*-  
		import urllib.request  
		import json
		#获取网页内容用的，
		def getHtml(url):  
		    page = urllib.request.urlopen(url)  
		    html=page.read()  
		    return html
		#截取网页内容用的，本人不擅长写正则。
		def get_list(text,str1,str2,pd):  
		    array_1=text.split(str1)
		    del array_1[0]
		    new_text = ""
		    new_array = []
		    for value in array_1:
		        array_2=value.split(str2)
		        if  pd==True:
		            new_text=array_2[0]
		        else:
		            new_array.append(array_2[0])
		    if pd==True:
		        return new_text
		    else:
		        return new_array
		#网页连接栏目英文名称+页码数量
		#网站太敏感，不公布。
		zongzhan={'jtll':55,'dsjq':20,'llxs':45,'qsxh':29,'rq':56,'wxgd':43,'xiaoyou':61,'xaijq':35}
		for key in zongzhan:
			data=[]
			for num in range(1,zongzhan[key]):
				url=''
				if num==1:
					url=("http://www.xxxx.com/list/"+str(key)+"/")
				else :
					url=("http://www.xxxx.com/list/"+str(key)+"/index-"+str(num)+".html")
				print(url)
				print("\r\n")
				html=getHtml(url).decode("UTF-8");
				#截取网页里面的 新闻块的数据
				content=get_list(html,'<div class=list>','</div>',1)
				#截取网页里面的 新闻块 url的数据，返回的是数组
				c_url=get_list(content,'<a href="','" title="',0);
				#截取网页里面的 新闻块 标题的数据，返回的是数组
				c_title=get_list(content,'title="','" target=',0);
				for key1 in range(len(c_url)):
					temp={'url':c_url[key1],'title':c_title[key1]}
					data.append(temp)
					del temp
			data1=str(json.dumps(data))
			#把一个栏目存入文件，大功告成，开始第二个栏目
			fo = open(key+".txt", "wb")
			fo.write(data1.encode());
			fo.close()
			del data
			del data1

	</code>
	</pre>
	<br>
	<br>
</div>