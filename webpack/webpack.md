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


//当package.json里面的scripts字段值里，添加三个字段，其中字段名可以根据自己的喜好来指定
scripts": {
    "dev": "./node_modules/.bin/webpack --mode development",
    "build": "./node_modules/.bin/webpack  --mode product",
    "des": "./node_modules/.bin/webpack-dev-server"
  }
//然后就可以使用 npm run dev 和 npm run build 命令，即可分别打包为：开发环境的项目文件，和生产环境的项目文件

//通常开发，都会使用 webpack-dev-server 插件， 这时运行 npm run des 进行开发更方便。
  
```

终端运行

```javascript
npm run start  开发 
npm run build  生产
```

5、webpack 插件

1. **HtmlWebpackPlugin**  该插件将为你生成一个HTML5文件

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

2. **CleanWebpackPlugin** 清空生产生成的文件（build）

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

   webpack只支持js，要解析css需要配置loader，配置完加在style标签中

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

**抽离样式，抽离到css文件，通过css文件引入**

插件：**extract-text-webpack-plugin@next**      **mini-css-extract-plugin**（不能分别抽离，只能抽离成一个文件） 推荐使用**extract-text-webpack-plugin@next**

extract-text-webpack-plugin    webpack3.0使用

extract-text-webpack-plugin@next    webpack4.0使用

安装

```javascript
npm i extract-text-webpack-plugin@next mini-css-extract-plugin -D
```

**extract-text-webpack-plugin@next  **插件配置webpack.config.js：

```javascript
//把css,less等打包到一个css文件中

