## Vue

#### 1.MVVM

+ MVVM是Model-View-ViewModel的简写 

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

.once 表示执行一次

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

