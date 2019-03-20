

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

#### 7、操作数组的方法集锦

**7.1  ES5**

**删除数组**

1、**delecte**方法，delete删除的只是数组元素的值，所占的空间是并没有删除的 

```javascript
var arr=[12,23,44,5,6,23,43,34];
delete arr[1];
console.log(arr)
//输出结果是：[12, empty, 44, 5, 6, 23, 43, 34]
```

2、**splice**方法，该方法删除数组指定的元素，并且可以给数组添加新的元素，即实现**删除/替换**数组的某项元素 

arr.splice(index,length,items,items,...);

注意：如果不添加item的时候，就是数组的删除，会改变原数组的长度

```javascript
var arr=[12,23,44,5,6,23,43,34];
arr.splice(1,2);//从下标为1的元素开始删除，删除的长度为2即23，44两个数
console.log(arr);
//输出  [12, 5, 6, 23, 43, 34]
arr.splice(0,1,10)//从下标为0的元素开始替换，替换的长度为1即12替换成10
//输出  [10, 5, 6, 23, 43, 34]
```

3、**shift**方法:删除第一个数组元素，不带参数，数组的长度会减1

　　arr.shift();

　　注意:如果数组是空的，那么 shift() 方法将不进行任何操作，返回 undefined 值。请注意，该方法不创建新数组，而是直接修改原有的 arrayObject

```javascript
var arr=[12,23,44,5,6,23,43,34];
arr.shift();
console.log(arr)
//输出[23,44,5,6,23,43,34];
```

4、**pop**方法:删除数组的最后一个元素,数组的长度会减1,

 arr.pop();

```javascript
var arr=['a','b','c','d','e','f'];          
arr.pop();            
console.log(arr);
//输出 ['a','b','c','d','e'];    
```
**增加数组**

1、**unshift()**： 方法:在数组的第一个元素前面增加一个元素,数组的长度会加1，该方法会改变原来数组的长度.

arr.unshift(newElement)

```javascript
var arr = [0,1,2,3];
arr.unshift(100)
console.log(arr)
// [100, 0, 1, 2, 3]
```

2、**push()**:在数组的结尾追加元素,可以追加多个元素，该方法会改变原来数组的长度　

　　arr.push(newElement,...);

```javascript
var arr = [0,1,2,3];
arr.push(100)
console.log(arr)
// [ 0, 1, 2, 3, 100]
```

3、concat():合并两个或多个数组，该方法不会改变现有的数组，而仅仅会返回被连接数组的一个副本 ，原数组不变

```javascript
var a = [0,1,2]
var b = [3,4,5]
var c = a.concat(b)
console.log(c)
//输出 [0, 1, 2, 3, 4, 5]
```

4、sort():该方法是对数组进行升序排序，规则是按ascii表的规则来的

　　arr.sort();

```javascript
var a = ["d","z","r"]
a.sort();
console.log(a)
//输出 ["d", "r", "z"]
```

5、reverse():对数组进行翻转操作

　　arr.reverse();

```javascript
//js模拟原理实现的代码
var arr = ["诸葛亮","安琪拉","狄仁杰","后羿","荆轲","娜可露露","鲁班"];
//思路：实现方法：1.操作原数组，让原数组第一位和最后一个位调换位置，以此类推。
    for(var i=0;i<arr.length/2;i++){
        //让前后数组中的元素交换位置。
        var temp = arr[i];
        //前面项和对应的后面项交换位置。（arr.length-1-i = 倒数第i+1项）
        arr[i] = arr[arr.length-1-i];
        arr[arr.length-1-i] = temp;
    }
    console.log(arr);
```

**检查是否为数组**

arr.isArray(obj):该方法适用于确定传递的值是否为Array，是Array返回的则是true，否的话返回的是false 

intanceof 同样也是检测使用的，语法：a instanceof Object    返回的值是true or false;

**其他方法**

**splice()**:从当前数组中截取一个新的数组，不影响原来的数组，参数start从0开始,end从1开始(从第一个元素开始)

　　arr.slice(star,end);

注意：如果slice里面没有定义结束的位置的话，那么截取的元素就从被截取元素的开始位置一直截取到结束位置 

**arr.join()**：把数组转换成为字符串，可以自己定义分隔符 arr.join("自定义分隔符如&")，默认是逗号隔开，如果要没有分隔符的话，就arr.join(""); 

**arr.toString()**:同样是把数组转成字符串，但是返回的字符串每项都是用逗号隔开的 

