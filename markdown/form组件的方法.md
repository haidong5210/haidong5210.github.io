#form组件

```python
from django.forms import Form   
from django.forms import fields   
from django.forms import widgets   #用于渲染的样式！


#第一种
class Login(Form):
	name = fields.CharField()	
	xxxxxxxxxxxxxxxxx

#第二种
#下面的字典可以自己生成
Login=type('Login',(Form,),
{"name" : fields.CharField(),
xxxxxxxxxxxxxxxxxxxx})

```