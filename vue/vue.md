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

.passive   不能和prevent一起用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你*不*想阻止事件的默认行为。 

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

##### 2.10  过滤器

过滤器可以用在两个地方：**双花括号插值和 v-bind 表达式**  

```javascript
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```



过滤器的定义语法

Vue.filter('过滤器名称',function(data){})

过滤器中的function第一个参数固定死，永远都是管道符前面传过来的数据，第二个参数就是过滤器名括号后面的内容。

过滤器可以在实例里面定义，也可以在全局定义(全局定义时必须在new Vue之前)

过滤器调用采用就近原则，如果全局过滤器和私有过滤器重名，优先使用私有过滤器

```javascript
{{date|newDate }}
//私有过滤器
var vm = new Vue({
    filters: {
       newDate(data) {                    
          return data.toLocaleDateString()
       }
    }
})

//全局过滤器  模板字符串，变量编译用 '${}' 来表示
   Vue.filter('newDate', function(data) {
        var dd = new Date(data);
        console.log(dd)
        var y = dd.getFullYear();
        var m = dd.getMonth();
        var d = dd.getDate()
        return `${y}-${m}-${d}`
   })
//function有两个参数的情况   {{date|newDate('yyyy-mm-dd')}}
 Vue.filter('newDate', function(data, type) {
            var dd = new Date(data);
            //console.log(dd)
            var y = dd.getFullYear();
            var m = dd.getMonth();
            var d = dd.getDate()
            if (type && type.toLowerCase() === 'yyyy-mm-dd') {
                return `${y}-${m}-${d}`
            } else {
                var hh = dd.getHours();
                var mm = dd.getMinutes();
                var ss = dd.getSeconds();
                return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
            }
        })
```



#### 3、表单处理事项

##### 3.1表单事件修饰符

**.lazy  ** `v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 ，你可以添加 `lazy` 修饰符，从而转变为使用 `change` 

```javascript
//在“change”时而非“input”时更新
<input v-model.lazy="msg" >
```

**.number**   如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符： 

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。 

```javascript
<div id='app'>       
        <input type='text' v-model.number='num' />
        <p>{{num}}</p>
        <button @click="assay()">获取当前的类型</button>
    </div>
    <script>
        var vm = new Vue({
            el: "#app",
            data: {                
                num: ''
            },
            methods: {
                assay() {
                    alert(typeof this.num)
                }
            }
        })
```

**.trim**  如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符

##### 3.2 字符串的 padStart()     padEnd()  填充字符串

如果某个字符串不够指定的长度，就在其头部或者尾部补全 

##### 3.3 按键修饰符

.enter

.tab

.delete（捕获“删除”和”退格“键）

.esc

.space

.up

.down

.left

.right

自定义键盘修饰符

```javascript
//在事件绑定时
<input type='text' @keyUp.f12='add'/>
//在全局定义
Vue.config.keycodes.f12=113
```



##### 3.4 系统修饰符

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`

- `.alt`

- `.shift`

- `.meta`

  **注意**：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。 

  #### 4、自定义指令

  在全局使用`Vue.directive()`指令,自定义指令，调用时用`v-`开头

  参数1，指令名称

  参数2，一个对象，对象中每个钩子函数的第一个参数都是el，即被绑定的元素

  具体的参数请见[钩子函数的参数][https://cn.vuejs.org/v2/guide/custom-directive.html]

  ```javascript
  Vue.directive('fouce'，{
     //第一次绑定到元素的时候就会立即执行，只执行一次      
     //和样式相关的操作，一般在bind中执行           
     bind:function(el){},
     //被绑定元素插入父节点时调用，就会被执行，执行一次 
     //和js行为相关的操作，最好在inserted中执行，防止js不生效
     inserted:function(el){},
     //所在组件的 VNode 更新时调用，可能执行多次    
     update:function(el){}    
   })
  ```

  私有自定义指令

  ```javascript
  directives:{
      focus:{
          bind:function(el){},
          inserted:function(el){},
          update:function(el){}    
      }
  }
  ```

  

#### 5、生命周期

beforeCreate()   实例被创建出来之前，data和methods 中的数据都还没有初始化

created()   实例被创建出来之后，data和methods 中的数据已经初始化，只是在内存中还没有挂载到真实DOM中

beforeMount()   挂载到dom前 ,模板已经在内存中编译完成了，但是尚未把模板渲染到页面中

mounted()     内存中的模板已经真实的渲染到页面中。如果要操作DOM节点，最早在此进行

运行阶段函数

beforeUpdate()   界面没有被更新，data数据更新了，页面和data数据还未同步

updated()    页面和data数据保持同步了。

销毁阶段

beforeDestroy()

destroyed()

#### 6、vue中发起ajax请求

vue-resonrce实现get，post, jsonp请求。还可以使用  `axios`  第三方包实现数据的请求

实例内部发送请求

请求成功数据是 result.body

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue-resource发送请求</title>
    <script src='js/vue.js'></script>
    <script src='https://cdn.staticfile.org/vue-resource/1.5.1/vue-resource.min.js'></script>
</head>

<body>
    <div id='app'>
        <input type='button' value='get请求' @click='getInfo' />
        <ul v-for='list in lists' key='list.id'>
            <li>{{list.id}}</li>
            <li>{{list.course_name}}</li>
            <li>{{list.autor}}</li>
            <li>{{list.college}}</li>
            <li>{{list.category_Id}}</li>
        </ul>
        <input type='button' value='post请求' @click='postInfo' />
        <input type='button' value='jsonp请求' @click='jsonpInfo' />
    </div>
    <script>
        var vm = new Vue({
            el: "#app",
            data: {
                dataUrl: 'http://localhost:3000/course',
                lists: []
            },
            methods: {
                getInfo() {
                    this.$http.get(this.dataUrl).then(function(result) {
                        result.body.map((d, i) => {
                            this.lists.push(d)
                        })

                    })
                },
                postInfo() {
                    //手动发起的post请求，默认没有表单格式，有的服务器处理不 ，通过post方法的第三个参数，{emulateJSON:true}设置提交内容类型为 普通表单数据格式
                    this.$http.post(this.dataUrl, {}, {
                        emulateJSON: true
                    }).then((result) => {
                        console.log(result)
                    })
                },
                jsonpInfo() {
                    this.$http.jsonp(this.dataUrl).then((result) => {
                        console.log(result.body)
                    })
                }
            },
        })
    </script>
</body>

</html>
```

jsonp实现原理：

- 由于浏览器的安全性限制，不允许ajax访问，协议不同，端口号不同的数据接口，浏览器会认为这种访问不安全
- 可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作jsonp（只支持get请求）
- 具体实现过程：
  - 先在客户端定义一个回调方法，预定义对数据的操作
  - 再把这个回调方法的名称，通过url传参的形式，提交到服务器的数据接口
  - 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行
  - 客户端拿到服务器返回的字符串之后，当做script脚本解析执行，这样就能拿到jsonp数据了

