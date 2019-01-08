## 1、react总结

#### 为什么学习react

1、和angular相比，React设计很优秀，一切基于js并且实现了组件化开发的思想

2、开发团队实力强悍，不必担心断更的情况

3、社区强大，很多问题都能找到对应的解决方案

4、提供了无缝转到ReactNative上的开发体验，让我们技术能力得到了扩展，增强了核心竞争力

5、很多企业，前端项目的技术选型采用的是react.js

#### Diff算法

Tree Diff      component diff       element diff

#### 创建基本的webpack4.x项目

1、初始化一个项目配置文件

```javascript
npm init -y
```

2、在项目根目录创建`src`源代码目录和`dist`产品目录

3、在src目录下创建index.html

4、安装webpack

5、npm安装淘宝镜像

```javascript
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

​全局运行`npm i cnpm -g`              

#### 在项目中使用react

1. 运行`npm i react react-dom -S`安装包
   - react  专门用于创建组件和虚拟DOM的，同时组件的生命周期都在这个包中
   - react-dom     专门进行DOM操作的，最主要的应用场景 就是 ReactDOM.render()

2. 使用React.createElement创建虚拟DOM

   2.1 index.html中创建容器

   ```javascript
   <div id='app'></div>
   ```

   2.2 index.js中引包创建虚拟dom并渲染

   ```javascript
   //1. 导入 react  react-dom 
   import React from 'react'   //创建组件，虚拟dom 生命周期
   import ReactDOM from 'react-dom'  //
   
   ```

   2.3 index.js中创建虚拟dom

   ```javascript
   //2. 创建虚拟DOM
   //参数1： 创建元素的类型，字符串，表示元素的名称
   //参数2： 是一个对象或 null ，表示当前这个 DOM 元素的属性
   //参数3： 子节点（包括 其他 虚拟dom 获取文本子节点）
   const myh1 = React.createElement('h1', { id: 'myh1', title: 'haha' }, 'This is a index page')
   ```

   2.4 index.js 中渲染dom

   ```javascript
   //3. 使用 ReactDOM 把虚拟DOM 渲染到页面上
   //参数1： 要渲染的那个虚拟DOM元素
   //参数2： 指定页面上的dom元素，当做容器
   ReactDOM.render(myh1, document.getElementById('app'))
   ```

3. jsx(混合写入类似于HTML的语法，叫做jsx，符合xml的js)

   jsx语法的本周还是在运行的时候，被babel转化成了React.createElement形式来执行

   3.1 如何启用jsx语法

   - 安装`babel`插件

     ```javascript
     npm i babel-core babel-loader babel-plugin-transform-runtime -D
     npm i babel-preset-env babel-preset-stage-0 -D
     ```

   - 安装能够识别转换jsx语法的包babel-preset-react

     ```javascript
     npm i babel-preset-react -D
     ```

   ​	配置webpack.config.js

   ```javascript
   let path = require('path')
   let HtmlWebpackPlugin = require('html-webpack-plugin')
   let CleanWebpackPlugin = require('clean-webpack-plugin')
   let webpack = require('webpack')
   module.exports = {
       entry: './src/index.js',
       output: {
           filename: '[name][hash:8].js',
           path: path.resolve('./build')
       },
       devServer: {
           contentBase: './build',
           port: 3000,
           compress: true //服务器压缩
       }, //开发服务器
       module: {
           rules: [{
               test: /\.js|jsx$/,
               use: 'babel-loader',
               //千万记住添加exclude排除项
               exclude: /node_modules/     
               
           }]
       }, //模块设置
       plugins: [
           new webpack.HotModuleReplacementPlugin(),
           new CleanWebpackPlugin('./build'),
           new HtmlWebpackPlugin({
               template: path.resolve('./src/index.html'),
               title: 'test',
               hash: true,
           })
       ], //插件配置
       mode: 'development', //可以更改模式
       resolve: {} //配置解析
   }
   ```

   babel配置文件

   在项目根目录下创建`.babelrc`文件，配置babel，`.babelrc`文件是json文件，只能是双引号，不能有注释。

   把安装好的语法和插件写到这个文件中

   ```javascript
   {
     "presets":["env","stage-0","react"],
     "plugins":["transform-runtime"]
   }
   ```

    