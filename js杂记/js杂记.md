#### 1、json对象和字符串互相转换

```javascript
一、JSON字符串转换为JSON对象： eval() 和 JSON.parse

eg- json字符串:   var data = '{ "name": "dran", "sex": "man" }';

    var obj = eval("("+data+")");  或者
    var obj = JSON.parse(data)；
  /* 提示：为什么要 eval这里要添加 ("("+data+")");呢？ 
　　原因在于：eval本身的问题。 由于json是以”{}”的方式来开始以及结束的，在JS中，它会被当成一个语句块来处理，所以必须强制性的将它转换成一种表达式。 
　　加上圆括号的目的是迫使eval函数在处理JavaScript代码的时候强制将括号内的表达式（expression）转化为对象，而不是作为语句（statement）来执行。举一个例子，例如对象字面量{}，如若不加外层的括号，那么eval会将大括号识别为JavaScript代码块的开始和结束标记，那么{}将会被认为是执行了一句空语句*/ 
    
```

```javascript
二、JSON对象转换为JSON字符串 ：  obj.toJSONString()或者全局方法JSON.stringify(obj)   (obj代表json对象)

 eg-json对象:   var obj = { "name": "dran", "sex": "man" };
  var jstring = JSON.stringify(obj) ;// 建议用这个
  var jstring = obj.toJSONString();  //toJSONString()不是js原生的方法，需要引入相应的库或自己定义后才能用   (不习惯用)
```

#### 2、es6 中find()  和 findIndex() 方法

1.find() find()方法用于找出第一个符合条件的数组成员，他的参数是一个回调函数，所有数组成员一次执行这个回调函数，知道找出第一个返回值为true的成员，然后返回该成员，如果没有符合条件的成员，就返回undefined 

```javascript
[1,5,15,20,25].find((value,index,arr) => {
    return value > 20;
}) //25
```

2.findIndex() findIndex()的方法与find()类似，返回第一个符合条件的数组成员的位置，如果所有的成员都不符合条件，就返回-1 

```javascript
[5,10,15,20].findIndex((value,index,arr) => {
    return value > 10
})   //2
```

#### 3、 jQery ajax    -serialize()方法

serialize() 方法通过序列化表单值，创建 URL 编码文本字符串 。

```javascript
<html>
<head>
<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("button").click(function(){
    $("div").text($("form").serialize());
  });
});
</script>
</head>
<body>
<form action="">
First name: <input type="text" name="FirstName" value="Bill" /><br />
Last name: <input type="text" name="LastName" value="Gates" /><br />
</form>

<button>序列化表单值</button>
<div></div>
</body>
</html>

```

**注释: **   只会将”成功的控件“序列化为字符串。如果不使用按钮来提交表单，则不对提交按钮的值序列化。如果要表单元素的值包含到序列字符串中，元素必须使用 name 属性。 

### 4.window.open  && window.showModalDialog

##### 4.1window.open

window.open 可实现以对话框形式弹出画面，并且关闭画面时刷新父页面

```javascript
//刷新父页面
window.opener.location.href=window.opener.location.href
//关闭窗口
window.opener = null
window.close()
```

基本语法：

```javascript
window.open(pageURL,name,parameters)
****其中： 
	pageURL 为子窗口路径 
　  name 为子窗口句柄 
　　 parameters 为窗口参数(各参数用逗号分隔)
```

各项参数：

| 参数          | 取值范围    | 说明                             |
| ------------- | ----------- | -------------------------------- |
| alwaysLowered | yes/no      | 指定窗口隐藏在所有窗口之后       |
| alwaysRaised  | yes/no      | 指定窗口悬浮在所有窗口之上       |
| depended      | yes/no      | 是否和父窗口同时关闭             |
| directories   | yes/no      | Nav2和3的目录栏是否可见          |
| height        | pixel value | 窗口高度                         |
| hotkeys       | yes/no      | 在没菜单栏的窗口中设安全退出热键 |
| innerHeight   | pixel value | 窗口中文档的像素高度             |
| innerWidth    | pixel value | 窗口中文档的像素宽度             |
| location      | yes/no      | 位置栏是否可见                   |
| menubar       | yes/no      | 菜单栏是否可见                   |
| outerHeight   | pixel value | 设定窗口(包括装饰边框)的像素高度 |
| outerWidth    | pixel value | 设定窗口(包括装饰边框)的像素宽度 |

原窗口中获取弹出窗口对象

```javascript
//infoWindow即代表了弹出窗口的window对象
var infoWindow=window.open ( “create.jsp” , “_blank” , “width=500,height=400″ )
```

弹出窗口中获取原窗口对象

```javascript
//我们可以如下操作来刷新原窗口
window.opener.location.reload () ;
//opener是DOM中提供的一个默认对象，表示的就是某个窗口的原窗口。
```

