# 前端基础html #
## html是什么 ##
- 超文本标记语言（Hypertext Markup Language，HTML）通过**标签语言**来标记要显示的网页中的各个部分。一套规则，浏览器认识的规则
- 浏览器按顺序渲染网页文件，然后根据标记符解释和显示内容。但需要注意的是，对于不同的浏览器，对同一标签可能会有不完全相同的解释（兼容性）
- 静态网页文件扩展名：.html 或 .htm 文件的后缀名是给人看的。
## HTML不是什么 ##
- HTML不是一种编程语言，而是一种标记语言。
- HTML使用标记标签来描述网页。![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170511162922019-657075356.png)
## HTML结构 ##
![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170511163803207-1491291563.png)
- <!DOCTYPE html> 告诉浏览器使用什么样的html或者xhtml来解析html文档
- <html></html>是文档的开始标记和结束标记。此元素告诉浏览器其自身是一个 HTML 文档，在它们之间是文档的头部<head>和主体<body>。
- <head></head>元素出现在文档的开头部分。<head>与</head>之间的内容不会在浏览器的文档窗口显示，但是其间的元素有特殊重要的意义。
- <title></title>定义网页标题，在浏览器标题栏显示。 
- <body></body>之间的文本是可见的网页主体内容
## HTML标签格式 ##
![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170511170514582-998625349.png)
- 标签的语法：
	- <标签名 属性1="">内容部分</标签名>  闭合
	- <标签名 属性1="" />               自闭和
## 常用标签 ##

```html
<!DOCTYPE>标签
	 <!DOCTYPE> 声明位于文档中的最前面的位置，处于 <html> 标签之前。此               标签可告知浏览器文档使用哪种 HTML 或 XHTML 规范。
	 作用：声明文档的解析类型(document.compatMode)，避免浏览器的怪异模式。

	document.compatMode：
		1，BackCompat：怪异模式，浏览器使用自己的怪异模式解析渲染页面。
		2，CSS1Compat：标准模式，浏览器使用W3C的标准解析渲染页面。
	这个属性会被浏览器识别并使用，但是如果你的页面没有DOCTYPE的声明，那么compatMode默认就是BackCompat

<head>内常用标签
	<meta>标签
		meta介绍	:
			<meta>元素可提供有关页面的元信息（meta-information），针对搜索引擎和更新频度的描述和关键词。

			<meta>标签位于文档的头部，不包含任何内容。
			<meta>提供的信息是用户不可见的

			meta标签的组成：meta标签共有两个属性，它们分别是http-equiv属性和name 属性，不同的属性又有不同的参数值，这些不同的参数值就实现了不同的网页功能。

			(1)name属性: 主要用于描述网页，与之对应的属性值为content，content中的内容主要是便于搜索引擎机器人查找信息和分类信息用的。    
			<meta name="keywords" content="meta总结,html meta,meta属性,meta跳转">
			<meta name="description" content="老男孩培训机构是由一个很老的男孩创建的">

			(2)http-equiv属性：相当于http的文件头作用，它可以向浏览器传回一些有用的信息，以帮助正确地显示网页内容，与之对应的属性值为content，content中的内容其实就是各个参数的变量值。
			
			<meta http-equiv="Refresh" content="2;URL=https://www.oldboy.com"> //(注意后面的引号，分别在秒数的前面和网址的后面)
 
			<meta http-equiv="content-Type" charset=UTF8">
 
			<meta http-equiv = "X-UA-Compatible" content = "IE=EmulateIE7" /> 
非meta标签
	<title>oldboy</title>
    <link rel="icon" href="http://www.jd.com/favicon.ico">
   	<link rel="stylesheet" href="css.css">
    <script src="hello.js"></script>　
```

```html
<body>内常用标签
	
	基本标签（块级标签和内联标签）
		*<hn>: n的取值范围是1~6; 从大到小. 用来表示标题.
		*<p>: 段落标签. 包裹的内容被换行.并且也上下内容之间有一行空白.
		<b> <strong>: 加粗标签.
		<strike>: 为文字加上一条中线.
		<em>: 文字变成斜体.
		<sup>和<sub>: 上角标 和 下角标.
		*<br>:换行.
		<hr>:水平线
```

