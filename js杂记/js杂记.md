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

