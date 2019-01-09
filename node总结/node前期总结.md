#### 1.fs

```javascript
var fs = require('fs')
//readFile的第二个参数是可选的，传入utf8 就是告诉把读取的文件直接按照utf8编码输出
//也可以用 data.toString() 的方式
fs.readFile('./db.json', 'utf8', function(err, data){
    //从文件中读取的数据一定是字符串， 通过JSON.parse(data) 转换成对象
    console.log(JSON.parse(data))
})
```

#### 2.回调函数

要得到一个函数内部异步操作的结果

不成立情况：

```javascript
//异步编程，回调函数，不会等待，执行完之后再执行setTimeout
function add(x, y) {
  console.log(1)
  setTimeout(function () { 
    console.log(2)
    var ret = x + y
    return ret  //不可能拿到return值
  }, 1000)
  console.log(3)
  //到这执行结束，不会等到前面的定时器，所以直接返回undefined
}
console.log(add(10, 20))
//结果是  1 3 undefind 2  
```

不成立情况：

```javascript
function add(x, y) {
    var ret
    console.log(1)
    setTimeout(function () { 
      console.log(2)
      ret = x + y
     
    }, 1000)
  console.log(3)
  return ret
  }
  console.log(add(10, 20))   //结果是  1 3 undefind 2   
```

成立情况：

```javascript
function add(x, y, callback) {
  //callback 是回调函数
  // var x = 10
  // var y = 20
  // var callback = function(a){ console.log(a) }
  console.log(1)
  setTimeout(function () { 
    var ret = x + y
    callback(ret) // ret是实参
  },1000)
}
add(10, 20, function (a) {
  //a 才是我们得到的结果  a 是形参
  console.log(a)
 })
```

往往异步API都伴随一个回调函数  setTimeout  readFile  writeFile  readdir ajax 

**话题外**

node_modules误删之后，通过`npm update` 恢复