```javascript
父页面：

window.open('url', '', 'resizable=1, menuBar=0, toolBar=0, scrollbars=yes, Status=yes, resizable=1');  

子界面：

//获取父页面值

opener.document.getElementById("xx").value="newValue";  

//判断是否有父窗口,即打开本页面的窗口  

if(window.opener){

    window.opener.location.reload();//刷新父窗口  

　 window.opener.close();  //关闭父窗口  

}  
```

##### 4.2window.showModalDialog

**基本介绍：**

showModalDialog() (IE 4+ 支持)
showModelessDialog() (IE 5+ 支持)
window.showModalDialog()方法用来创建一个显示HTML内容的模态对话框。
window.showModelessDialog()方法用来创建一个显示HTML内容的非模态对话框。

**基本语法：**
​          vReturnValue = window.showModalDialog(sURL [ vArguments],[sFeatures])
​          vReturnValue = window.showModelessDialog(sURL [ vArguments],[sFeatures])
参数说明：
​         sURL          --  必选参数，类型：字符串。用来指定对话框要显示的文档的URL。
​         vArguments    -- 可选参数，类型：变体。用来向对话框传递参数。传递的参数类型不限，包括数组等。对话框通过 

​          window.dialogArguments来取得传递进来的参数。
​         sFeatures     -- 可选参数，类型：字符串。用来描述对话框的外观等信息，可以使用以下的一个或几个，用分号“;”隔开。

示例 :

window.showModalDialog('page.html', '','dialogHeight=30;dialogWidth=60;dialogTop=30;dialogLeft=465;center=yes;resizable:no;scroll:no;status:no;help=no')

参数：

| 参数                                                  | 取值范围                         | 说明                                                         |
| ----------------------------------------------------- | -------------------------------- | ------------------------------------------------------------ |
| center                                                | yes \| no \| 1 \| 0              | 窗口是否居中，默认yes，但仍可以指定高度和宽度                |
| resizable                                             | yes \| no \| 1 \| 0              | 是否可被改变大小。默认no。                                   |
| status                                                | yes \| no \| 1 \| 0              | 是否显示状态栏。默认为yes[ Modeless]或no[Modal]。            |
| scroll                                                | yes \| no \| 1 \| 0              | 指明对话框是否显示滚动条。默认为yes。                        |
| help                                                  | yes \| no \| 1 \| 0              | 是否显示帮助按钮，默认yes。                                  |
| dialogHeight                                          |                                  | 对话框高度，不小于１００px，ＩＥ４中dialogHeight 和 dialogWidth 默认的单位是em，而ＩＥ５中是px，为方便其见，在定义modal方式的对话框时，用px做单位。 |
| dialogWidth                                           |                                  | 对话框宽度。                                                 |
| dialogLeft                                            |                                  | 离屏幕左的距离。                                             |
| dialogTop                                             |                                  | 离屏幕上的距离。                                             |
| 下面几个属性是用在HTA中的，在一般的网页中一般不使用。 |                                  |                                                              |
| dialogHide                                            | yes \| no \| 1 \| 0 \| on \| off | 在打印或者打印预览时对话框是否隐藏。默认为no。               |
| edge                                                  | sunken \| raised                 | 指明对话框的边框样式。默认为raised。                         |
| unadorned                                             |                                  | 默认为no。                                                   |

**参数传递：**

1.要想对话框传递参数，是通过vArguments来进行传递的。类型不限制，对于字符串类型，最大为4096个字符。也可以传递对象

```javascript
parent.htm：

<script>
var obj = new Object();
obj.name="51js";
window.showModalDialog("modal.htm",obj,"dialogWidth=200px;dialogHeight=100px");
</script>
modal.htm：

<script>
var obj = window.dialogArguments
alert("您传递的参数为：" + obj.name)
</script>
```

2.可以通过window.returnValue向打开对话框的窗口返回信息，当然也可以是对象

```javascript
parent.htm

<script>
str =window.showModalDialog("modal.htm",,"dialogWidth=200px;dialogHeight=100px");
alert(str);
</script>
modal.htm

<script>
window.returnValue="http://...";
</script> 
```

#### 5、客户端异步提交（ajax），服务端重定向无效

客户端需要写 `window.location.href='/'`

服务端写`res.redircet('./')`无效

#### 6、表单提交密码是‘’小眼睛切换显示密码‘’

```javascript
//点击小眼睛实现显示密码
$('.icon_psd').click(function(){	
	$(this).addClass('text')
	var a = $('#password').val()
	$('#password').remove()
	
	$('.dl_form_infos').children('.one').eq(1).append(`<input id="password" placeholder="密码" name="password" value='' class="dl_text" type="text">`)
	$('#password').val(a)
})

//点击小眼睛实现不显示密码

```

