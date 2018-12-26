



##1、sublime分屏快捷键   

​         shift+alt+2（分两屏）

## 2、node中CommonJs模块

#### 2.1加载`require`

语法：

```javascript
1.   var 自定义变量名称=require('模块')
```

​	两个作用：

​		*执行被加载模块中的代码

​		*得到被加载模块中的` exports`对象

#### 2.2导出`exports`

​		*node中是模块作用域，默认文件中所有的成员只在当前文件模块有效

​		*对于希望可以被其他模块访问的成员，我们就需要把这些公开的成员都挂载到`exports`接口中就可以了

​		导出多个成员（必须在对象中）：

```javascript
1. exports.a=123
2. exports.b='foo'
3. exports.c=function(){
    console.log(111)
}
4. exports.s={
    foo:'bar'
}
```

​		导出单个成员（拿到的就是函数、字符串）：

```javascript
module.exports='hello'
```

一下情况会被覆盖

```javascript
module.exports='hello'
module.exports=function(x, y){
    return x+y
}
```

也可以这样导出多个

```javascript
module.exports={
    add:function(x, y){
        return x+y
    },
    str:'hello'
}
```

#### 2.3原理解释

exports和`module.exports`等价

```javascript
console.log(exports===module.exports)  =>true
1 exports.foo = 'bar'
2 module.exports.add = function (x, y) {
  return x + y
 }
等价于
3 exports.add=function(x,y){
    return x + y
}


```

注释：node底层

在 Node 中，每个模块内部都有一个自己的 module 对象 ，该 module 对象中，有一个成员叫：exports 也是一个对象 ，当需要对外导出成员，只需要把导出的成员挂载到 module.exports 中 

```javascript
var module={
    exports:{
        
    }
}
return module.exports  //最后return 一定是module.exports
```

```javascript
// {foo: bar}
exports.foo = 'bar'

// {foo: bar, a: 123}
module.exports.a = 123

//exports重新赋值了，导致 exports !== module.exports
// 最终 return 的是 module.exports
// 所以无论你 exports 中的成员是什么都没用
exports = {
  a: 456
}
 
// {foo: 'haha', a: 123}
module.exports.foo = 'haha'

// 没关系，混淆你的
exports.c = 456

// 重新建立了和 module.exports 之间的引用关系了
exports = module.exports

// 由于在上面建立了引用关系，所以这里是生效的
// {foo: 'haha', a: 789}
exports.a = 789

// 前面再牛逼，在这里都全部推翻了，重新赋值
// 最终得到的是 Function
module.exports = function () {
  console.log('hello')
}
```

## 3、状态码

\- 301 和 302 状态码区别

  \+ 301 永久重定向，浏览器会记住

  \+ 302 临时重定向

## 4、npm

####4.1初始化

- 建议每个项目都有package.json文件。npm init  初始化
- 建议执行`npm install`包名的时候都加上`--save`或者`-S`这个选项，目的是用来保存以来信息
- 如果你的`node_module`删除了也不用担心，我们只需要`npm install` 就会自动把`package.json`中的`dependencies`中的所有以来都下载下来

#### 4.2网站

```javascript
npmjs.com
```

  #### 4.3常用命令

```javascript
1 npm -v    //查看npm版本
2 npm install --global npm      //升级npm(自己升级自己)
3 npm init
  npm init -y 
4 npm install         //一次性把dependencies选项中的依赖全部安装
5 npm install 包名     //只下载
6 npm install --save   // 下载并且保存 依赖项（package.json文件中的dependencies选项）
7 npm uninstall 包名    //只删除，如果有依赖项会依然保存   npm un 包名
8 npm uninstall --save 包名    //删除的同事也会把依赖信息去除   npm un -S 包名
9 npm -- help      //查看使用帮助
```

#### 4.4解决npm被墙的问题

淘宝镜像网址 `http://npm.taobao.org/`

安装淘宝的cnpm

```javascript
1 npm i --global cnpm   //在任意一个目录执行都可以
```

接下来安装包的时候把之前的npm替换成cnpm

如果不想安装`cnpm`又想使用淘宝的服务器来下载。

```javascript
1 npm i jquery --registry=https://registry.npm.taobao.org 
```

但是每一次手动这样加参数很麻烦，所以我们可以把这个选项加入配置文件中