let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
//1. 引入插件
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
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
        //2. 配置插件
        rules: [{
                test: /\.css$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                        loader: 'css-loader'
                    }]
                })
            },
            {
                test: /\.less$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        {
                            loader: 'less-loader'
                        }
                    ]
                })
            }
        ]
    }, //模块设置
    plugins: [
        //3. 配置抽离css样式插件
        new ExtractTextWebpackPlugin({
            filename: 'css/index.css'
        }),
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

```javascript
//将css，less等分别打包到对应的样式文件

let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
//1. 引插件，new实例
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
let lessExtract = new ExtractTextWebpackPlugin('css/less.css')
let cssExtract = new ExtractTextWebpackPlugin('css/css.css')
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
        //2. 配置插件
        rules: [{
                test: /\.css$/,
                use: cssExtract.extract({
                    use: [{
                        loader: 'css-loader'
                    }]
                })
            },
            {
                test: /\.less$/,
                use: lessExtract.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        {
                            loader: 'less-loader'
                        }
                    ]
                })
            }
        ]
    }, //模块设置
    plugins: [
        //3. 抽离css样式
        lessExtract,
        cssExtract,
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

//以上设置有一个问题，就是分别把css，less打包，通过link标签引入，任意修改之后不能实现热刷新。还得强制刷新。在开发过程中可以通过把css，less打包成style标签中实现热刷新，等项目上线了，再换成link标签
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
    //let MiniCssEtractPlugin = require('mini-css-extract-plugin')
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
//1. 实例化并传入参数
let lessExtract = new ExtractTextWebpackPlugin({
    filename: 'css/less.css',
    disable: true
})
let cssExtract = new ExtractTextWebpackPlugin({
    filename: 'css/css.css',
    disable: true
})
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
        //2. 配置，增加fallback: 'style-loader',
        rules: [{
                    test: /\.css$/,
                    use: cssExtract.extract({
                        fallback: 'style-loader',
                        use: [{
                            loader: 'css-loader'
                        }]
                    })
                },
                {
                    test: /\.less$/,
                    use: lessExtract.extract({
                        fallback: 'style-loader',
                        use: [{
                                loader: 'css-loader'
                            },
                            {
                                loader: 'less-loader'
                            }
                        ]
                    })
                }
            ]
            
    }, //模块设置
    plugins: [
        //3. 抽离css样式
        lessExtract,
        cssExtract,        
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
var hy = require('./a.js')
//用import 引入css ，less 文件
import './index.css'
import './style.less'

console.log(hy)
document.getElementById('box1').innerHTML = hy
if (module.hot) {
    module.hot.accept()
}
```

  **mini-css-extract-plugin **插件配置webpack.config.js

```javascript                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
//1. 引插件
let MiniCssEtractPlugin = require('mini-css-extract-plugin')   
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
        //2. 配置
        rules: [{
                test: /\.css$/,
                use: [
                    MiniCssEtractPlugin.loader,
                    {
                        loader: 'css-loader'
                    }
                ]

            },
            {
                test: /\.less$/,
                use: [
                    MiniCssEtractPlugin.loader,
                    {
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
        //3. 抽离css样式        
        new MiniCssEtractPlugin({
            filename: 'css/index.css'
        }),
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

去掉css中冗余的样式代码：

插件   **purifycss-webpack **

安装：purifycss-webpack插件内置会调用purify-css插件， 搜索的时候引用glob插件

```javascript
npm i purifycss-webpack purify-css glob -D
```

webpack.config.js配置

```javascript
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')   
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
 //1. 引入插件
let glob = require('glob')
let PurifyCssPlugin = require('purifycss-webpack')
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
        //配置插件
        rules: [{
                test: /\.css$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                        loader: 'css-loader'
                    }]
                })
            },
            {
                test: /\.less$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        {
                            loader: 'less-loader'
                        }
                    ]
                })
            }
        ]
    }, //模块设置
    plugins: [
        //配置抽离css样式插件
        new ExtractTextWebpackPlugin({
            filename: 'css/css.css'
        }),
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

        }),
        //2. 配置插件glob.sync同步提交  //一定放在HtmlWebpackPlugin之后
        new PurifyCssPlugin({
            paths: glob.sync(path.resolve('src/*.html'))
        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}
```

自动给css样式加前缀，比如css动画

插件：**postcss-loader **   **autoprefixer**

安装：

```javascript
npm i postcss-loader autoprefixer -D
```

新建 postcss.config.js

```javascript
module.exports = {
    plugins: [
        require('autoprefixer')
    ]
}
```

webpack.config.js

```javascript
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
let glob = require('glob')
let PurifyCssPlugin = require('purifycss-webpack')
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
        //1. 配置插件 增加 loader: 'postcss-loader'
        rules: [{
                test: /\.css$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        { loader: 'postcss-loader' }
                    ]
                })
            },
            {
                test: /\.less$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        {
                            loader: 'less-loader'
                        },
                        {
                            loader: 'postcss-loader'
                        }
                    ]
                })
            }
        ]
    }, //模块设置
    plugins: [
        // 配置抽离css样式插件
        new ExtractTextWebpackPlugin({
            filename: 'css/css.css'
        }),
        //热更新
        new webpack.HotModuleReplacementPlugin(),
        //清空生产的文件夹 清除多个可以是数组        
        new CleanWebpackPlugin(['./build']),
        //打包html文件
        new HtmlWebpackPlugin({
            template: './src/index.html',
            title: '首页练习',
            hash: true //清缓存
        }),
        new PurifyCssPlugin({
            paths: glob.sync(path.resolve('src/*.html'))
        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}
```

css, less文件

```javascript
body{
  #box1{
    color: yellow;
    font-size: 25px;
    border:2px solid #ddd;
    transform: translate(45deg)
  }
}
```

#### 6.  直接把src目录下的文件拷贝到生产中，不需要打包(实际操作用到很少)

插件   **copy-webpack-plugin**

安装:

```javascript
npm i copy-webpack-plugin -D
```

配置webpack-config.js

```javascript
let path = require('path')
let HtmlWebpackPlugin = require('html-webpack-plugin')
let CleanWebpackPlugin = require('clean-webpack-plugin')
let webpack = require('webpack')
let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
    //1. 引插件
let CopyWebpackPlugin = require('copy-webpack-plugin')

let glob = require('glob')
let PurifyCssPlugin = require('purifycss-webpack')
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
        rules: [{
                test: /\.css$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        { loader: 'postcss-loader' }
                    ]
                })
            },
            {
                test: /\.less$/,
                use: ExtractTextWebpackPlugin.extract({
                    use: [{
                            loader: 'css-loader'
                        },
                        {
                            loader: 'less-loader'
                        },
                        {
                            loader: 'postcss-loader'
                        }
                    ]
                })
            }
        ]
    }, //模块设置
    plugins: [
        new ExtractTextWebpackPlugin({
            filename: 'css/css.css'
        }),
        //2. 直接拷贝不需要打包的文件  new CopyWebpackPlugin()里面必须是数组
        new CopyWebpackPlugin([{
            from: './src/doc',
            to: 'public'
        }]),
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

        }),
        new PurifyCssPlugin({
            paths: glob.sync(path.resolve('src/*.html'))
        })
    ], //插件配置
    mode: 'development', //可以更改模式 'development' 和'production'
    resolve: {} //配置解析

}
```

#### 7. 打包图片

在实际生产中img标签的src属性引用或者内嵌样式引用

1. HTML文件中img标签src属性引用或者内嵌样式引用

   ```javascript
   <img src="photo.jpg" />
   <div style="background:url(photo.jpg)"></div>
   ```

2. CSS文件中的背景图等设置 

   ```javascript
   .photo {
       background: url(photo.jpg);
   }
   ```

3. JavaScript文件中动态添加或者改变的图片引用 

   ```javascript
   var imgTempl = '<img src="photo.jpg" />';
   document.body.innerHTML = imgTempl;
   ```

4. ReactJS中图片的引用 

   ```javascript
   import React from 'react';
   import ReactDOM from 'react-dom';
   
   class App extends React.Component {
       render() {
           return (<img src='photo.jpg' />);
       }
   }
   
   ReactDom.render(<App />, document.querySelector('#container'));
   ```

   ##### url-loader

   在 webpack 中引入图片需要依赖 url-loader 这个加载器。 

   安装：

   ```javascript
   npm i url-loader -D
   ```

   webpack.config.js配置

   ```javascript
   let path = require('path')
   let HtmlWebpackPlugin = require('html-webpack-plugin')
   let CleanWebpackPlugin = require('clean-webpack-plugin')
   let webpack = require('webpack')
   let ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
   let CopyWebpackPlugin = require('copy-webpack-plugin')
   let glob = require('glob')
   let PurifyCssPlugin = require('purifycss-webpack')
   module.exports = {
       entry: './src/index.js', //入口
       output: {
           filename: '[name][hash:8].js', //修改打包文件名
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
           //1. 设置图片和html中引入图片的规则
           rules: [{
                   test: /\.css$/,
                   use: ExtractTextWebpackPlugin.extract({
                       use: [{
                               loader: 'css-loader'
                           },
                           { loader: 'postcss-loader' }
                       ]
                   })
               },
               {
                   test: /\.less$/,
                   use: ExtractTextWebpackPlugin.extract({
                       use: [{
                               loader: 'css-loader'
                           },
                           {
                               loader: 'less-loader'
                           },
                           {
                               loader: 'postcss-loader'
                           }
                       ]
                   })
               },
               {
                   test: /\.(png|jpg|gif|svg)$/,
                   use: [{
                       loader: 'url-loader',
                       options: {
                           limit: 5000,   //limit 字段代表图片打包限制，这个限制并不是说超过了就不能打包，而是指当图片大小小于限制时会自动转成 base64 码引用
                           outputPath: 'img/',
                           name: '[name].[ext]?[hash]' //通过 name 字段来指定图片打包的目录与文件名
                       }
                   }]
               },
               {
                   test: /\.html$/,
                   use: [{
                       loader: 'html-withimg-loader'
                   }]
               }
           ]
       }, //模块设置
       plugins: [
           new ExtractTextWebpackPlugin({
               filename: 'css/css.css'
           }),
           //直接拷贝不需要打包的文件
           new CopyWebpackPlugin([{
               from: './src/doc',
               to: 'public'
           }]),
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
   
           }),
           new PurifyCssPlugin({
               paths: glob.sync(path.resolve('src/*.html'))
           })
       ], //插件配置
       mode: 'development', //可以更改模式 'development' 和'production'
       resolve: {} //配置解析
   
   }
   ```

   注：output.publicPath 表示资源的发布地址，当配置过该属性后，打包文件中所有通过相对路径引用的资源都会被配置的路径所替换 

   该属性的好处在于当你配置了图片 CDN 的地址，本地开发时引用本地的图片资源，上线打包时就将资源全部指向 CDN 了。

   但是要注意，如果没有确定的发布地址不建议配置该属性，否则会让你打包后的资源路径很混乱。

js中的图片

```javascript
var imgUrl = require('./images/bg.jpg'),
    imgTempl = '<img src="'+imgUrl+'" />';
document.body.innerHTML = imgTempl;
```

react中的图片

```javascript
render() {
    return (<img src={require('./images/bg.jpg')} />);
}
```

html中的图片

由于 webpack 对 html 的处理不太好，打包 HTML 文件中的图片资源是相对来说最麻烦的。这里需要引用一个插件—— html-withimg-loder 

```javascript
npm i html-withimg-loader -D
```

webpack.config.js 添加配置 

```javascript
module: {
　　rules: [
　　　　{
　　　　　　test: /\.html$/,
           use:[{
            loader:'html-withimg-loader'
        }]
　　　　}
　　]
}
```



