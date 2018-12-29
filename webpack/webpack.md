#### 使用webpack

1、初始化package.json

```javascript
npm init -y
```

2、安装webpack

```javascript
//全局安装 （不推荐）
npm i webpack -g
//本地安装 4.0得安装webpack-cli
npm i webpack webpack-cli -D
```

3、运行webpack

`npx webpack`直接允许webpack，会执行node_modules对应的bin下的webpack.cmd

```javascript
//假如没安装webpack会直接安装
npx webpack 
```

4、webpack配置文件

最基本的配置webpack.config.js

```javascript
let path = require('path')
module.exports = {
    entry: './src/index.js', //入口，可以是字符串，数组，对象
    output: {
        filename: 'build[hash:8].js', //修改打包文件名  多文件出口写成  [name]build[hash:8 ].js
        path: path.resolve('./build') //修改打包文件夹的名字
    }, //出口
    devServer: {
        contentBase: './build',
        port: 3000,
        compress: true //服务器压缩
    }, //开发服务器
    module: {}, //模块设置
    plugins: [], //插件配置
    mode: 'development', //可以更改模式
    resolve: {} //配置解析

}
```

package.json

```javascript
{
    "name": "webpack00",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
        "build": "webpack", 
        "start": "webpack-dev-server"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "devDependencies": {
        "webpack": "^4.28.2",
        "webpack-cli": "^3.1.2",
        "webpack-dev-server": "^3.1.14"
    }
}

```

终端运行

```javascript
npm run start  开发 
npm run build  生产
```

5、webpack 插件

1. HtmlWebpackPlugin  该插件将为你生成一个HTML5文件

安装

```javascript
npm i html-webpack-plugin -D
```

配置webpack.config.js

```javascript
let path = require('path')
//1.引插件
let HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: './src/index.js', //入口
    output: {
        filename: 'build[hash:8].js', //修改打包文件名
        path: path.resolve('./build') //修改打包文件夹的名字,这个路径必须是绝对路径
    }, //出口
    devServer: {
        contentBase: './build',
        port: 3000,
        compress: true, //服务器压缩
        //hot: true,
        open: true //自动打开浏览器
    }, //开发服务器
    module: {}, //模块设置
    plugins: [
        //2.打包html文件
        new HtmlWebpackPlugin({
            template: './src/index.html',
            title:'练习首页',   //html页面中title显示此处的文字需要在html页面中写<%=htmlWebpackPlugin.options.title%>
            hash: true, //清缓存
            minify: {
                removeAttributeQuotes: true, //去掉双引号
                collapseWhitespace: true //折叠一行
			   
            }
        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}


运行命名： npm run start
```

2. CleanWebpackPlugin 清空生产生成的文件（build）

安装

```javascript
npm i clean-webpack-plugin -D
```

配置webpack-config.js

```javascript
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
//1. 引插件
let CleanWebpackPlugin = require('clean-webpack-plugin')
module.exports = {
    entry: './src/index.js', //入口
    output: {
        filename: 'build[hash:8].js', //修改打包文件名
        path: path.resolve('./build') //修改打包文件夹的名字,这个路径必须是绝对路径
    }, //出口
    devServer: {
        contentBase: './build',
        port: 3000,
        compress: true, //服务器压缩
        //hot: true,
        //open: true //自动打开浏览器
    }, //开发服务器
    module: {}, //模块设置
    plugins: [
        //2. 清空生产的文件夹 清除多个可以是数组        
        new CleanWebpackPlugin('./build'),
        //打包html文件
        new HtmlWebpackPlugin({
            template: './src/index.html',
            title: '首页练习',
            hash: true //清缓存
                // minify: {
                //     removeAttributeQuotes: true, //去掉双引号
                //     collapseWhitespace: true //折叠一行

            // }

        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}

运行命名： npm run start
```

3. 多页a.html 引入index.js     b.html 引入 a.js

