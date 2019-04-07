

#### 1、let

var 

可以重复声明

无法限制修改（没有常量）

没有块级作用域

let

不能重复声明，

变量-可以修改，

块级作用域

const

不能重复声明，

常量-不可以修改，

块级作用域

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <script>
        //ES5中 var 生命变量，会提升。
        for (var i = 0; i < 10; i++) {
            setTimeout(function() {
                console.log(i)
            }, 100)
        }
        //ES5修改变量提升，用匿名函数自调
        for (var i = 0; i < 5; i++) {
            (function(i) {
                setTimeout(function() {
                    console.log(i)
                }, 100)
            })(i)
        }

        //ES6中let 块级作用域,{}就是一个块级作用域，for 循环除外，（）中也是一个块级作用域
        for (let i = 0; i < 5; i++) {
            setTimeout(function() {
                console.log(i)
            }, 100)
        }
    </script>
</body>

</html>
```

#### 箭头函数

只有一个参数，（）可以省

只有一个return ， {}  可以省

#### 函数-参数

参数扩展/数组展开

1收集剩余的参数，且 必须是最后一个

```javascript
function show(a,b,...args){
    alert(a)
    alert(b)
    alert(...args)
}
show(1,2,3,4,56,4,8)
//先弹1
//再弹2
//最后弹 3,4,56,4,8
```

2展开数组

```javascript
var arr=[1,2,3]
...arrr
console.log(arr)
```

#### 解构赋值

两边结构必须一致

声明和赋值不能分开，必须在一句话里

#### 数组

map()     

reduce()

filter()

forEach()

`Array.from`方法用于将两类对象转为真正的数组 

`Array.of`方法用于将一组值，转换为数组。 

数组实例的`copyWithin`方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组 

**Set**  

 `Set`本身是一个构造函数 

可以用来数组去重

```javascript
//方法一
var arr = [1,2,3,1,3,45]
var arr1 = [...new Set(arr)]
console.log(arr1)
//方法二
var arr = [1,2,3,1,3,45]
var arr1 =[...Array.from(new Set(arr))]
console.log(arr1)
```

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）

- `add(value)`：添加某个值，返回 Set 结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值。
- `add(value)`：添加某个值，返回 Set 结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值。 

**Map**

ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。 

+ `size`属性返回 Map 结构的成员总数。 
+ `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。 可以采用链式写法 
+ `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。 
+ `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。 
+ `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。 
+ `clear`方法清除所有成员，没有返回值 
+ `keys()`：返回键名的遍历器。
+ `values()`：返回键值的遍历器。
+ `entries()`：返回所有成员的遍历器。
+ `forEach()`：遍历 Map 的所有成员。

#### 字符串

- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

#### 面向对象

1. class关键字，constructor构造器和类分开
2. class里面直接加方法

```javascript
<script>
        class Person {
            constructor(name, age) {
                this.name = name;
                this.age = age
            }
            sayName() {
                alert(this.name)
            }
            sayAge() {
                alert(this.age)
            }
        }

        var zs = new Person('张三', '20')
        zs.sayAge()
        zs.sayName()
    </script>
```

3.继承

extends   扩展的意思

super    超类=父类

```javascript
<script>
        class Person {
            constructor(name, age) {
                this.name = name;
                this.age = age
            }
            sayName() {
                console.log(this.name)
            }
            sayAge() {
                console.log(this.age)
            }
        }


        class Sonperson extends Person {
            constructor(name, age, sex) {
                super(name, age)
                this.sex = sex
            }
            showSex() {
                console.log(this.sex)
            }
        }
        var p1 = new Sonperson("张三", 50, '男')
        p1.sayName()
        p1.sayAge()
        p1.showSex()
    </script>
```

#### json

JSON.stringify()  把json转为字符串

JSON.parse()   把字符串转为json

注意 ：json的标准写法

1.字符串和Object的键必须是双引号`""`

2.名字和值一样可以简写，写一个

```javascript
let a = 5;
let b = 6;
let json = {a,b}
console.log(json)
```

#### Promise---承诺

Promise 是异步编程的一种解决方案 

Promise.all()    全部请求成功才可以

```javascript
Promise.all( $.ajax() ，$.ajax()).then(result=>{
    console.log("成功了" + result)
},error=>{
    console.log("失败了" + err)
})
```

Promise.race()   至少一个请求成功就可以

