# 视图层之试图函数（views） #

- 试图函数必须包含两个对象：
	- request ------>请求信息
	- Httpresponse----->响应字符串
	
一个视图函数，简称视图，是一个简单的Python 函数，它接受Web请求并且返回Web响应。响应可以是一张网页的HTML内容，一个重定向，一个404错误，一个XML文档，或者一张图片. . . 是任何东西都可以。无论视图本身包含什么逻辑，都要返回响应。代码写在哪里也无所谓，只要它在你的Python目录下面。除此之外没有更多的要求了——可以说“没有什么神奇的地方”。为了将代码放在某处，约定是将视图放置在项目或应用程序目录中的名为views.py的文件中。
##一个简单的试图##
下面是一个返回当前日期和时间作为HTML文档的视图：

```python
from django.http import HttpResponse
import datetime

def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```

让我们逐行阅读上面的代码：

- 首先，我们从 django.http模块导入了HttpResponse类，以及Python的datetime库。
- 接着，我们定义了current_datetime函数。它就是视图函数。每个视图函数都使用HttpRequest对象作为第一个参数，并且通常称之为request
- 注意,视图函数的名称并不重要；不需要用一个统一的命名方式来命名，以便让Django识别它。我们将其命名为current_datetime，是因为这个名称能够精确地反映出它的功能。
- 这个视图会返回一个HttpResponse对象，其中包含生成的响应。每个视图函数都负责返回一个HttpResponse对象。

![](http://images2015.cnblogs.com/blog/877318/201607/877318-20160725101445044-1768854009.jpg)
##request对象##

```python
属性：
request.path               请求页面的全路径，不包括域名

request.method             请求中使用的HTTP方法的字符串表示。全大写表示。例如
                           if  req.method=="GET":

                         		do_something()

              			   elif req.method=="POST":

                        		do_something_else()

request.GET                包含所有HTTP GET参数的类字典对象,获得响应name的值，request.GET.get("name")

request.POST               包含所有HTTP POST参数的类字典对象,
获得响应name的值，request.POST.get("name")	
```

方法：
```python
get_full_path :这个方法，如果请求数据的方式是get路径中会带着数据

request.POST.getlist("hobby")：在input框中的多选框，或者是select标签，涉及到values的值有多个，取到values的值，就用这个方法，如果还用上面的方法只会取到最后一个值。value只有一个的时候，用上面的方法即可。
```
##render函数

```python
render(request, template_name[, context]）

结合一个给定的模板和一个给定的上下文字典，并返回一个渲染后的 HttpResponse 对象。

参数：
     request： 用于生成响应的请求对象。

     template_name：要使用的模板的完整名称，可选的参数

     context：添加到模板上下文的一个字典。默认是一个空字典。如果字典中的某个值是可调用的，视图将在渲染模板之前调用它。

     content_type：生成的文档要使用的MIME类型。默认为DEFAULT_CONTENT_TYPE 设置的值。

     status：响应的状态码。默认为200。
```

##redirect函数

```pyhton
参数可以是：

一个模型：将调用模型的get_absolute_url() 函数
一个视图，可以带有参数：将使用urlresolvers.reverse 来反向解析名称
一个绝对的或相对的URL，将原封不动的作为重定向的位置。
默认返回一个临时的重定向；传递permanent=True 可以返回一个永久的重定向。

示例:

你可以用多种方式使用redirect() 函数。
```
传递要重定向的一个硬编码的URL

```pyhton
def my_view(request):
    ...
    return redirect('/some/url/')
也可以是一个完整的URL：
def my_view(request):
    ...
    return redirect('http://example.com/')
```
传递一个视图的名称

```python
可以带有位置参数和关键字参数；将使用reverse() 方法反向解析URL：　
def my_view(request):
    ...
    return redirect('some-view-name', foo='bar')
传递要重定向的一个硬编码的URL
```
传递一个对象

```pyhton
将调用get_absolute_url() 方法来获取重定向的URL：
rom django.shortcuts import redirect
 
def my_view(request):
    ...
    object = MyModel.objects.get(...)
    return redirect(object)
```
跳转（重定向）应用
```pyhton
-----------------------------------url.py

 url(r"login",   views.login),
 url(r"yuan_back",   views.yuan_back),

-----------------------------------views.py
def login(req):
    if req.method=="POST":
        if 1:
            # return redirect("/yuan_back/")
            name="yuanhao"

            return render(req,"my backend.html",locals())

    return render(req,"login.html",locals())


def yuan_back(req):

    name="海东"

    return render(req,"my backend.html",locals())

-----------------------------------login.html

<form action="/login/" method="post">
    <p>姓名<input type="text" name="username"></p>
    <p>性别<input type="text" name="sex"></p>
    <p>邮箱<input type="text" name="email"></p>
    <p><input type="submit" value="submit"></p>
</form>
-----------------------------------my backend.html
<h1>用户{{ name }}你好</h1>


注意：render和redirect的区别:

1、 if 页面需要模板语言渲染,需要的将数据库的数据加载到html,那么render方法则不会显示这一部分。

2、 the most important: url没有跳转到/yuan_back/,而是还在/login/,所以当刷新后又得重新登录。
```