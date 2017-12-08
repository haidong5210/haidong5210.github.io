# Django-model基础 #
## orm ##
映射关系：
表名--------类名
字段-------属性
表记录-----类实力对象
备注：通过在setting文件中设置，可以查看orm翻译成的sql语句。在setting中设置。
```python
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'console':{
            'level':'DEBUG',
            'class':'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'propagate': True,
            'level':'DEBUG',
        },
    }
}　　
```
## 创建表 ##
创建表就是创建类。
创建一个书籍的表,在modles.py文件中，如下
```python
from django.db import models

class Book(models.Model):
	bid=models.Autofield(primarykey=Ture)
	book_name=models.Charfield(max_length=32)
	auther = models.CharField(max_length=32)
    price = models.DecimalField(max_digits=5,decimal_places=2)
```

- 注意事项：
1，表的名称为appname_类名，是根据模型中的元素自动生成的。
2，当使用的时候，需要在app文件下的init文件中，导入pymysql，目的是让pycharm识别pymysql模块，
```python
import pymysql
pymysql.install_as_MySQLdb()
```
**字段选项**

```python
(1)null

如果为True，Django 将用NULL 来在数据库中存储空值。 默认值是 False.

(2)blank

如果为True，该字段允许不填。默认为False。
要注意，这与 null 不同。null纯粹是数据库范畴的，而 blank 是数据验证范畴的。
如果一个字段的blank=True，表单的验证将允许该字段是空值。如果字段的blank=False，该字段就是必填的。

(2)default

字段的默认值。可以是一个值或者可调用对象。如果可调用 ，每有新对象被创建它都会被调用。

(3)primary_key

如果为True，那么这个字段就是模型的主键。如果你没有指定任何一个字段的primary_key=True，
Django 就会自动添加一个IntegerField字段做为主键，所以除非你想覆盖默认的主键行为，
否则没必要设置任何一个字段的primary_key=True。

(4)unique

如果该值设置为 True, 这个数据字段的值在整张表中必须是唯一的

(5)choices
由二元组组成的一个可迭代对象（例如，列表或元组），用来给字段提供选择项。 如果设置了choices ，默认的表单将是一个选择框而不是标准的文本框，而且这个选择框的选项就是choices 中的选项。

这是一个关于 choices 列表的例子：

YEAR_IN_SCHOOL_CHOICES = (
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
)
每个元组中的第一个元素，是存储在数据库中的值；第二个元素是在管理界面或 ModelChoiceField 中用作显示的内容。 在一个给定的 model 类的实例中，想得到某个 choices 字段的显示值，就调用 get_FOO_display 方法(这里的 FOO 就是 choices 字段的名称 )。例如：

from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)


>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
'L'
>>> p.get_shirt_size_display()
'Large'
```
## 添加表记录 ##

**普通字段**

```python
方式一：
book_obj = Book(book_name="哈哈哈",auther="海燕",price=122)
book_obj.save()    #将数据保存到数据库


方式二：
models.Book.objects.create(book_name="哈哈哈",auther="海燕",price=122)
```
**外键字段  一对多**

```python
首先，添加外键值的时候，子表必须有记录。
方式一：
models.Book.objects.create(title="金瓶眉",publishDate="2012-12-12",price=665,pageNum=334,publish_id=1)    #外键创建的时候写的是publish，django把他存成publish_id

方式二：
publish_obj=models.Publish.objects.get(nid=1)  #先取到子表里的对象
Book.objects.create(title="金瓶眉",publishDate="2012-12-12",price=665,pageNum=334,publish=publish_obj)     # 外键=找到的对象
```
**多对多字段**

```python
外键写到哪个类里面就找哪一个：
首先找到一本书：给这本书添加关联！
book_obj=Book.objects.create(title="追风筝的人",publishDate="2012-11-12",price=69,pageNum=314,publish_id=1)

再找到与之关联的作者：
author_yuan=Author.objects.create(name="yuan",age=23,authorDetail_id=1)
author_egon=Author.objects.create(name="egon",age=32,authorDetail_id=2)

book_obj.authors.add(author_yuan,author_egon)#  将某个特定的 model 对象添加到被关联对象集合中。  
 
=======    book_obj.authors.add(*[])   #[]的意思是找到所有要添加的queryset让后给函数传参数，用*[]。

解除关系：
book_obj.authors.remove()     # 将某个特定的对象从被关联对象集合中去除。    ======   book_obj.authors.remove(*[])
book_obj.authors.clear()       #清空被关联对象集合。
```
## 修改表记录 ##

