# js对象 #

----------
## string对象 ##
- 字符串创建(两种形式)
	- 1，变量="字符串"
		- var str1="hello"
	- 2，变量=new String(字符串)
		- var str2=new String("hello")
- 字符串对象的属性和函数
	- x.length         --------获取字符串的长度
	- x.toLowerCase()  --------把字符串转为小写
	- x.toUpperCase()  --------把字符串转为大写
	- x.trim()         --------去除字符串两边的空格


- 字符串查询方法
	- x.charAt(index)    -------获取索引处的字符
	- x.indexOf(findstr,index)  ---------查询字符串索引，index表示开始的索引
	- x.lastIndexOf(findstr,fromindex) -------默认从字符串后面开始查找索引，formindex指定查找的开始位置
	- x.match(regexp)   -----根据正则表达式找到所匹配的字符，返回值是数组
	- x.search(regexp)  ------search返回匹配字符串的首字符位置索引
- 字符串的处理方法
	- x.substr(start,length) -----start表示开始索引，length表示截取长度
	- x.substring(start,end) -----start表示开始索引,end表示结束位置
	- x.slice(start,end)   -------切片操作字符串与python原则一样
	- x.replace(findstr,tostr)  -----替换字符
	- x.concat(addstr)    -----拼接字符串
## Aarry对象 ##
- 数组创建（三种方式）
	- 1，var arrname=[元素0，元素1.....];
	- 2,var arrname=new Array[元素0，元素1.....]
	- 3,var arrname = new Array(长度); 
- 数组对象的属性和方法
	- x.join(bystr)   -----将数组元素拼接成字符串（与python的的字符串join方法一样）
	- x.concat(value...) -----value数组的添加元素
	- x.reverse()  -----颠倒数组的数据
	- x.sort()  ------排序数组的元素 一位一位排（111,12,32,444）
	- x.sort(function)  ----function函数，定义个函数，按照函数的内容来排序
		- function InSort(a,b){return a-b;}"按照大小降序排序"
	- x.slice(start,end)  -----数组的切片
	- x.splice(start,deleteCount,value..)  -----splice的主要用途是对数组指定位置进行删除和插入,deleteCount删除数组元素的个数,value表示在删除位置插入的数组元素(value参数可以省略 )
	- x.push(value....)    -----添加值
	- x.pop()    -------取值（去最后一个）
	- x.unshift(value....) -----value可以为字符串、数字、数组等任何值,unshift是将value值插入到数组x的开始
	- x.shift() -------shift是将数组x的第一个元素删除
- 总结js数组的特性
	- 1，js中的数组特性1: js中的数组可以装任意类型,没有任何限制.
	- 2，js中的数组,长度是随着下标变化的.用到多长就有多长
##Date对象  ##
- 创建Date对象（4种方法）
	- var nowd1=new Date();
	- var nowd2=new Date("2004/3/20 11:12");参数为日期字符串
	- var nowd3=new Date(5000); 参数单位为毫秒数
	- var nowd4=new Date(2004,2,20,11,12,0,300);参数为年月日小时分钟秒毫秒
- Date对象的方法-获取时间日期
	- getDate() ------获取日
	- getDay () ------ 获取星期
	- getMonth ()  -----获取月（0-11）
	- getFullYear () -----获取完整年份
	- getYear ()  ----- 获取年
	- getHours ()  -----获取小时
	- getMinutes ()  ---- 获取分钟
	- getSeconds () ------获取秒
	- getMilliseconds () ----- 获取毫秒
	- getTime ()    -------返回累计毫秒数(从1970/1/1午夜)
- Date对象的方法-设置日期和时间
	- setDate(day_of_month)  -----设置日
	- setMonth (month  ------设置月
	- setFullYear (year)  ---- 设置年
	- setHours (hour) -----设置小时
	- setMinutes (minute) ----设置分钟
	- setSeconds (second)  ----设置秒
	- setMillliseconds (ms) -----设置毫秒(0-999)
	- setTime (allms)  -----设置累计毫秒(从1970/1/1午夜)
- Date对象的方法-日期和时间的转换
	- getTimezoneOffset():8个时区×15度×4分/度=480;
返回本地时间与GMT的时间差，以分钟为单位
toUTCString()
返回国际标准时间字符串
toLocalString()
返回本地格式时间字符串
Date.parse(x)
返回累计毫秒数(从1970/1/1午夜到本地时间)
Date.UTC(x)
返回累计毫秒数(从1970/1/1午夜到国际时间)
## Math对象 ##
-  Math对象属性方法
	-  Math.abs(x)    返回数的绝对值。
	-  exp(x)    返回 e 的指数。
	-  floor(x)对数进行下舍入。
	-  log(x)    返回数的自然对数（底为e）。
	-  max(x,y)    返回 x 和 y 中的最高值。
	-  min(x,y)    返回 x 和 y 中的最低值。
	-  pow(x,y)    返回 x 的 y 次幂。
	-  random()    返回 0 ~ 1 之间的随机数。
	-  round(x)    把数四舍五入为最接近的整数。
	-  sin(x)    返回数的正弦。
	-  sqrt(x)    返回数的平方根
	-  tan(x)    返回角的正切。
## Function对象 ##
- 函数的定义
	- function 函数名 (参数){ <br>    函数体;
    return 返回值;
}
	- 功能说明
		- 可以使用变量、常量或表达式作为函数调用的参数
	函数由关键字function定义
	函数名的定义规则与标识符一致，大小写是敏感的
	返回值必须使用return
Function 类可以表示开发者定义的任何函数
-  Function对象的属性
	-  ECMAScript 定义的属性 length 声明了函数期望的参数个数。
		-  alert(func1.length)
-  Function的调用
	-  function func1(a,b){
    alert(a+b);}
	- func1(1,2)
-  函数的内置对象arguments
	-  arguments 类似于python中函数的*args
-  匿名函数
	-  var func = function(arg){return "tony";}
-  匿名函数应用
	-  (function(){alert("tony");} )()
## Bom对象 ##
- windows对象
	- 所有浏览器都支持 window 对象。概念上讲.一个html文档对应一个window对象.功能上讲: 控制浏览器窗口的.使用上讲: window对象不需要创建对象,直接使用即可.
- windows对象方法
	- alert()    ------显示带有一段消息和一个确认按钮的警告框。
	- confirm()  ------显示带有一段消息以及确认按钮和取消按钮的对话框。
	- prompt()   ------显示可提示用户输入的对话框。
	
	- open()     ------打开一个新的浏览器窗口或查找一个已命名的窗口。
	- close()    ------关闭浏览器窗口。
	- setInterval()  -------按照指定的周期（以毫秒计）来调用函数或计算表达式
	- clearInterval()  ------取消由 setInterval() 设置的 timeout。
	- setTimeout()    ------ 在指定的毫秒数后调用函数或计算表达式。只执行一次
	
	- clearTimeout()  ------取消由 setTimeout() 方法设置的 timeout。
	- scrollTo()    ------ 把内容滚动到指定的坐标。
## DOM对象 ##
- 什么是HEML DOM
	- HTML Document Object Model（文档对象模型）
	- HTML DOM 定义了访问和操作HTML文档的标准方法
	- HTML DOM 把 HTML 文档呈现为带有元素、属性和文本的树结构（节点树)
	- ![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170524162619154-241270145.png)
	- ![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170524163054950-914162209.png)
	- 画DOM树为了展示文档中各个对象之间的关系，用于对象的导航。
