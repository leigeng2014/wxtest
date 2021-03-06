wxtest
======

微信公众账号测试平台。本系统同时支持tornado和百度的BAE。

使用tornado建立的WEB服务平台
=========================

1. 运行WEB服务平台命令
------------------

		python mainServer

2. web应用配置
------------
* 配置文件

		config/web.ini

* 添加一个WEB应用

	如下例所示，在[url-patterns]下添加:

		classes.web.TestHandler.TestHandler = /Test
		
	“=”号前面是类的地址路径。“=”号后面是类的URL映射地址。

	web应用编写方法参考：http://sebug.net/paper/books/tornado/#_4

* 添加一个模板

	如下例所示，在[templates]下添加:

		MainHandler = templates/index.html

	“=”号前面是程序内引用模板的名称。“=”号后面是模板位置。

* 程序内引用模板示例

		self.render(TEMPLATES['MainHandler'], title="电信微信服务平台")	

	模板编写方法参考：http://sebug.net/paper/books/tornado/#_5

使用BAE建立的WEB服务平台
=====================

1. 百度BAE配置
-------------
* 配置文件

		app.conf
		
* 在配置文件中添加一个WEB应用
		
	如下例所示，在handlers:下添加:

		- url : /Test
		script: index.py

	注意：此处添加的url必须与下面web应用配置的url映射地址一致。


2. web应用配置
-------------

* 配置文件

		config/web.ini

* 在配置文件中添加一个WEB应用

	如下例所示，在[url-patterns]下添加:

		classes.web.TestHandler.TestHandler = /Test

		“=”号前面是类的地址路径。“=”号后面是类的URL映射地址。

	web应用编写方法参考：http://sebug.net/paper/books/tornado/#_4

3. 添加一个模板
-------------

* 如下例所示，在[templates]下添加:

		MainHandler = templates/index.html

	“=”号前面是程序内引用模板的名称。“=”号后面是模板位置。

* 程序内引用模板示例:

		self.render(TEMPLATES['MainHandler'], title="电信微信服务平台")

	模板编写方法参考：http://sebug.net/paper/books/tornado/#_5


微信消息配置
==========

1. 配置文件
----------

		config/message.ini

2. 配置说明
----------
* 文本消息，如下例所示

		[root]
		command = 0
		class = wxplatform.MessageManage.TextMessage
		welcome = 欢迎参加电信营销与服务微信平台测试！发送数字进入菜单。当前菜单如下：
				输入“1”，进入客户服务
				输入“2”，进入优惠信息查询
				输入“3”，进入营业厅服务

	command：表示微信公众账号接受用户发来的指令
	class：表示文本消息的处理类
	welcome：表示被文本消息类所处理的欢迎内容。即：返回给用户的内容。

* 图文消息，如下例所示

		[产品信息查询]
		command = 11
		class = wxplatform.MessageManage.NewsMessage
		articles = wxplatform.query.ProductInfo.ProductInfo

	command：表示微信公众账号接受用户发来的指令
	class：表示图文消息的处理类
	articles：表示被图文消息类所处理的返回文章内容的自定义类。即：处理返回用户文章内容的类。

	以wxplatform.query.ProductInfo.ProductInfo为例，我们可以看到返回文章内容的类必须实现如下结构：

		class ProductInfo(object):
		"""docstring for ProductInfo"""
	
			def queryArticles(self, pushMsg, configSection):
				articles = list()

				article1 = NewsArticle()
				article1.title = "产品信息查询"
				article1.description = ""
				article1.picUrl = BAE_URL + "static/images/logo.png"
				article1.url = "http://telecomtest.duapp.com/static/html/productInfo.html"
				articles.append(article1)
				
				...

				return articles