##### 7.2 ES6 新增加方法

1、**Array.from**方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map） 

```javascript
let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
};

// ES5的写法
var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']

// ES6的写法
let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
```

2、解构

```javascript
var a = [1,2,3]
var b = [4,5,6]
var c = [...a]+[...b]
var d = [...a,...b]
console.log(c) //1,2,34,5,6
console.log(d) //[1,2,3,4,5,6]
```

3、**reduce()**方法接收一个函数作为累加器(accumulator),数组中的每个值(从左到右)开始合并,最终为一个值. 

```
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
参数	                描述
total	              必需。初始值, 或者计算结束后的返回值。
currentValue	      必需。当前元素
currentIndex	      可选。当前元素的索引
arr	                  可选。当前元素所属的数组对象。
initialValue	      可选。传递给函数的初始值
```

```javascript
var arr = [1,2,3,4,5,6]
arr.reduce((pre,cur)=>{
	return pre+cur
})
//输出21
```

4、**filter()**方法使用指定的函数测试所有元素,并创建一个包含所有通过测试的元素的新数组.  

```javascript
var a = [1, 2, 3, 4, 5];

var b = a.filter((item) => {
    return item > 3;
});
//输出[4, 5]
console.log(b);
```

5、**every()**方法用于测试数组中所有元素是否**都通过**了指定函数的测试. 

- currentValue【必选】：当前元素的值
- index【可选】当前元素的索引
- arr【可选】当前元素属于的数组对象
- 不改变原数组的值

```javascript
var a = [1, 2, 3, 4, 5];
var b = a.every((item) => {
    return item > 0;
});
var c = a.every((item) => {
    return item > 1;
});
console.log(b); // true
console.log(c); // false
```

6、**some()**方法用于测试数组中是否**至少有一项元素**通过了指定函数的测试. 

参数 item : 数组中正在处理的元素

参数index : 数组中正在处理的元素的下标

```javascript
var bb = a.some((item) => {
    return item > 4;
});

var cc = a.some((item) => {
    return item > 5;
});
console.log(bb);    // true
console.log(cc);    // false
```

7、**forEach()**为每个元素执行对应的方法. 

8、**indexOf()**方法返回在该数组中第一个找到的元素位置,如果它不存在则返回-1. 

9、**Object.assign()** 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。 

10、**findIndex()**  当条件为true时findIndex()返回的是索引值 ，否则返回 -1

11、**find()**  当条件为true是 find 返回是该元素，否则返回undefined

12、ES6中字符串的方法**includes()**    

#### 8、css动画

8.1过渡transition

```javascript
transition
```

| [transition](http://www.w3school.com.cn/cssref/pr_transition.asp) | 简写属性，用于在一个属性中设置四个过渡属性。 | 3    |
| ------------------------------------------------------------ | -------------------------------------------- | ---- |
| [transition-property](http://www.w3school.com.cn/cssref/pr_transition-property.asp) | 规定应用过渡的 CSS 属性的名称。              | 3    |
| [transition-duration](http://www.w3school.com.cn/cssref/pr_transition-duration.asp) | 定义过渡效果花费的时间。默认是 0。           | 3    |
| [transition-timing-function](http://www.w3school.com.cn/cssref/pr_transition-timing-function.asp) | 规定过渡效果的时间曲线。默认是 "ease"。      | 3    |
| [transition-delay](http://www.w3school.com.cn/cssref/pr_transition-delay.asp) | 规定过渡效果何时开始。默认是 0。             |      |

默认 ease ：慢速开始，中间变快，慢速结束；相当于 cubic-bezier(0.25, 0.1, 0.25, 1)。

可选 liner：匀速运动；相当于 cubic-bezier(0, 0, 1, 1)。

可选 ease-in：慢速开始；相当于 cubic-bezier(0.42, 0, 1, 1)。

可选 ease-out：慢速结束；相当于 cubic-bezier(0, 0, 0.58, 1)

可选 ease-in-out：慢速开始，慢速结束；相当于 cubic-bezier(0.42, 0, 0.58, 1)

可选 cubic-bezier(n, n, n, n)：在 bezier 函数中自定义 0 ~ 1 之间的数值。



8.2转换transform

2D转换

- translate()  位移
- rotate()   旋转
- scale()   缩放
- skew()    翻转
- matrix()   所有2D转换集合（6个参数）

3D转换

- rotateX()
- rotateY()