```javascript
1 npm config set registry https://registry.npm.taobao.org 

//查看npm配置信息
2 npm config list
```

只要经过了上面命令配置，则以后所有的`npm intall  `都会默认通过淘宝的服务来下载

## 5、Express

#### 5.1基本语法

```javascript
// 0. 安装
// 1. 引包
var express = require('express')

// 2. 创建你服务器应用程序
//    也就是原来的 http.createServer
var app = express()

// 在 Express 中开放资源就是一个 API 的事儿
// 公开指定目录
// 只要这样做了，你就可以直接通过 /public/xx 的方式访问 public 目录中的所有资源了
app.use('/public/', express.static('./public/'))
app.use('/static/', express.static('./static/'))
//app.use('/static/', express.static(path.join(__dirname,'./static/')))  //把相对路径变成绝对路径

// 模板引擎，在 Express 也是一个 API 的事儿

app.get('/about', function (req, res) {
  // 在 Express 中可以直接 req.query 来获取查询字符串参数
  console.log(req.query)
  res.send('你好，我是 Express!')
})
// 当服务器收到 get 请求 / 的时候，执行回调处理函数
app.get('/', function (req, res) {
  res.send(`
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
  </head>
<body>
  <h1>hello Express！你好</h1>
</body>
</html>
`)
})

// 相当于 server.listen
app.listen(3000, function () {
  console.log('app is running at port 3000.')
})

```

#### 5.2文件操作路径和模块标识路径

```javascript
在文件操作的相对路径中
    ./data/a.txt 相对于当前目录      require时 其中.不能省略
    data/a.txt   相对于当前目录      读取文件时
    /data/a.txt  绝对路径，当前文件模块所处磁盘根目录
    c:/xx/xx...  绝对路径
```

#### 5.3安装并输出hello world

```javascript
1 npm i -S express
```

#### 5.4使用nodemon 工具自动重启服务

`nodemon`是一个基于node.js开发的一个第三方命令行工具，需要独立安装

```javascript
1 npm i --global nodemon
```

安装完之后，使用

```javascript
1 node index.js

#使用nodemo
2 nodemon index.js
```

监听文件变化，当文件发生变化的时候，自动帮你重启服务器

#### 5.5使用路由

###### 5.5.1基本路由

请求方法+请求路径+请求函数

get:

```javascript
app.get('/',function(req, res) {
	res.send('hello world')
})
```

post:

```javascript
app.post('/',function(req, res) {
	res.send('hello world')
})
```

###### 5.5.2静态服务

```javascript
1 app.use('/public/',express.static('./public/'))    //去public目录下找对应的文件，浏览器访问localhost:3000/public/login.html
或者
2 app.use('',express.static('./public/'))    //省略第一个参数，在浏览器中访问localhost:3000/login.html
或者
3 app.use('/a/',express.static('./public/'))        //必须是/a/public目录中的文件，a相当于public的别名    在浏览器中访问   localhost:3000/a/login.html
```

#### 5.6在Express中配置使用art-template模板引擎

