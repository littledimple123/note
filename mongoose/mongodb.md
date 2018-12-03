## 1.MongoDB

#### 1.1安装

[MongoDB下载地址](https://www.mongodb.com/download-center/community)

#### 1.2启动和关闭数据库

启动：

```javascript
//mongodb默认使用执行 mongod 命令所处盘符跟目录下的 /data/db 作为自己的数据存储目录，在第一次执行命令之前手动新建一个/data/db
mongod
```

想要修改默认的数据存储目录，可以

```javascript
mongod --dbpath=数据存储路径
```

停止：

```javascript
//在开启服务的控制台，直接ctrl+c即可停止或者直接关闭开启服务的控制台
```

#### 1.3连接和退出数据库

连接：

```javascript
//该命令默认连接本机MongoDB服务
mongo
```

 退出：

```javascript
//在连接状态输入 exit 退出连接
exit
```

#### 1.4基本命令

- `show dbs`
  - 查看所有数据库列表
- `use 数据库名称 `
  - 切换到指定的数据（如果没有会新建）
- `db` 显示当前数据库
- `db.student.insert({"name":"jack"})`  student 是数据集合  
- `show collenctions`  显示表集合
- `db.student.find()`  查询数据

#### 1.5在Node中如何操作MongoDB数据

###### 1.5.1使用mongo官方包

[https://github.com/mongodb/node-mongodb-native]()

###### 1.5.2使用第三方mongoose来操作MongoDB数据库

第三方包：mongoose基于MongoDB官方的mongodb包再一次做了封装

[https://mongoosejs.com/]()

## 2、mongoose

- 官网：[https://mongoosejs.com/]()
- 官方指南：[https://mongoosejs.com/docs/guide.html]()
- 官方API文档：[https://mongoosejs.com/docs/api.html]()

#### 2.1MongoDB数据库的基本概念

+ 可以有多个数据库
+ 一个数据库可以有多个集合(collections)（表）
+ 一个集合可以有多个文档
+ 文档结构很灵活，没有任何限制

####2.2起步

安装：

```javascript
npm i mongoose
```

示例：

```javascript
const mongoose = require('mongoose');

//连接mongodb数据库
mongoose.connect('mongodb://localhost/test');

//创建一个模型  就是在设计数据库   表的名字是Cat， 生成 cats
const Cat = mongoose.model('Cat', { name: String });

for (var i = 0; i < 100; i++) {
    //实例化一个Cat
    const kitty = new Cat({ name: '八戒' + i });

    //持久化保存Kitty实例
    kitty.save().then(() => console.log('meow'));
}
```

#### 2.3官方指南

##### 2.3.1设计 Scheme 发布Model

```javascript
var mongoose = require('mongoose')

var Schema = mongoose.Schema

//1.连接数据库  指定连接的数据库不需要存在，当插入第一条数据即可创建
mongoose.connect('mongodb://localhost/itcast')

//2.设计集合结构（表结构）
var userSchema = new Schema({
    userName: {
        type: String,
        required: true
    },
    password: {
        type: String,
        required: true
    },
    email: {
        type: String
    }
})

//3.将文档结构发布为模型
//   mongoose.modle 方法就是用来将一个架构发布为module，
//   第一个参数：穿融入一个大写名词单数字符串用来表示的数据库名称
//              mongoose会自动将大写的字符串生成小写复数的集合名称
//   第二个参数：架构 Schema
//   返回值：模型构造函数
var User = mongoose.model('User', userSchema)

//4.增删改查
```

增加：

```javascript
var admin = new User({
    userName: 'admin',
    password: '123456',
    email: 'admin@admin.com'
})

admin.save(function(err, ret) {
    if (err) {
        console.log('保存失败' + err)
    } else {
        console.log('保存成功' + ret)
    }
})
```

查询：

```javascript
//查询所有
User.find(function(err, ret) {
    if (err) {
        console.log(err)
    } else {
        console.log(ret)
    }
})

//按条件查询单个
 User.findOne({ userName: 'admin' }, function(err, ret) {
     if (err) {
         console.log(err)
     } else {
         console.log(ret)
     }
 })

//按条件查询所有
User.find({ userName: 'admin' }, function(err, ret) {
    if (err) {
        console.log(err)
    } else {
        console.log(ret)
    }
})
```

删除：

```javascript
User.remove({ userName: 'admin' }, function(err, ret) {
    if (err) {
        console.log(err)
    } else {
        console.log(ret)
    }
})
```

更新

```javascript
User.findByIdAndUpdate("5bff8a56392802049c53ed75", { password: "654789" }, function(err, ret) {
    if (err) {
        console.log(err)
    } else {
        console.log(ret)
    }
})

```





