```html
****<div>和<span>
	<div></div> ： <div>只是一个块级元素，并无实际的意义。主要通过CSS样式为其赋予不同的表现. 

	<span></span>： <span>表示了内联行(行内元素),并无实际的意义,主要通过CSS样式为其赋予不同的表现.

****块级元素与行内元素的区别
	所谓块元素，是以另起一行开始渲染的元素，行内元素则不需另起一行。如果单独在网页中插入这两个元素，不会对页面产生任何的影响。
	这两个元素是专门为定义CSS样式而生的。
```
![](http://images2015.cnblogs.com/blog/877318/201705/877318-20170511175516832-1225680479.png)

```html
图形标签: <img> (内联标签)
	属性：
		src: 要显示图片的路径.
		alt: 图片没有加载成功时的提示.
		title: 鼠标悬浮时的提示信息.
		width: 图片的宽
		height:图片的高 (宽高两个属性只用一个会自动等比缩放.)
```

```html
超链接标签(锚标签): <a> </a> (内联标签)
	所谓的超链接是指从一个网页指向一个目标的连接关系，这个目标可以是另一个网页，也可以是相同网页上的不同位置，还可以是一个图片，一个电子邮件地址，一个文件，甚至是一个应用程序

什么是url？
	URL是统一资源定位器(Uniform Resource Locator)的缩写，也被称为网页地址，是因特网上标准的资源的地址。

url举例：
	http://www.sohu.com/stu/intro.html
	http://222.172.123.33/stu/intro.html

url地址由4部分组成
	第一部分：为协议：http协议，ftp协议等
	第二部分：为站点地址：可以使域名或IP地址
	第三部分：为页面站点的目录
	第四部分：为页面的名称
	各部分用“/”隔开  

a标签：
	<a href="" target="_blank" >click</a>

	href属性指定目标网页地址。该地址可以有几种类型：

    	绝对 URL - 指向另一个站点（比如 href="http://www.jd.com）
    	相对 URL - 指当前站点中确切的路径（href="index.htm"）
    	**锚 URL - 指向页面中的锚（href="#id"）
```

```html
**<ul>: 无序列表 
	[type属性：disc(实心圆点)(默认)、circle(空心圆圈)、square(实心方块)]

<ol>: 有序列表
	<li>:列表中的每一项.

<dl>  自定义列表
	<dt> 列表标题
    <dd> 列表项
```

```html
**表格标签：<table>
	表格概念
		表格是一个二维数据空间，一个表格由若干行组成，一个行又有若干单元格组成，单元格里可以包含文字、列表、图案、表单、数字符号、预置文本和其它的表格等内容。

		表格最重要的目的是显示表格类数据。表格类数据是指最适合组织为表格格式（即按行和列组织）的数据

**表格的基本结构：
	<table>
		<tr>           //一行
			<td>标题</td>     //一列
			<td>标题</td>
		</tr>	
		<tr>
            <td>内容</td>
            <td>内容</td>
        </tr>
	</table>


<tr>: table row

<th>: table head cell

<td>: table data cell
属性：
	border: 表格边框.
	cellpadding: 内边距
	cellspacing: 外边距.
	width: 像素 百分比.（最好通过css来设置长宽）
	rowspan:  单元格竖跨多少行
	colspan:  单元格横跨多少列（即合并单元格）
```	

```html
**表单标签: <form>
	功能：表单用于向服务器传输数据，从而实现用户与服务器的交互 
	
	表单能够包含input系列标签，比如文本字段，复选框，单选框，提交按钮等

	表单还可以包含textare、select、fieldset和label

表单属性
	action：表单提交到哪.一般指向服务器端一个程序,程序接收到表单提交过来的数据（即表单元素值）作相应处理，比如https://www.sogou.com/web
	
	method: 表单的提交方式  默认取值就是get
	post  请求会放在请求体里
	get   请求会放在url后面 

表单元素：
基本概念：
	HTML表单是HTML元素中较为复杂的部分，表单往往和脚本、动态页面、数据处理等功能相结合，因此它是制作动态网站很重要的内容。表单一般用来收集用户的输入信息

表单工作原理：
	访问者在浏览有表单的网页时，可填写必需的信息，然后按某个按钮提交。这些信息通过Internet传送到服务器上。 
	
	服务器上专门的程序对这些数据进行处理，如果有错误会返回错误信息，并要求纠正错误。当数据完整无误后，服务器反馈一个输入完成的信息

<input>系列标签
	
<1> 表单类型

type：
	text 文本输入框
	password 密码输入框
	radio 单选框
	checkbox 多选框  
	submit 提交按钮 
	button 按钮(需要配合js使用.) button和submit的区别？
	
	file 提交文件
	上传文件注意两点：
        1 请求方式必须是post
        2 enctype="multipart/form-data"

<2> 表单属性
name： 表单提交项的键
		注意和id属性的区别：name属性是和服务器通信时使用的名称；而id属性是浏览器端使用的名称，该属性主要是为了方便客户端编程，是在css和Javascript中使用的!

value: 表单提交项的值，对于不同的输入类型，value属性的用法不同

checked：radio和checkbox默认是选中的 checked="checked"

readonly:只读，text和password

disabled：不可点击的，对所用的input都好使

select标签：
	<select>下拉选标签属性
		name：表单提交项的键
		size:选项的个数
		multiple：multiple  多选
			
select内部的option属性：
	value：提交的值
	selected=selected  此项默认选中	

<textarea> 多行文本框
	<form id="form1" name="form1" method="post" action="">
        <textarea cols=“宽度” rows=“高度” name=“名称”>
                   默认内容
        </textarea>
</form>


<label>标签
	定义：<label> 标签为 input 元素定义标注（标记）。
	说明：
		1 label 元素不会向用户呈现任何特殊效果。
		2 <label> 标签的 for 属性值应当与相关元素的 id 属性值相同。
<label>标签实例：
	<form method="post" action="">
        <label for=“username”>用户名</label>
        <input type=“text” name=“username” id=“username” size=“20” />
</form>
```