- DOM节点
	- HTML 文档中的每个成分都是一个节点。
	- DOM 是这样规定的：
		- 1，整个文档是一个文档节点
		- 2， 每个 HTML 标签是一个元素节点 
		- 3，包含在 HTML 元素中的文本是文本节点 
		- 4， 每一个 HTML 属性是一个属性节点
		- ![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170524181750638-251706028.png)
		- 其中document和element节点是重点
- 节点关系
	- 节点树中的节点彼此拥有层级关系。
	- 父(parent),子(child)和同胞(sibling)等术语用于描述这些关系。父节点拥有子节点。同级的子节点被称为同胞（兄弟或姐妹）。
		- 在节点树中，顶端节点被称为根（root）
		- 每个节点都有父节点、除了根（它没有父节点）
		-  一个节点可拥有任意数量的子
		-    同胞是拥有相同父节点的节点
	-    下面的图片展示了节点树的一部分，以及节点之间的关系：
		- ![](http://images2015.cnblogs.com/blog/877318/201611/877318-20161109150149420-1767730470.png)   
		- 访问html元素，访问html元素等同于访问节点，我们能以不同的方式访问html元素。
- 节点查找
	- 直接查找节点
		- 1，document.getElementById(“idname”)  ------查找依赖idname，返回节点对象
		- 2，document.getElementsTagName（“tagname”） ----依居于标签名，返回shuzu
		- 3，document.getElementsByName(“name”) ----依据与属性name，返回数组
		- 4，document.getElementsByClassName(“name”) -----依据于classname查找，返回数组
	- 导航查找节点
		- 1，parentElement           // 父节点标签元素
		- 2，children                // 所有子标签
		- 3，firstElementChild       // 第一个子标签元素
		- 4，lastElementChild        // 最后一个子标签元素
		- 5，nextElementtSibling     // 下一个兄弟标签元素
		- 6，previousElementSibling  // 上一个兄弟标签元素
- 节点操作
	- 1、获取文本节点的值：
		- innerText  只能是文本操作，查找或添加
		- innerHTML   可以操作标签，查找或添加
	- 2，attribute操作
		-  elementNode.setAttribute(name,value)
		-   elementNode.setAttribute(name,value)
		-  elementNode.属性名(DHTML)
	- 3， value获取当前选中的value值
		-  1.input  
		-  2.select （selectedIndex）
		-  3.textarea  
	-  4，innerHTML 给节点添加html代码：
	-  5，关于class的操作：
		-  elementNode.className
		-  elementNode.classList.add
		-  elementNode.classList.remove
	- 6，改变css样式：
		-  <p id="p2">Hello world!</p>
		-  document.getElementById("p2").style.color="blue";
## DOM Event事件 ##
- 事件类型
	- onclick        当用户点击某个对象时调用的事件句柄。
	- ondblclick     当用户双击某个对象时调用的事件句柄。
	- onfocus        元素获得焦点
	- onchange       域的内容被改变
	- onkeydown      某个键盘按键被按下。
	- onkeypress     某个键盘按键被按下并松开。
	- onkeyup        某个键盘按键被松开。
	- onload         一张页面或一幅图像完成加载。
	- onmousedown    鼠标按钮被按下。
	- onmousemove    鼠标被移动
	- onmouseout     鼠标从某元素移开。
	- onmouseover    鼠标移到某元素之上。
	- onmouseleave   鼠标从元素离开
	- onselect       文本被选中。
	- onsubmit       确认按钮被点击。
- 绑定事件方式
	- 1，在标签中直接绑定on事件=“函数function”
	- 2，ele.on事件=function(){}