```python
方式一：
book_obj=models.Book.object.get(bid=2)   #get方法只能取到一个，取多个会报错
book_obj.auther="jingkong"
这种修改方式，看他从转化成的sql语句看效率很低。


方式二：
models.Book.object.filter(id=2).update(auther="jingkong",...)   update 是Queryset的方法，不能用get，get返回来的是一个model对象。
```
- 注意：

<1> 第二种方式修改不能用get的原因是：update是QuerySet对象的方法，get返回的是一个model对象，它没有update方法，而filter返回的是一个QuerySet对象(filter里面的条件可能有多个条件符合，比如name＝'alvin',可能有两个name＝'alvin'的行数据)。

<2>在“插入和更新数据”小节中，我们有提到模型的save()方法，这个方法会更新一行里的所有列。 而某些情况下，我们只需要更新行里的某几列。

此外，update()方法对于任何结果集（QuerySet）均有效，这意味着你可以同时更新多条记录update()方法会返回一个整型数值，表示受影响的记录条数。

注意，这里因为update返回的是一个整形，所以没法用query属性；对于每次创建一个对象，想显示对应的raw sql，需要在settings加上日志记录部分

## 删除表记录 ##
删除方法就是 delete()。它运行时立即删除对象而不返回任何值。例如：
```python
e.delete()
```
你也可以一次性删除多个对象。每个 QuerySet 都有一个 delete() 方法，它一次性删除 QuerySet 中所有的对象。

例如，下面的代码将删除 pub_date 是2005年的 Entry 对象：
```python
Entry.objects.filter(pub_date__year=2005).delete()
```
要牢记这一点：无论在什么情况下，QuerySet 中的 delete() 方法都只使用一条 SQL 语句一次性删除所有对象，而并不是分别删除每个对象。如果你想使用在 model 中自定义的 delete() 方法，就要自行调用每个对象的delete 方法。(例如，遍历 QuerySet，在每个对象上调用 delete()方法)，而不是使用 QuerySet 中的 delete()方法。

在 Django 删除对象时，会模仿 SQL 约束 ON DELETE CASCADE 的行为，换句话说，删除一个对象时也会删除与它相关联的外键对象。例如：
```python
b = Blog.objects.get(pk=1)
# This will delete the Blog and all of its Entry objects.
b.delete()
```
要注意的是： delete() 方法是 QuerySet 上的方法，但并不适用于 Manager 本身。这是一种保护机制，是为了避免意外地调用 Entry.objects.delete() 方法导致 所有的 记录被误删除。如果你确认要删除所有的对象，那么你必须显式地调用：
```pyton
Entry.objects.all().delete()
```
## 查询表记录 ##
**查询相关API**

```pyhton
<1> all():                 查询所有结果
 
<2> filter(**kwargs):      它包含了与所给筛选条件相匹配的对象
 
<3> get(**kwargs):         返回与所给筛选条件相匹配的对象，返回结果有且只有一个，
                           如果符合筛选条件的对象超过一个或者没有都会抛出错误。
 
<4> exclude(**kwargs):     它包含了与所给筛选条件不匹配的对象
 
<5> values(*field):        返回一个对应字段的字典，——一个特殊的QuerySet，运行后得到的并不是一系列
                           model的实例化对象，而是一个可迭代的字典序列
 
<6> values_list(*field):   它与values()非常相似，它返回的是一个元组序列，values返回的是一个字典序列
 
<7> order_by(*field):      对查询结果排序
 
<8> reverse():             对查询结果反向排序
 
<9> distinct():            从返回结果中剔除重复纪录
 
<10> count():              返回数据库中匹配查询(QuerySet)的对象数量。
 
<11> first():              返回第一条记录
 
<12> last():               返回最后一条记录
 
<13> exists():             如果QuerySet包含数据，就返回True，否则返回False
```
注意：一定区分object与querySet的区别 ！！！

**双下划线之单表查询**

```python
models.Tb1.objects.filter(id__lt=10, id__gt=1)   # 获取id大于1 且 小于10的值 
lt  是less than的缩写，小于， gt是 greater than的缩写，大于。
小于等于：lte 是 less than or equal to的缩写。

models.Tb1.objects.filter(id__in=[11, 22, 33])   # 获取id等于11、22、33的数据
models.Tb1.objects.exclude(id__in=[11, 22, 33])  # not in
 
models.Tb1.objects.filter(name__contains="ven")
models.Tb1.objects.filter(name__icontains="ven") # icontains大小写不敏感
 
models.Tb1.objects.filter(id__range=[1, 2])      # 范围bettwen and
 
startswith，istartswith, endswith, iendswith　
```
**基于对象的跨表查询**（对应的sql语句是子查询，查询一个标志后，再利用查到的再去查询）

