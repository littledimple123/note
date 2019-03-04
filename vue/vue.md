## Vue

#### 1.MVVM

+ MVVM是Model-View-ViewModel的简写 

  model : 代表数据模型，数据和业务逻辑都在model层中定义

  View ： 代表UI视图，负责数据的展示

  ViewModel : 负责监听Model中数据的改变并且控制视图的更新，处理用户交互操作

  Model 和View 并无直接关联，而是通过ViewModel来进行联系的，Model 和 ViewModel 之间有点双向数据绑定的联系，因此当model中数据改变时会触发View层的刷新， View 中由于用户交互操作而改变的数据也会在Model中同步。

  这种模式实现了Model和View 的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作dom 

优点

MVVM模式和MVC模式一样，主要目的是分离[视图](https://baike.baidu.com/item/%E8%A7%86%E5%9B%BE)（View）和模型（Model），有几大优点

**1. 低耦合**。视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。

**2. 可重用性**。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。

**3. 独立开发**。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xaml代码。

**4. 可测试**。界面素来是比较难于测试的，而现在测试可以针对ViewModel来写。

#### 2语法

##### 2.1 {{}} 实现插值

```javascript
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Hello World</title>
    <script src="js/vue.js"></script>
    <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>

<body>
    <div id="app">
        <p v-cloak>{{ message }}</p>
        <!-- 数据绑定最常见的形式就是使用 {{...}}（双大括号）的文本插值： -->
        <!-- 使用v-cloak 能够解决 插值表达式闪烁的问题 -->
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                message: 'hello world'
            }
        })
    </script>
</body>

</html>
```

##### 2.2  v-text 和 v-html

{{}}会有闪烁问题，只会替换自己这个占位符，不会把整个元素内容清空

v-text 默认不会有闪烁问题， 会覆盖元素中原来的内容
v-html 会解析html 标签,会覆盖元素中原来内容

```javascript
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Hello World</title>
    <script src="js/vue.js"></script>
</head>

<body>
    <div id="app">
        <p>ddddd {{mes}} ffffff</p>
        <!--会输出 ddddd 哈哈哈 ffffff-->
        <p v-text='mes'>aaaaa</p>
        <!--会输出 哈哈哈 ，并不会输出aaaaa-->
        <p v-html="message"></p>
        <!-- 使用 v-html 指令用于输出 html 代码： -->
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                message: '<h1>hello,world!</h1>',
                mes: '哈哈哈'
            }

        })
    </script>
</body>

</html>
```

##### 2.3 v-bind: 是vue 中用于绑定属性的指令  v-bind:  可以简写为 ：

```javascript
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <title>Hello World</title>
    <script src="js/vue.js"></script>
    <style>
        .red {
            color: #f00;
        }
    </style>
</head>

<body>
    <div id="app">
        <label for="r1">修改颜色</label><input type="checkbox" v-model="isred" id="r1">
        <br><br>
        <!--  HTML 属性中的值应使用 v-bind 指令。
  以下实例判断 class1 的值，如果为 true 使用 class1 类的样式，否则不使用该类： -->
        <div v-bind:class="{'red': isred}">
            <!-- 大括号里面的键名是字符串，键值是布尔值 -->
            aaaaaaaaaaaaaaaaaaaaaaaaa
        </div>
        <input type='button' :title="mytitle" value='按钮' />
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                isred: false,
                mytitle: '这是一个按钮'
            }
        });
    </script>
</body>

</html>
```

##### 2.4  v-on 绑定事件   

v-on可以简写为  @  

eg: @click

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Hello World</title>
<script src="js/vue.js"></script>
</head>
<body>
<div id="app">
  <h2>v-on.once</h2>
  <button type="button" @click.once="once_click">Button</button>
  <hr />
</div>
 
<script>
new Vue({
  el:"#app",
  methods: {
    once_click: function () {
      console.log("enter click...");
    }
  }

})
</script>
	

</body>
</html>
```

**注意： 在vm实例中，如果想要获取data上的数据，或者想要调用methods中的方法，必须通过this. 数据属性名   或者  this. 方法名进行访问。这里的this ，这里的this ，就表示 new 出来的 vm 实例对象**

##### 2.5 事件修饰符

.stop  阻止冒泡     事件由里往外，给谁加.stop就只会触发哪个事件

.prevent 阻止默认事件

.capture  添加事件监听器时使用事件捕捉模式      事件由外往里一次执行

.self  只当事件在该元素本身（比如不是子元素）触发时触发，里面的事件不触发

.once 事件只执行一次

事件修饰符可以多个一起用

##### 2.6 v-model 双向数据绑定

智能运用到表单元素中    input  select   checkbox textarea

 在data中多选框是一个空的数组 ,不可以是空字符串和空对象，

单选框可以使空数组，空字符串，空对象 ，

select可以是空数组也可以是空字符串也可以是空对象 

##### 2.7 样式问题

**class**

1。 通过v-bind 数据绑定，直接传一个数据，英文逗号隔开,

```javascript
<h1 :class=['active','thin']> 这是一个H1 </h1>
```

2。数组中使用三元表达式

```javascript
<h1 :class=['active','thin',isactive?'active':'']> 这是一个H1 </h1>
```

3。数组中嵌套对象

```javascript
<h1 :class=['active','thin',{'active':isactive}]> 这是一个H1 </h1>
```

4。直接使用对象，属性名不加引号也可以。如果属性中有-，必须加引号

```javascript
<h1 :class={'red':true，'font-size':true}> 这是一个H1 </h1>
```

**内联样式**

通过"data"中属性

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Hello World</title>
<script src="js/vue.js"></script>
</head>
<body>
<div id="app">  
  <div v-bind:style="[isshow?isblock:notblock]">111222333444555666</div>
</div>
	
<script>
new Vue({
	el: '#app',	
  data:{
  	isblock:{
      display:'block'
    },
    notblock:{
      display:'none'
    },
    isshow:true
  }
});

</script>
</body>
</html>
```

##### 2.8 v-for 循环

   1.key/index 都是可选参数
   2.在迭代属性输出的之前，v-for会对属性进行升序排序输出
   3.可以遍历数组，整数
   4.可以同时添加v-if指令，v-for优先级更高，
   5.如果使用v-for迭代数字的话，前面的count的值从 1 开始
   6.在一些特殊情况下，或者v-for有问题，必须使用v-for的同时，指定唯一的字符串/数字 类型的 :key 值       

```javascript
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Hello World</title>
<script src="js/vue.js"></script>
</head>
<body>
<div id="app">
  <div v-for="val in object">{{val}}</div>
  <hr>
  <div v-for="(val,key) in object" :key='key'>{{key}}:{{val}}</div>
  <hr>
   <div v-for="(val,key,index) in object">{{index}},{{key}}:{{val}}</div>
   <hr>
   <div v-for="person in persons">name:{{person.name}},age:{{person.age}}</div>
   <div v-for="person in persons" v-if="person.age >= 23">{{person.name}}</div>
  
</div>
 
<script>
   var vm=new Vue({
    el:"#app",
    data:{
      object:{
        name:"百度",
        link:"http://www.baidu.com",
        slogn:"百度一下，你就知道！"
      },
      persons: [
        {
          name: 'Dale',
          age: 22
        }, 
        {
          name: 'Tim',
          age: 30
        },
        {
          name: 'Rex',
          age: 23
        }
      ]
    }
   }) 
</script>
	

</body>
</html>
```

##### 2.9  v-if 和 v-show

v-if 的特点，每次都会重新删除或者创建元素，但是有较高的切换性能消耗，如果元素涉及频繁的切换，最好不要用v-if，使用v-show。永远不想被看见就用v-if

v-show 的特点  每次不会重新进行DOM 的删除和创建操作，只是切换了元素的display：none样式。但是，有较高的初始渲染消耗