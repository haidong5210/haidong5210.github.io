# COOKIE 与 SESSION #

**概念**
cookie不属于http协议范围，由于http协议无法保持状态，但实际情况，我们却又需要“保持状态”，因此cookie就是在这样一个场景下诞生。

cookie的工作原理是：由服务器产生内容，浏览器收到请求后保存在本地；当浏览器再次访问时，浏览器会自动带上cookie，这样服务器就能通过cookie的内容来判断这个是“谁”了。

cookie虽然在一定程度上解决了“保持状态”的需求，但是由于cookie本身最大支持4096字节，以及cookie本身保存在客户端，可能被拦截或窃取，因此就需要有一种新的东西，它能支持更多的字节，并且他保存在服务器，有较高的安全性。这就是session。

问题来了，基于http协议的无状态特征，服务器根本就不知道访问者是“谁”。那么上述的cookie就起到桥接的作用。

我们可以给每个客户端的cookie分配一个唯一的id，这样用户在访问时，通过cookie，服务器就知道来的人是“谁”。然后我们再根据不同的cookie的id，在服务器上保存一段时间的私密资料，如“账号密码”等等。

总结而言：cookie弥补了http无状态的不足，让服务器知道来的人是“谁”；但是cookie以文本的形式保存在本地，自身安全性较差；所以我们就通过cookie识别不同的用户，对应的在session里保存私密的信息以及超过4096字节的文本。

另外，上述所说的cookie和session其实是共通性的东西，不限于语言和框架
**登陆应用**
前几节的介绍中我们已经有能力制作一个登陆页面，在验证了用户名和密码的正确性后跳转到后台的页面。但是测试后也发现，如果绕过登陆页面。直接输入后台的url地址也可以直接访问的。这个显然是不合理的。其实我们缺失的就是cookie和session配合的验证。有了这个验证过程，我们就可以实现和其他网站一样必须登录才能进入后台页面了。

先说一下这种认证的机制。每当我们使用一款浏览器访问一个登陆页面的时候，一旦我们通过了认证。服务器端就会发送一组随机唯一的字符串（假设是123abc）到浏览器端，这个被存储在浏览端的东西就叫cookie。而服务器端也会自己存储一下用户当前的状态，比如login=true，username=hahaha之类的用户信息。但是这种存储是以字典形式存储的，字典的唯一key就是刚才发给用户的唯一的cookie值。那么如果在服务器端查看session信息的话，理论上就会看到如下样子的字典

{'123abc':{'login':true,'username:hahaha'}}

因为每个cookie都是唯一的，所以我们在电脑上换个浏览器再登陆同一个网站也需要再次验证。那么为什么说我们只是理论上看到这样子的字典呢？因为处于安全性的考虑，其实对于上面那个大字典不光key值123abc是被加密的，value值{'login':true,'username:hahaha'}在服务器端也是一样被加密的。所以我们服务器上就算打开session信息看到的也是类似与以下样子的东西

{'123abc':dasdasdasd1231231da1231231}

知道了原理，下面就来用代码实现。
**Django实现的COOKIE**
- 1，获取Cookie

```python
request.COOKIES.get('key',defalut)   # 取值key键的值，有就是有的值，没有就是default设置的值。
request.get_signed_cookie(key, default=RAISE_ERROR, salt='', max_age=None)
    #参数：
        default: 默认值
           salt: 加密盐
        max_age: 后台控制过期时间
```
- 2,设置Cooike

```python
rep = HttpResponse(...) 或 rep ＝ render(request, ...) 或 rep ＝ redirect()


在response的返回值上设置cookie
rep.set_cookie(key,value,...)
rep.set_signed_cookie(key,value,salt='加密盐',...)　
```
参数：
```python
'''

def set_cookie(self, key,                 键
　　　　　　　　　　　　 value='',            值
　　　　　　　　　　　　 max_age=None,        超长时间
　　　　　　　　　　　　 expires=None,        超长时间
　　　　　　　　　　　　 path='/',           Cookie生效的路径，
                                         浏览器只会把cookie回传给带有该路径的页面，这样可以避免将
                                         cookie传给站点中的其他的应用。
                                         / 表示根路径，特殊的：根路径的cookie可以被任何url的页面访问
　　　　　　　　　　　　 
                     domain=None,         Cookie生效的域名
                                        
                                          你可用这个参数来构造一个跨站cookie。
                                          如， domain=".example.com"
                                          所构造的cookie对下面这些站点都是可读的：
                                          www.example.com 、 www2.example.com 
　　　　　　　　　　　　　　　　　　　　　　　　　和an.other.sub.domain.example.com 。
                                          如果该参数设置为 None ，cookie只能由设置它的站点读取。

　　　　　　　　　　　　 secure=False,        如果设置为 True ，浏览器将通过HTTPS来回传cookie。
　　　　　　　　　　　　 httponly=False       只能http协议传输，无法被JavaScript获取
                                         （不是绝对，底层抓包可以获取到也可以被覆盖）
　　　　　　　　　　): pass

'''
```
由于cookie保存在客户端的电脑上，所以，JavaScript和jquery也可以操作cookie。