- 一对多查询（Publish与Book)

```python
正向查询（外键在哪个类里面从这个类开始查找就是正向查询）
查询nid=1的书籍的出版社所在的城市 
book_obj=models.Book.objects.get(nid=1)
book_obj.publish.city   #book_obj.publish 是nid=1的书籍对象关联的出版社对象　
　

反向查询：与正向查询相反
#查询 人民出版社出版过的所有书籍
publish_obj=models.Publish.objects.get(name="人民出版社")
publish_onj.book_set.all()  # 与人民出版社关联的所有书籍对象集合
publish_onj.book_set.all().values(title) 
```
- 一对一查询(Author 与 AuthorDetail)

```pyhton
正向查询：
#查询egon作者的手机号
auther_obj=models.Auther.objects.get(name="egon")
auther_obj.autherDetail.telephone   #auther_obj.autherDetail是egon作者关联的所有详细信息

反向查询
#查询所有住址在北京的作者的姓名
authorDetail_list=model.AuthorDetail.objects.filter(addr="beijing")
for obj in authorDetail_list:
	print(obj.auther.name)
```
- 多对多查询 (Author 与 Book)

```python
正向查询：
# 金瓶眉所有作者的名字以及手机号
book_obj=models.Book.objects.filter(title="金瓶梅")[0]
book_obj.authers.values("name","telephone")

反向查询：
查询egon出过的所有书籍的名字
auther_obj=models.Auther.get(name="egon")
book_list=auther_obj.book_set.values("title")
```
- 注意：

你可以通过在 ForeignKey() 和ManyToManyField的定义中设置 related_name 的值来覆写 FOO_set 的名称。例如，如果 Article model 中做一下更改： publish = ForeignKey(Blog, **related_name='bookList'**)，那么接下来就会如我们看到这般：
```python
# 查询 人民出版社出版过的所有书籍
 
   publish=Publish.objects.get(name="人民出版社")
 
   book_list=publish.bookList.all()  # 与人民出版社关联的所有书籍对象集合
```

**基于双下划线的跨表查询**（对应的sql语句是联表查询inner join，更高效，推荐使用）

更高效，更直观的关联关系，他能自动确认sql，join练习，要做跨关系查询，就使用两个下划线来链接模型，直到最终链接到你想要的model为止。

model__字段
```python
# 练习1:  查询人民出版社出版过的所有书籍的名字与价格(一对多)

    # 正向查询 按字段:publish

    queryResult=Book.objects
　　　　　　　　　　　　.filter(publish__name="人民出版社")
　　　　　　　　　　　　.values_list("title","price")

    # 反向查询 按表名:book

    queryResult=Publish.objects
　　　　　　　　　　　　　　.filter(name="人民出版社")
　　　　　　　　　　　　　　.values_list("book__title","book__price")



# 练习2: 查询egon出过的所有书籍的名字(多对多)

    # 正向查询 按字段:authors:
    queryResult=Book.objects
　　　　　　　　　　　　.filter(authors__name="yuan")
　　　　　　　　　　　　.values_list("title")

    # 反向查询 按表名:book
    queryResult=Author.objects
　　　　　　　　　　　　　　.filter(name="yuan")
　　　　　　　　　　　　　　.values_list("book__title","book__price")


# 练习3: 查询人民出版社出版过的所有书籍的名字以及作者的姓名


    # 正向查询
    queryResult=Book.objects
　　　　　　　　　　　　.filter(publish__name="人民出版社")
　　　　　　　　　　　　.values_list("title","authors__name")
    # 反向查询
    queryResult=Publish.objects
　　　　　　　　　　　　　　.filter(name="人民出版社")
　　　　　　　　　　　　　　.values_list("book__title","book__authors__age","book__authors__name")


# 练习4: 手机号以151开头的作者出版过的所有书籍名称以及出版社名称

    queryResult=Book.objects
　　　　　　　　　　　　.filter(authors__authorDetail__telephone__regex="151")
　　　　　　　　　　　　.values_list("title","publish__name")
```
*注意*
反向查询时，如果定义了related_name ，则用related_name替换表名，例如： publish = ForeignKey(Blog, related_name='bookList')：如果没有就用类名。
```python
# 练习1:  查询人民出版社出版过的所有书籍的名字与价格(一对多)
 
    # 反向查询 不再按表名:book,而是related_name:bookList
 
    queryResult=Publish.objects
　　　　　　　　　　　　　　.filter(name="人民出版社")
　　　　　　　　　　　　　　.values_list("bookList__title","bookList__price")
```
**聚合查询**
- 聚合函数：aggregate（*args，**kwargs）

