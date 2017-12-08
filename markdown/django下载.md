#Django
##MTV模型

- urls: 路径与试图函数的映射关系
- views：逻辑处理
- models：数据库相关的操作
- template：模板语法----->将变量（数据）如何巧妙的嵌入到html页面中。
![](http://images2017.cnblogs.com/blog/877318/201710/877318-20171019165754693-400950154.png)

##下载django
- 在解释器的匹配目录下 下载
	- 命令：pip3 install django
- 创建django项目
	- django-admin startproject 项目名
	- mysite：项目名
		- manage.py：启动文件，控制项目命令
		- mysite 与创建的项目名称一样
- 创建一个应用：在项目的文件下
	- pyhton manage.py startapp 应用名
- 启动django项目：
	- pyhton manage.py runserver ip port
	- ![](http://images2017.cnblogs.com/blog/1197778/201710/1197778-20171020200114490-1433307122.jpg)
			 