[art-template官方文档](http://aui.github.io/art-template/)

安装：

```shell
npm install --save art-template
npm install --save express-art-template
```

配置：

```javascript
app.engine('html', require('express-art-template'));
```

使用：

```javascript
app.get('/', function (req, res) {
    //express 默认会去项目中的views目录找index.html
    res.render('index.html', {
        user: {
            name: 'aui',
            tags: ['art', 'template', 'nodejs']
        }
    });
});
```

如果希望修改默认的`views` 示图渲染存储目录

```javascript
app.set('views',目录路径)
```

**模板继承和子模板**

模板继承

```javascript
{{extend './layout.html'}}
{{block 'head'}} {{/block}}
```

子模板

```javascript
{{include './header.html'}}
{{block 'head'}} {{/block}}
```

#### 5.7在express获取表单get请求体数据

Express内置了一个API,可以直接通过`req.query`来获取

```javascript
req.query
```

#### 5.8在express获取表单post请求体数据

在express中没有内置获取表单POST请求体的API，这里我们需要使用一个第三方包：`body-parser`

安装：

```javascript
npm i -S body-parser
```

配置：

```javascript
var express = require('express')
// 0 引包
var bodyParser = require('body-parser')

var app = express()

// 1 配置body-parser
// 只要加入这个配置，则在req请求对象上会多出来一个属性：body, 可以直接通过req.body来获取表单POST请求体数据了
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())

app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  //通过req.body来获取表单POST请求体数据
  res.end(JSON.stringify(req.body, null, 2))
})
```

#### 5.9 path路径操作模块

+ path.basename  获取路径的文件名包括扩展名

+ path.dirname    获取路径中目录部分

+ path.extname    获取路径中扩展名

+ path.parse          把路径转为对象

  ​                                 root    跟路径

  ​                                 dir      目录

  ​                                 base   包含后缀名的文件名

  ​                                 ext      后缀名

  ​                                 name   不包括后缀名的文件名

+ path.join    需要路径拼接

+ path.isAbsolute   是否绝对路径

```javascript
//url.parse方法使用说明
url.parse(urlStr, [parseQueryString], [slashesDenoteHost])


接收参数：

urlStr url字符串

parseQueryString 为true时将使用查询模块分析查询字符串，默认为false

slashesDenoteHost

默认为false，//foo/bar 形式的字符串将被解释成 { pathname: ‘//foo/bar' }

如果设置成true，//foo/bar 形式的字符串将被解释成 { host: ‘foo', pathname: ‘/bar' }

var url = require('url');
var a = url.parse('http://example.com:8080/one?a=index&t=article&m=default',true);
console.log(a);

```



####5.10Express中的json

Express提供了一个响应方法： json

该方法接收一个对象作为参数，他会自动帮你把对象转为字符串在发送给浏览器

#### 5.11Express中的express-session

安装：

```javascript
npm i express-session
```

配置：

```javascript
app.use(session({
    secret: 'keyboard cat',   //配置加密字符串，他会在原有的加密基础之上和这个字符串拼起来去加密，目的增加安全性，防止客户端恶意伪造
    resave: false,
    saveUninitialized: true    //true的话无论是否使用session，都默认直接分配一把钥匙
}))
```

使用：

```javascript
//添加session数据
req.session.foo='bar'
//获取session数据
req.session.foo
```

提示：默认session数据是内存存储的，服务器一旦重启就会丢失，真正的生产环境会把session进行持久化存储

持久化存储把session存入Resis中

connect-reids　 是一个 Redis 版的 session　存储器，使用node_redis作为驱动。借助它即可在Express中启用Redis来持久化你的Session 

```javascript
//安装
npm i connect-redis
```

参数配置：

```ja
client: 你可以复用现有的redis客户端对象， 由redis.createClient() 创建

host: Redis服务器名

port: Redis服务器端口

socket: Redis服务器的unix_socket

ttl: Redis session TTL 过期时间 （秒）

disableTTL: 禁用设置的 TTL

db: 使用第几个数据库

pass: Redis数据库的密码

prefix: 数据表前辍即schema, 默认为 "sess:"
```

使用：

```javascript
// 将 express-session 传给 connect-redis 来启用
// 引入相关模块：
var session = require('express-session');  
var redis = require('redis');  
var RedisStore = require('connect-redis')(session);


// 创建Redis客户端
var redisClient = redis.createClient(6379, '127.0.0.1', {auth_pass: 'password'});


// 设置 Express 的 Session 存储中间件
app.use(session({  
    store:new RedisStore({client: redisClient}),
    secret: 'password',
    resave: false,
    saveUninitialized: false
}))

// 测试
app.use(function (req, res, next) {  
  if (!req.session) {
    return next(new Error('error'))
  }
  next()
})
```



##6、node中的其他成员

在每个模块中，除了`require`  `exports`  等模块相关API之外，还有两个特殊的成员：

+ `__dirname`  **动态获取**当前文件模块所属目录的绝对路径
+ `__filename`  **动态获取**当前文件的绝对路径

在文件操作中，使用相对路径是不可靠的，因为在node中文件操作的路径被设计为相对执行node命令所处的路径。（不是bug）使用`__dirname`  和 `__filename`  解决

```javascript
var fs = require('fs')
var path = require('path')
fs.readFile(path.join(__dirname,'./00-文件路径.js'),function(err, data){
    if(err){
        throw err
    }
    console.log(data.toString())
})
```

```javascript
//nodejs + express访问静态资源
app.use('/node_modules', express.static(path.join(__dirname, "./node_modules")))
```

## 7、node中时间插件

silly-datetime 时间插件

安装：

```shell
npm i silly-datetime --save
```

配置：

```javascript
var sd = require('silly-datetime');
sd.format(new Date(), 'YYYY-MM-DD HH:mm');
// 2015-07-06 15:10
 
sd.fromNow(+new Date() - 2000);
// a few seconds ago
```

[silly-datetime链接文档](https://www.npmjs.com/package/silly-datetime)

## 8、路由

#### 8.1静态路由

```javascript
var express = require('express')
var app = express()
//静态路由
app.use('/public/',express.static('./public'))
app.listen(3000,function() {console.log('running......')})
```

#### 8.2用Router 创建路由

router.js

```javascript
var Student = require('./student')

// Express 提供了一种更好的方式
// 专门用来包装路由的
var express = require('express')

// 1. 创建一个路由容器
var router = express.Router()

// 2. 把路由都挂载到 router 路由容器中
router.get('/students', function (req, res) {
  console.log(req.body)
})
// 3. 把 router 导出
module.exports = router
```

app.js(入口js文件)

```javascript
var router = require('./router')
//挂载路由
app.use(router)
```

#### 8.3设计操作文件数据的API文件模块

```javascript
/**
 * 数据操作文件模块
 * 不涉及业务，只对文件进行操作，增 删 改 查
 */


 /**
  * 获取所有学生列表
  */

exports.find = function () {

 }

/**
  * 添加保存学生
  */

exports.save = function () {
  
}

/**
* 更新学生
*/

exports.update = function () {
  
}

/**
* 删除学生
*/

exports.delect = function () {
  
}
```

## 9 Promise

回调地狱 callback hell

无法保证顺序：

```javascript
var fs = require('fs')

fs.readFile('./data/a.text','utf8',function(err,data){
    if(err){
        //抛出异常：
        //    1. 阻止程序的执行
        //    2. 把信息消息打印到控制台
        throw err
    }
    console.log(data)
})

fs.readFile('./data/b.text','utf8',function(err,data){
    if(err){
        //抛出异常：
        //    1. 阻止程序的执行
        //    2. 把信息消息打印到控制台
        throw err
    }
    console.log(data)
})

fs.readFile('./data/c.text','utf8',function(err,data){
    if(err){
        //抛出异常：
        //    1. 阻止程序的执行
        //    2. 把信息消息打印到控制台
        throw err
    }
    console.log(data)
})
```

通过回调嵌套的方式来保证顺序：

```javascript
var fs = require('fs')

fs.readFile('./data/a.text','utf8',function(err,data){
    if(err){
        //抛出异常：
        //    1. 阻止程序的执行
        //    2. 把信息消息打印到控制台
        throw err
    }
    console.log(data)
	fs.readFile('./data/b.text','utf8',function(err,data){
    	if(err){
        	//抛出异常：
        	//    1. 阻止程序的执行
        	//    2. 把信息消息打印到控制台
        	throw err
    	}
    	console.log(data)
        fs.readFile('./data/c.text','utf8',function(err,data){
    		if(err){
        		//抛出异常：
        		//    1. 阻止程序的执行
        		//    2. 把信息消息打印到控制台
        		throw err
    		}
    		console.log(data)
		})
	})
})
```

为了解决以上编码方式带来的问题（回调地狱嵌套），所以在ES6中新增一个API:`promise`

promise是一个构造函数(本身不是异步，但是内部往往都是封装一个异步)

```javascript
var fs = require('fs')

//创建Promise 容器
var p1 = new Promise(function(resolve, reject) {
    fs.readFile('a.txt', 'utf8', function(err, data) {
        if (err) {
            //失败调用reject 往下传递参数，且只接受一个参数
            reject(err)
        } else {
            //成功调用resolve 往下传递参数，且只接受一个参数
            resolve(data)
        }
    })
})

var p2 = new Promise(function(resolve, reject) {
    fs.readFile('b.txt', 'utf8', function(err, data) {
        if (err) {
            //承诺失败容器中任务失败了
            //把容器的Pending的状态变为 Rejected
            reject(err)
        } else {
            //承诺容器中任务成功了
            //把容器的Pending的状态变为成功 Resolve
            resolve(data)
        }
    })
})

var p3 = new Promise(function(resolve, reject) {
    fs.readFile('c.txt', 'utf8', function(err, data) {
        if (err) {
            //承诺失败容器中任务失败了
            //把容器的Pending的状态变为 Rejected
            reject(err)
        } else {
            //承诺容器中任务成功了
            //把容器的Pending的状态变为成功 Resolve
            resolve(data)
        }
    })
})

p1
    .then(function(data) {
        console.log(data)
            //当p1读取成功的时候，return 一个Promise 对象的时候，后续的 then 中的方法的第一个参数会作为 P2 的 resolve
        return p2
    }, function(err) {
        console.log("读取失败" + err)
    })
    .then(function(data) {
        console.log(data)
        return p3
    })


//封装promise-API 
var fs = require('fs')
function pReadFile(filepath){
    return new Promise(function(resolve, reject){
        fs.readFile(filepath,'utf8',function(err, data){
            if(err){
                reject(err)
            }else{
                resolve(data)
            }
        })
    })
}

pReadFile('a.txt')
 .then(function(data){
    console.log(data)
    return pReadFile('b.txt')
}).then(function(data){
    console.log(data)
    return pReadFile('c.txt')
})
```

##### 每次调用then都会返回一个新创建的Promise对象，不管是then还是catch方法调用，都返回一个新的Promise

Promise.catch()方法是promise.then(undefined,onRejected)方法的一个别名，该方法用来注册当promise对象状态变为Rejected的回调函数。 

```javascript
var promise = Promise.reject(new Error("失败"))
promise.catch(function(error){
    console.log(error)
})
```

```javascript
var promise = new Promise(function(resolve){
    resolve(1)
})
promise.then(function(value){
	return value*2	
})
promise.then(function(value){
    return value*2
})
promise.then(function(value){
    console.log('1'+value)
})

//打印输出 11  因为每次调用then方法时，使用不同的promise对象，
```

##### 使用方法链的then，多个then方法连接在一起，函数严格执行resolve--then--then--then的顺序执行，并且传递每个then方法的value的值都是前一个promise对象中return的值

```javascript
var promise = new Promise(function(resolve){
    resolve(1)
})
promise.then(function(value){
	return value*2	
}).then(function(value){
    return value*2
}).then(function(value){
    console.log('1'+value)
})

//打印输出 '1'+ 1*2*2 = 15
```

##### 延伸jquery中的promise

```javascript
var ajaxPromise = new Promise(function(resolve, reject){
    resolve()
})

ajaxPromise.then(function(){
    $.ajax({
        url:'',
        dataType:'json',
        success:function(data){
            
        }
    })
}).then(function(){
    $.ajax({
        url:'',
        dataType:'json',
        success:function(data){
            
        }
    })
})
```

##### Promise.all

Promise.all可以接受一个元素为Promise对象的数组作为参数，当这个数组里面所有的promise对象都变为resolve时，该方法才会返回。 

```javascript
var promise1 = new Promise(function(resolve, reject){
    setTimeout(function(){
        resolve(1)
    },3000)
})

var promise2 = new Promise(function(resolve, reject){
    setTimeout(function(){
        resolve(2)
    },1000)
})

Promise.all([promise1, promise2]).then(function(data){
    console.log(data)
})

//打印结果[1,2]  promise1对象中的setTimeout是3秒的时间，而promise2对象中的setTimeout是1秒的时间，但是在Promise.all方法中会按照数组的原先顺序将结果返回；
```

##### Promise race

promise.race只要有一个promise对象进入FullFilled或者Rejected状态的话，程序就会停止，且会继续后面的处理逻辑

```javascript
//场景一
function timerPromise(delay) {
    return new Promise(function(resolve) {
        setTimeout(() => {
            resolve(delay)
        }, delay);
    })
}

//任何一个promise变成resolve或reject的话程序停止执行
Promise.race([timerPromise(1), timerPromise(32), timerPromise(64)]).then(function(value) {
    console.log(value)   //输出1
})

//场景二
var rpromise1 = new Promise(function(resolve, reject) {
    setTimeout(function() {
        console.log(1)
        resolve(2)
    }, 300)
})

var rpromise2 = new Promise(function(resolve, reject) {
    setTimeout(function() {
        console.log(3)
        resolve(4)
    }, 1000)
})

//这种情况下，当一个promise对象变为(FulFilled)成功状态的时候，后面的promise对象并没有停止运行
Promise.race([rpromise1, rpromise2]).then(function(value) {
    console.log(value) //输出 1 2 3
})

```

##10、中间件

```javascript

```



