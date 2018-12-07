## json-server

json-server是一个node模块，运行Express服务器，你可以指定一个json文件作为api的数据源

源码地址： [https://github.com/typicode/json-server]()

#### 安装json-server

```javascript
npm i -g json-server
```

#### 启动json-server

```javascript
json-server --watch [json文件]
```

+ 相关启动参数

+ 选项表

  ![](C:\Users\Administrator\Desktop\总结\1544065166325.png) 	

     语法：

  ```javascript
  json-server [option] <source>
  ```

  ```javascript
  json-server --watch -c db.json
  json-server --watch app.js
  json-server db.json
  json-server --watch -port 8888 db.json
  ```

  

  

  

  