```javascript
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
module.exports = {
    //1. 入口设置为一个对象
    entry: {
        index: './src/index.js',
        a: './src/a.js'
    }, //入口  可以是字符串，数组，对象
    //2. 出口根据入口的key值命名打包文件
    output: {
        filename: '[name]build[hash:8].js', //修改打包文件名
        path: path.resolve('./build') //修改打包文件夹的名字,这个路径必须是绝对路径
    }, //出口
    devServer: {
        contentBase: './build',
        port: 3000,
        compress: true, //服务器压缩
        //hot: true,
        //open: true //自动打开浏览器
    }, //开发服务器
    module: {}, //模块设置
    plugins: [
        //清空生产的文件夹
        new CleanWebpackPlugin('./build'),
        //3. 打包两个html文件
        new HtmlWebpackPlugin({
            filename: 'a.html',  //打包完的html的名字
            chunks: ['index'],   //打包完的html引入的js,与上面入口文件的key值一致
            template: './src/index.html',
            title: '首页练习',
            hash: true //清缓存
                // minify: {
                //     removeAttributeQuotes: true, //去掉双引号
                //     collapseWhitespace: true //折叠一行

            // }

        }),
        new HtmlWebpackPlugin({
            filename: 'b.html',
            chunks: ['a'],
            template: './src/index.html',
            title: '首页练习',
            hash: true //清缓存
                // minify: {
                //     removeAttributeQuotes: true, //去掉双引号
                //     collapseWhitespace: true //折叠一行

            // }

        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}


运行命名： npm run start
```

4. 及时更新

   配置webpack.config.js

   ```javascript
   let path = require('path')
   let HtmlWebpackPlugin = require('html-webpack-plugin')
   let CleanWebpackPlugin = require('clean-webpack-plugin')
   //1. 引入webpack插件
   let webpack = require('webpack')
   module.exports = {
       entry: './src/index.js', //入口
       output: {
           filename: 'build[hash:8].js', //修改打包文件名
           path: path.resolve('./build') //修改打包文件夹的名字,这个路径必须是绝对路径
       }, //出口
       devServer: {
           contentBase: './build',
           port: 5000,
           compress: true, //服务器压缩
           hot: true,
           open: true //自动打开浏览器
       }, //开发服务器
       module: {}, //模块设置
       plugins: [
           //2. 热更新
           new webpack.HotModuleReplacementPlugin(),
           //清空生产的文件夹 清除多个可以是数组        
           new CleanWebpackPlugin(['./build']),
           //打包html文件
           new HtmlWebpackPlugin({
               template: './src/index.html',
               title: '首页练习',
               hash: true //清缓存
                   // minify: {
                   //     removeAttributeQuotes: true, //去掉双引号
                   //     collapseWhitespace: true //折叠一行
   
               // }
   
           })
       ], //插件配置
       mode: 'development', //可以更改模式 'development' 和'production'
       resolve: {} //配置解析
   
   }
   ```

   index.js

   ```javascript
   var hy = require('./a.js')
   console.log(hy)
   document.getElementById('box1').innerHTML = hy
   //以下代码最为关键
   if (module.hot) {
       module.hot.accept()
   }
   ```

   #### 5. loader

   webpack只支持js，要解析css需要配置loader

   安装：

```javascript
npm i style-loader css-loader less less-loader stylus stylus-loader node-sass sass-loader -D 
```

配置webpack.config.js

```javascript
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
module.exports = {
    entry: './src/index.js', //入口
    output: {
        filename: 'build[hash:8].js', //修改打包文件名
        path: path.resolve('./build') //修改打包文件夹的名字,这个路径必须是绝对路径
    }, //出口
    devServer: {
        contentBase: './build',
        port: 5000,
        compress: true, //服务器压缩
        hot: true,
        open: true //自动打开浏览器
    }, //开发服务器
    module: {
        //1. 配置module
        rules: [{
                test: /\.css$/,
                use: [{
                    loader: 'style-loader'
                }, {
                    loader: 'css-loader'
                }]
            },
            {
                test: /\.less$/,
                use: [{
                        loader: 'style-loader'
                    }, {
                        loader: 'css-loader'
                    },
                    {
                        loader: 'less-loader'
                    }
                ]
            }
        ]
    }, //模块设置
    plugins: [
        //热更新
        new webpack.HotModuleReplacementPlugin(),
        //清空生产的文件夹 清除多个可以是数组        
        new CleanWebpackPlugin(['./build']),
        //打包html文件
        new HtmlWebpackPlugin({
            template: './src/index.html',
            title: '首页练习',
            hash: true //清缓存
                // minify: {
                //     removeAttributeQuotes: true, //去掉双引号
                //     collapseWhitespace: true //折叠一行

            // }

        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}
```

index.js

```javascript
import './index.css'
import './style.less'
```