```python
<script src='/static/js/jquery.cookie.js'>
 
</script> $.cookie("key", value,{ path: '/' });
```
- 3,删除Cookie

```python
	
response.delete_cookie("cookie_key",path="/",domain=name)

方式二：
del response.COOKIE["key"]
```
**Django实现的session**
- 1，基本操作

```python
request.session是一个字典
1、设置Sessions值
          request.session['session_name'] ="admin"
2、获取Sessions值
          session_name = request.session["session_name"]
					request.session.get(key, default=None)  # 有就取出，没有返回default设置的值
3、删除Sessions值
          del request.session["session_name"] #清除某一个
			request.session.flash    # 清除当前的所有session
4、检测是否操作session值
          if "session_name" is request.session :

 
5、pop(key) #取值 也有删除的意思
 
fav_color = request.session.pop('fav_color')
 
6、keys()
 
7、items()
```
- 2 流程解析图
![](http://images2017.cnblogs.com/blog/877318/201709/877318-20170929173142981-2106190717.png)
- 3 示例

```python
#登陆
def login(request):
	if request.method=="POST":
		username = request.POST.get("user")
		password = request.POST.get("pwd")
		user = models.UserInfo.objects.filter(username=username,password=password)
		if user:
			#设置session值，下次登陆主页面的时候验证需要
			request.session["is_blog"]=Ture
			request.session["username"]=username
			
			return redirect('/backend/')
	#登录不成功或第一访问就停留在登录页面
	return render(request,"login.html")

#主页
def backend(request):
	"""
    这里必须用读取字典的get()方法把is_login的value缺省设置为False，
    当用户访问backend这个url先尝试获取这个浏览器对应的session中的
    is_login的值。如果对方登录成功的话，在login里就已经把is_login
    的值修改为了True,反之这个值就是False的，不设置会报错
    """
	is_login=request.session.get('is_login',False)
	if is_login:
		return render(request,"..html")
	else:
 		"""
        如果访问的时候没有携带正确的session，
        就直接被重定向url回login页面
        """
		return redirect('/login/')

#注销
def log_out(request):
    """
    直接通过request.session['is_login']回去返回的时候，
    如果is_login对应的value值不存在会导致程序异常。所以
    需要做异常处理
    """
    try:
        #删除is_login对应的value值
        del request.session['is_login']
        
        # OR---->request.session.flush() # 删除django-session表中的当前对的一行记录

    except KeyError:
        pass
    #点击注销之后，直接重定向回登录页面
    return redirect('/login/')
```
- 4 session存储的相关配置

```python

（1）数据库配置（默认）：
Django默认支持Session，并且默认是将Session数据存储在数据库中，即：django_session 表中。
  
a. 配置 settings.py
  
    SESSION_ENGINE = 'django.contrib.sessions.backends.db'   # 引擎（默认）
      
    SESSION_COOKIE_NAME ＝ "sessionid"                       # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串（默认）
    SESSION_COOKIE_PATH ＝ "/"                               # Session的cookie保存的路径（默认）
    SESSION_COOKIE_DOMAIN = None                             # Session的cookie保存的域名（默认）
    SESSION_COOKIE_SECURE = False                            # 是否Https传输cookie（默认）
    SESSION_COOKIE_HTTPONLY = True                           # 是否Session的cookie只支持http传输（默认）
    SESSION_COOKIE_AGE = 1209600                             # Session的cookie失效日期（2周）（默认）
    SESSION_EXPIRE_AT_BROWSER_CLOSE = False                  # 是否关闭浏览器使得Session过期（默认）
    SESSION_SAVE_EVERY_REQUEST = False                       # 是否每次请求都保存Session，默认修改之后才保存（默认）

（2）缓存配置　
a. 配置 settings.py
  
    SESSION_ENGINE = 'django.contrib.sessions.backends.cache'  # 引擎
    SESSION_CACHE_ALIAS = 'default'                            # 使用的缓存别名（默认内存缓存，也可以是memcache），此处别名依赖缓存的设置
  
  
    SESSION_COOKIE_NAME ＝ "sessionid"                        # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串
    SESSION_COOKIE_PATH ＝ "/"                                # Session的cookie保存的路径
    SESSION_COOKIE_DOMAIN = None                              # Session的cookie保存的域名
    SESSION_COOKIE_SECURE = False                             # 是否Https传输cookie
    SESSION_COOKIE_HTTPONLY = True                            # 是否Session的cookie只支持http传输
    SESSION_COOKIE_AGE = 1209600                              # Session的cookie失效日期（2周）
    SESSION_EXPIRE_AT_BROWSER_CLOSE = False                   # 是否关闭浏览器使得Session过期
    SESSION_SAVE_EVERY_REQUEST = False                        # 是否每次请求都保存Session，默认修改之后才保存

（3）文件配置
a. 配置 settings.py
  
    SESSION_ENGINE = 'django.contrib.sessions.backends.file'    # 引擎
    SESSION_FILE_PATH = None                                    # 缓存文件路径，如果为None，则使用tempfile模块获取一个临时地址tempfile.gettempdir()        
    SESSION_COOKIE_NAME ＝ "sessionid"                          # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串
    SESSION_COOKIE_PATH ＝ "/"                                  # Session的cookie保存的路径
    SESSION_COOKIE_DOMAIN = None                                # Session的cookie保存的域名
    SESSION_COOKIE_SECURE = False                               # 是否Https传输cookie
    SESSION_COOKIE_HTTPONLY = True                              # 是否Session的cookie只支持http传输
    SESSION_COOKIE_AGE = 1209600                                # Session的cookie失效日期（2周）
    SESSION_EXPIRE_AT_BROWSER_CLOSE = False                     # 是否关闭浏览器使得Session过期
    SESSION_SAVE_EVERY_REQUEST = False                          # 是否每次请求都保存Session，默认修改之后才保存

```
**用户认证**
	
from django.contrib import auth

- 1 、authenticate()  

提供了用户认证，即验证用户名以及密码是否正确,一般需要username  password两个关键字参数

如果认证信息有效，会返回一个  User  对象。authenticate()会在User 对象上设置一个属性标识那种认证后端认证了该用户，且该信息在后面的登录过程中是需要的。当我们试图登陆一个从数据库中直接取出来不经过authenticate()的User对象会报错的！！

```python
1user = authenticate(username='someone',password='somepassword')
```

- 2 login(HttpRequest, user)　　

```python
该函数接受一个HttpRequest对象，以及一个认证了的User对象

此函数使用django的session框架给某个已认证的用户附加上session id等信息。

from django.contrib.auth import authenticate, login
   
def my_view(request):
  username = request.POST['username']
  password = request.POST['password']
  user = authenticate(username=username, password=password)
  if user is not None:
    login(request, user)
    # Redirect to a success page.
    ...
  else:
    # Return an 'invalid login' error message.
    ...
```

- 3 logout(request) 注销用户　

```python
from django.contrib.auth import logout
   
def logout_view(request):
  logout(request)
  # Redirect to a success page.

该函数接受一个HttpRequest对象，无返回值。当调用该函数时，当前请求的session信息会全部清除。该用户即使没有登录，使用该函数也不会报错
```

- 4 user对象的 is_authenticated()

```python
要求：

1  用户登陆后才能访问某些页面，

2  如果用户没有登录就访问该页面的话直接跳到登录页面

3  用户在跳转的登陆界面中完成登陆后，自动访问跳转到之前访问的地址

方法1:
def my_view(request):
  if not request.user.is_authenticated():
    return redirect('%s?next=%s' % (settings.LOGIN_URL, request.path))


方法2:

django已经为我们设计好了一个用于此种情况的装饰器：login_requierd()


from django.contrib.auth.decorators import login_required
      
@login_required
def my_view(request):
  ...
```
**User对象**
User 对象属性：username， password（必填项）password用哈希算法保存到数据库

is_staff ： 用户是否拥有网站的管理权限.

is_active ： 是否允许用户登录, 设置为``False``，可以不用删除用户来禁止 用户登录
- 1 is_authenticated()

如果是真正的 User 对象，返回值恒为 True 。 用于检查用户是否已经通过了认证。
通过认证并不意味着用户拥有任何权限，甚至也不检查该用户是否处于激活状态，这只是表明用户成功的通过了认证。 这个方法很重要, 在后台用request.user.is_authenticated()判断用户是否已经登录，如果true则可以向前台展示request.user.name
- 2创建用户

```python
rom django.contrib.auth.models import User
user = User.objects.create_user（username='',password='',email=''）
```
-3check_password(passwd)
用户需要修改密码的时候 首先要让他输入原来的密码 ，如果给定的字符串通过了密码检查，返回 True
- 4修改密码
user = User.objects.get(username='')
user.set_password(password='')
user.save　
- 5 s实例


```python
Z注册：
def sign_up(request):
 
    state = None
    if request.method == 'POST':
 
        password = request.POST.get('password', '')
        repeat_password = request.POST.get('repeat_password', '')
        email=request.POST.get('email', '')
        username = request.POST.get('username', '')
        if User.objects.filter(username=username):
                state = 'user_exist'
        else:
                new_user = User.objects.create_user(username=username, password=password,email=email)
                new_user.save()
 
                return redirect('/book/')
    content = {
        'state': state,
        'user': None,
    }
    return render(request, 'sign_up.html', content)
```
```python
修改密码
@login_required
def set_password(request):
    user = request.user
    state = None
    if request.method == 'POST':
        old_password = request.POST.get('old_password', '')
        new_password = request.POST.get('new_password', '')
        repeat_password = request.POST.get('repeat_password', '')
        if user.check_password(old_password):
            if not new_password:
                state = 'empty'
            elif new_password != repeat_password:
                state = 'repeat_error'
            else:
                user.set_password(new_password)
                user.save()
                return redirect("/log_in/")
        else:
            state = 'password_error'
    content = {
        'user': user,
        'state': state,
    }
    return render(request, 'set_password.html', content)

```
