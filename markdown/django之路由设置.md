# 视图层之路由配置系统（views） #

- url配置
```python
 urlpatterns = [
         url(正则表达式, views视图函数，参数，别名),]
参数说明：

    一个正则表达式字符串
    一个可调用对象，通常为一个视图函数或一个指定视图函数路径的字符串
    可选的要传递给视图函数的默认参数（字典形式）
    一个可选的name参数
```
## URLconf的正则字符串参数 ##
- 1,简单配置

```python
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/([0-9]{4})/$', views.year_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/$', views.month_archive),
    url(r'^articles/([0-9]{4})/([0-9]{2})/([0-9]+)/$', views.article_detail),
]
匹配根目录：r"^$"

注意：
1，一旦匹配成功不在继续
2，若要从URL 中捕获一个值，只需要在它周围放置一对圆括号。
3，不需要添加一个前导的反斜杠，因为每个URL 都有。例如，应该是^articles 而不是 ^/articles。
4，每个正则表达式前面的'r' 是可选的但是建议加上。
*****5，可以再全局的urls文件中设置：调用include模块
url(r'^blog/', include('blog.urls')) ：这个设置的意思就是 路径以blog开头，包含blog文件下的urls内的映射关系，（两个blog是可以随便写的）
```
- 2，有名分组（named group）
上面的示例使用简单的、没有命名的正则表达式组（通过圆括号）来捕获URL 中的值并以位置 参数传递给视图。在更高级的用法中，可以使用命名的正则表达式组来捕获URL 中的值并以关键字 参数传递给视图。

在Python 正则表达式中，命名正则表达式组的语法是(?P<name>pattern)，其中name 是组的名称，pattern 是要匹配的模式。
下面是以上URLconf 使用命名组的重写：
```pyhton
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^articles/2003/$', views.special_case_2003),
    url(r'^articles/(?P<year>[0-9]{4})/$', views.year_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/$', views.month_archive),
    url(r'^articles/(?P<year>[0-9]{4})/(?P<month>[0-9]{2})/(?P<day>[0-9]{2})/$', views.article_detail),
]
```
这个实现与前面的示例完全相同，只有一个细微的差别：捕获的值作为关键字参数而不是位置参数传递给视图函数。是django帮你做到的。像下面这样：
```pyhton
  /articles/2005/03/    
    请求将调用views.month_archive(request, year='2005', month='03')函数
    /articles/2003/03/03/ 
    请求将调用函数views.article_detail(request, year='2003', month='03', day='03')
```
在实际应用中，这意味你的URLconf 会更加明晰且不容易产生参数顺序问题的错误 —— 你可以在你的视图函数定义中重新安排参数的顺序。当然，这些好处是以简洁为代价；有些开发人员认为命名组语法丑陋而繁琐。
## url的反向解析 ##
```python
urlpatterns = [
         url(正则表达式, views视图函数，参数，别名),]
别名就是用来反向解析的，当需要改变正则的时候后面对应的html页面也要改，很多的映射关系都改的话，会很繁琐，解耦严重。用别名来代替就会改善这个问题。

from django.conf.urls import url

from . import views

urlpatterns = [
    #...
    url(r'^articles/([0-9]{4})/$', views.year_archive, name='news-year-archive'),
    #...
]

对应：
<a href="{% url 'news-year-archive' 2012 %}">2012 Archive</a>

<ul>
{% for yearvar in year_list %}
<li><a href="{% url 'news-year-archive' yearvar %}">{{ yearvar }} Archive</a></li>
{% endfor %}
</ul>

在Python 代码中，这样使用：
from django.core.urlresolvers import reverse
from django.http import HttpResponseRedirect

def redirect_to_year(request):
    # ...
    year = 2006
    # ...
    return HttpResponseRedirect(reverse('news-year-archive', args=(year,)))
```