```python
# 计算所有图书的平均价格
    >>> from django.db.models import Avg
    >>> Book.objects.all().aggregate(Avg('price'))
    {'price__avg': 34.35}
```
aggregate()是QuerySet 的一个终止子句，意思是说，它返回一个包含一些键值对的字典。键的名称是聚合值的标识符，值是计算出来的聚合值。键的名称是按照字段和聚合函数的名称自动生成出来的。如果你想要为聚合值指定一个名称，可以向聚合子句提供它。
```python
>>> Book.objects.aggregate(average_price=Avg('price'))
{'average_price': 34.35}
```
如果你希望生成不止一个聚合，你可以向aggregate()子句中添加另一个参数。所以，如果你也想知道所有图书价格的最大值和最小值，可以这样查询：
```python
>>> from django.db.models import Avg, Max, Min
>>> Book.objects.aggregate(Avg('price'), Max('price'), Min('price'))
{'price__avg': 34.35, 'price__max': Decimal('81.20'), 'price__min': Decimal('12.99')}
```

**分组：annotate**
为QuerySet中每一个对象都生成一个独立的汇总值

(1) 练习：统计每一本书的作者个数
```python
bookList=Book.objects.annotate(authorsNum=Count('authors')) #分组的条件就是前面找到的内容，这个的分组条件就是全部的book分为一组。annotate内的参数是分组之后的数据付给book为字段。
for book_obj in bookList:
    print(book_obj.title,book_obj.authorsNum)
```
（2）统计每一个出版社的最便宜的书
```python
queryResult=Book.objects.values("publish__name").annotate(MinPrice=Min('price'))  #分组条件就是找到的出版社，然后以出版社分组，后面annotate内的参数是付给book的字段，可以通过属性直接查找。
```
**F查询与Q查询**
- F查询（字段和字段之间的比较，不能直接写成filed=filed，所以django引入了F）
Django 提供 F() 来做这样的比较。F() 的实例可以在查询中引用字段，来比较同一个 model 实例中两个不同字段的值。
```python
# 查询评论数大于收藏数的书籍
 
   from django.db.models import F
   Book.objects.filter(commnetNum__lt=F('keepNum'))
```
Django 支持 F() 对象之间以及 F() 对象和常数之间的加减乘除和取模的操作。
```python
# 查询评论数大于收藏数2倍的书籍
    Book.objects.filter(commnetNum__lt=F('keepNum')*2)
```
```python
修改操作也可以使用F函数,比如将每一本书的价格提高30元：
Book.objects.all().update(price=F("price")+30)　
```

- Q查询

filter() 等方法中的关键字参数查询都是一起进行“AND” 的。 如果你需要执行更复杂的查询（例如OR 语句），你可以使用Q 对象。
```python
from django.db.models import Q
Q(title__startswith='Py')
```
Q 对象可以使用& 和| 和~操作符组合起来。&是且的意思，|是获得意思，~是非的意思
```python
bookList=Book.objects.filter(Q(authors__name="yuan")|Q(authors__name="egon"))

等同于下面的SQL WHERE 子句：
	
WHERE name ="yuan" OR name ="egon"
```
你可以组合& 和|  操作符以及使用括号进行分组来编写任意复杂的Q 对象。同时，Q 对象可以使用~ 操作符取反，这允许组合正常的查询和取反(NOT) 查询：
```python
bookList=Book.objects.filter(Q(authors__name="yuan") & ~Q(publishDate__year=2017)).values_list("title")
```
查询函数可以混合使用Q 对象和关键字参数。所有提供给查询函数的参数（关键字参数或Q 对象）都将"AND”在一起。但是，如果出现Q 对象，它必须位于所有关键字参数的前面。例如：
```python
 bookList=Book.objects.filter(Q(publishDate__year=2016) | Q(publishDate__year=2017),title__icontains="python")
```






