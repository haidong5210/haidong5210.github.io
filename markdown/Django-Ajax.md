# Ajax准备知识：json #
定义：
```python
JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式。
它基于 ECMAScript (w3c制定的js规范)的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。
简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。
```
讲json对象，不得不提到JS对象：
![](http://images2017.cnblogs.com/blog/877318/201708/877318-20170825211200714-1635996938.png)
合格的json对象：
```python
["one", "two", "three"]

{ "one": 1, "two": 2, "three": 3 }

{"names": ["张三", "李四"] }

[ { "name": "张三"}, {"name": "李四"} ]
```
不合格的json对象：
```python
{ name: "张三", 'age': 32 }                     // 属性名必须使用双引号

[32, 64, 128, 0xFFF] // 不能使用十六进制值

{ "name": "张三", "age": undefined }            // 不能使用undefined

{ "name": "张三",
  "birthday": new Date('Fri, 26 Aug 2011 07:13:10 GMT'),
  "getName":  function() {return this.name;}    // 不能使用函数和日期对象
}
```
**stringify与parse方法**
```python
JSON.parse():     用于将一个 JSON 字符串转换为 JavaScript 对象　
eg:
console.log(JSON.parse('{"name":"Yuan"}'));
console.log(JSON.parse('{name:"Yuan"}')) ;   // 错误
console.log(JSON.parse('[12,undefined]')) ;   // 错误



JSON.stringify(): 用于将 JavaScript 值转换为 JSON 字符串。　
eg:  console.log(JSON.stringify({'name':"egon"})) ;
```
# Ajax简介 #
AJAX（Asynchronous Javascript And XML）翻译成中文就是“异步Javascript和XML”。即使用Javascript语言与服务器进行异步交互，传输的数据为XML（当然，传输的数据不只是XML）。

- 同步交互：客户端发出一个请求后，需要等待服务器响应结束后，才能发出第二个请求；
- 异步交互：客户端发出一个请求后，无需等待服务器响应结束，就可以发出第二个请求。
AJAX除了异步的特点外，还有一个就是：浏览器页面局部刷新；（这一特点给用户的感受是在不知不觉中完成请求和响应过程）

js实现的局部刷新:
```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        .error{
            color:red
        }
    </style>
</head>
<body>


<form class="Form">

    <p>姓名&nbsp;&nbsp;<input class="v1" type="text" name="username" mark="用户名"></p>
    <p>密码&nbsp;&nbsp;<input class="v1" type="text" name="email" mark="邮箱"></p>
    <p><input type="submit" value="submit"></p>

</form>

<script src="jquery-3.1.1.js"></script>

<script>

    $(".Form :submit").click(function(){

        flag=true;

        $("Form .v1").each(function(){

            var value=$(this).val();
            if (value.trim().length==0){
                 var mark=$(this).attr("mark");
                 var $span=$("<span>");
                 $span.html(mark+"不能为空!");
                 $span.prop("class","error");
                 $(this).after($span);

                 setTimeout(function(){
                      $span.remove();
                 },800);

                 flag=false;
                 return flag;

            }
        });
        return flag
    });


</script>


</body>
</html>
```
**AJAX常见应用情景**
当我们在百度中输入一个“老”字后，会马上出现一个下拉列表！列表中显示的是包含“传”字的4个关键字。

其实这里就使用了AJAX技术！当文件框发生了输入变化时，浏览器会使用AJAX技术向服务器发送一个请求，查询包含“传”字的前10个关键字，然后服务器会把查询到的结果响应给浏览器，最后浏览器把这4个关键字显示在下拉列表中。

- 整个过程中页面没有刷新，只是刷新页面中的局部位置而已！
- 当请求发出后，浏览器还可以进行其他操作，无需等待服务器的响应！
![](http://images2015.cnblogs.com/blog/877318/201610/877318-20161025165534625-1155566124.png)
当输入用户名后，把光标移动到其他表单项上时，浏览器会使用AJAX技术向服务器发出请求，服务器会查询名为zhangSan的用户是否存在，最终服务器返回true表示名为lemontree7777777的用户已经存在了，浏览器在得到结果后显示“用户名已被注册！”。

- 整个过程中页面没有刷新，只是局部刷新了；
- 在请求发出后，浏览器不用等待服务器响应结果就可以进行其他操作；

**AJAX的优缺点**
优点：

AJAX使用Javascript技术向服务器发送异步请求；
AJAX无须刷新整个页面；
因为服务器响应内容不再是整个页面，而是页面中的局部，所以AJAX性能高；
# jquery实现的ajax #
```python
$.ajax({
	url:"/handle_Ajax/",
	type:"POST",
	data:{username:"Yuan",password:123},
	success:function(data){
                  alert(data)
               },


	error: function (jqXHR, textStatus, err) {

                        // jqXHR: jQuery增强的xhr
                        // textStatus: 请求完成状态
                        // err: 底层通过throw抛出的异常对象，值与错误类型有关
                        console.log(arguments);
                    },
	complete: function (jqXHR, textStatus) {
                    // jqXHR: jQuery增强的xhr
                    // textStatus: 请求完成状态 success | error
                    console.log('statusCode: %d, statusText: %s', jqXHR.status, jqXHR.statusText);

})
```
**$.ajax参数**
*请求参数*
```python

data: 当前ajax请求要携带的数据，是一个json的object对象，ajax方法就会默认地把它编码成某种格式
             (urlencoded:?a=1&b=2)发送给服务端；此外，ajax默认以get方式发送请求。

             function testData() {
               $.ajax("/test",{     //此时的data是一个json形式的对象
                  data:{
                    a:1,
                    b:2
                  }
               });                   //?a=1&b=2

(formdata的时候用到 processData=flase)
processData：声明当前的data数据是否进行转码或预处理，默认为true，即预处理；if为false，
             那么对data：{a:1,b:2}会调用json对象的toString()方法，即{a:1,b:2}.toString()
             ,最后得到一个［object，Object］形式的结果。

(formdata的时候用到 contentType=flase)
contentType：默认值: "application/x-www-form-urlencoded"。发送信息至服务器时内容编码类型。
             用来指明当前请求的数据编码格式；urlencoded:?a=1&b=2；如果想以其他方式提交数据，
             比如contentType:"application/json"，即向服务器发送一个json字符串：
               $.ajax("/ajax_get",{
             
                  data:JSON.stringify({
                       a:22,
                       b:33
                   }),
                   contentType:"application/json",
                   type:"POST",
             
               });                          //{a: 22, b: 33}
注意：contentType:"application/json"一旦设定，data必须是json字符串，不能是json对象
 

traditional：一般是我们的data数据有数组时会用到 ：data:{a:22,b:33,c:["x","y"]},
              traditional为false会对数据进行深层次迭代；            
```
*响应参数*
```python
/*

dataType：  预期服务器返回的数据类型,服务器端返回的数据会根据这个值解析后，传递给回调函数。
            默认不需要显性指定这个属性，ajax会根据服务器返回的content Type来进行转换；
            比如我们的服务器响应的content Type为json格式，这时ajax方法就会对响应的内容
            进行一个json格式的转换，if转换成功，我们在success的回调函数里就会得到一个json格式
            的对象；转换失败就会触发error这个回调函数。如果我们明确地指定目标类型，就可以使用
            data Type。
            dataType的可用值：html｜xml｜json｜text｜script
            见下dataType实例

*/
```
**csrf跨站请求伪造**
*方式一*
```python
$.ajaxSetup({
    data: {csrfmiddlewaretoken: '{{ csrf_token }}' },
});
```
*方式二*
```python
<form>
###{% csrf_token %}##
</form><br><br><br>$.ajax({<br>...<br>data:{
"csrfmiddlewaretoken":$("[name='csrfmiddlewaretoken']").val()
}<br>})
```
*方式三*

```python
<script src="{% static 'js/jquery.cookie.js' %}"></script>
$.ajax({
 
headers:{"X-CSRFToken":$.cookie('csrftoken')},
 
})
```
# jQuery.serialize() #
serialize()函数用于序列化一组表单元素，将表单内容编码为用于提交的字符串。

serialize()函数常用于将表单内容序列化，以便用于AJAX提交。

该函数主要根据用于提交的有效表单控件的name和value，将它们拼接为一个可直接用于表单提交的文本字符串，该字符串已经过标准的URL编码处理(字符集编码为UTF-8)。

该函数不会序列化不需要提交的表单控件，这和常规的表单提交行为是一致的。例如：不在<form>标签内的表单控件不会被提交、没有name属性的表单控件不会被提交、带有disabled属性的表单控件不会被提交、没有被选中的表单控件不会被提交。
```python
与常规表单提交不一样的是：常规表单一般会提交带有name的按钮控件，而serialize()函数不会序列化带有name的按钮控件。更多详情请点击这里。
```
*语法*
```python
jQueryObject.serialize()
$(filter).serialize()
```
serialize()函数的返回值为String类型，返回将表单元素编码后的可用于表单提交的文本字符串。

请参考下面这段初始HTML代码：
```python
<form name="myForm" action="http://www.365mini.com" method="post">
    <input name="uid" type="hidden" value="1" />
    <input name="username" type="text" value="张三" />
    <input name="password" type="text" value="123456" />
    <select name="grade" id="grade">
        <option value="1">一年级</option>
        <option value="2">二年级</option>
        <option value="3" selected="selected">三年级</option>
        <option value="4">四年级</option>
        <option value="5">五年级</option>
        <option value="6">六年级</option>
    </select>
    <input name="sex" type="radio" checked="checked" value="1" />男
    <input name="sex" type="radio" value="0" />女
    <input name="hobby" type="checkbox" checked="checked" value="1" />游泳
    <input name="hobby" type="checkbox" checked="checked" value="2" />跑步
    <input name="hobby" type="checkbox" value="3" />羽毛球
    <input name="btn" id="btn" type="button" value="点击" />
```
对<form>元素进行序列化可以直接序列化其内部的所有表单元素。

```python
// 序列化<form>内的所有表单元素
// 序列化后的结果：uid=1&username=%E5%BC%A0%E4%B8%89&password=123456&grade=3&sex=1&hobby=1&hobby=2
alert( $("form").serialize() );

我们也可以直接对部分表单元素进行序列化。

// 序列化所有的text、select、checkbox表单元素
// 序列化后的结果：username=%E5%BC%A0%E4%B8%89&password=123456&grade=3&hobby=1&hobby=2
alert( $(":text, select, :checkbox").serialize() );


data:$(":text, select, :checkbox").serialize()
```

# 上传文件 #
**form表单上传文件**
```python
<h3>form表单上传文件</h3>


<form action="/upload_file/" method="post" enctype="multipart/form-data">
    <p><input type="file" name="upload_file_form"></p>
    <input type="submit">
</form>
```
*view*
```python
def index(request):

    return render(request,"index.html")


def upload_file(request):
    print("FILES:",request.FILES)
    print("POST:",request.POST)
    return HttpResponse("上传成功!")
```
**Ajax(FormData)**
FormData是什么呢？

 

XMLHttpRequest Level 2添加了一个新的接口FormData.利用FormData对象,我们可以通过JavaScript用一些键值对来模拟一系列表单控件,我们还可以使用XMLHttpRequest的send()方法来异步的提交这个"表单".比起普通的ajax,使用FormData的最大优点就是我们可以异步上传一个二进制文件.
所有主流浏览器的较新版本都已经支持这个对象了，比如Chrome 7+、Firefox 4+、IE 10+、Opera 12+、Safari 5+
```python
<h3>Ajax上传文件</h3>

<p><input type="text" name="username" id="username" placeholder="username"></p>
<p><input type="file" name="upload_file_ajax" id="upload_file_ajax"></p>

<button id="upload_button">提交</button>
{#注意button标签不要用在form表单中使用#}

<script>
    $("#upload_button").click(function(){

        var username=$("#username").val();
        var upload_file=$("#upload_file_ajax")[0].files[0];

       ** var formData=new FormData();
        formData.append("username",username);
        formData.append("upload_file_ajax",upload_file);**


        $.ajax({
            url:"/upload_file/",
            type:"POST",
            data:formData,
            contentType:false,
            processData:false,

            success:function(){
                alert("上传成功!")
            }
        });


    })
</script>
```
*views*
```python
def index(request):
  
    return render(request,"index.html")
  
  
def upload_file(request):
    print("FILES:",request.FILES)
    print("POST:",request.POST)
    return HttpResponse("上传成功!")
```