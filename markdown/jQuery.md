## data方法 ##
$(选择器).data（"key","value"） value可以是jquery对象，数组，等    给 点前面的 jquery对象存储一个值

$(选择器).removedata（"key"）   移除对应key的值
## jQuery补充 ##

第一种情况：文档树加载完之后绑定事件（绝大部分情况下）

第二种方法：
```jquery
$(document).ready(function(){
	绑定事件的代码...
});

简写：
$(function($){
	绑定事件的代码...
});
```

## jQuery扩展 ##

扩展:就是把一些方法，事件，属性等自定义封装到一个js文件里，然后想用这个方法导入文件即可.

```jquery
定义规范：
给jquery添加扩展:
$.extend({
	"xxx":function(){
			要执行的代码
	}
})；

调用：$.xxx()

给jQuery对象添加扩展:
$.fn.extend({
	"xxx":function(){
			要执行的代码
	}
})

调用：$(选择器).xxx()
```
