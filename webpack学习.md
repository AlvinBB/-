# webpack打包工具使用
webpack可以进行代码分割，按需加载依赖，通过"loaders"可以处理CommonJS，AMD，ES6模块，CSS，图片，JSON，LESS或自定义文件等多种文件
## 概况
### 目标
- 切分依赖树，按需加载依赖
- 缩短初始化加载事件
- 任何静态资源都可以视为一个模块
- 可以整合第三方类库，视作模块处理
- 项目打包的每个部分都可以自定义
- 适合大型项目
### 特点
- 代码分割（Code Splitting）
- 加载（Loaders）
- 插件系统（Plugin System）
- 模块热更新
### 使用步骤
- 创建测试目录
- 进入文件夹，在文件夹下初始化npm，生成package.json
- npm安装webpack，参数--save-dev用于将依赖自动写入package.json文件中


``` bash
$ mkdir webpack-test
$ cd webpack-test
$ npm init
$ npm install webpack --save-dev
```
- 如果以上步骤执行后仍无法找到webpack命令，可尝试全局安装webpack

``` bash
$ npm install -g webpack
```
## webpack应用
### loader
- 可以使用命令行对js文件进行打包
``` bash
$ webpack test.js test.bundle.js
```
- 如果文件中引入了css等其他文件，则需要使用lodaer来处理引用，首先引入lodaer（以css-loader和style-loader为例），然后打包时设置参数

```javascript
    require('./world.js')   //引入的js文件
    require('./style.css')  //引入的css文件
        
    function hello(str){
        alert(str)
    }
        
    hello('hello world!')
```
``` bash
$ npm install css-loader style-loader --save-dev
$ webpack hello.js hello.bundle.js --module-bind 'css=style-loader!css-loader'
```
- 也可以在webpack.config.js中配置loader(必须先npm命令行安装相应loader才可以使用),loader模块参考https://www.npmjs.com/package/postcss-loader

```javascript
    const path = require('path')
    var htmlWebpackPlugin = require('html-webpack-plugin')
    
    module.exports ={
        entry:'./src/app.js',
        output:{
            path:path.resolve(__dirname, 'dist'),
            filename:'js/[name].bundle.js'
        },
        module:{
            loaders:[
                {
                    test:/\.js$/,
                    loader:'babel-loader',
                    exclude:path.resolve(__dirname,'node_modules/'),
                    include:path.resolve(__dirname,'src'),
                    query:{
                        presets:['latest']
                    }
                },
                {
                    test:/\.css$/,
                    loader:[
                        'style-loader',
                        'css-loader',
                        'postcss-loader'
                    ]
                },
                {
                    test:/\.less$/,
                    loader:'style-loader!css-loader!postcss-loader!less-loader'
                }
            ]
        },
        plugins:[
            new htmlWebpackPlugin({
                filename:'index.html',
                template:'index.html',
                inject:'body'
            })
        ]
    }
```


### webpack.config.js配置
- 在webpack.config.js中定义入口文件entry和输出文件output
    * entry
    
        1.值可以是字符串，表示只有一个入口文件
        
        2.值可以是数组，多个入口文件
        
        3.值可以是对象，多页面时可以使用，不同的页面配置不同入口（需要和output配合使用）
        
    * output
    
        1.当entry对象有多个值时，output可以使用[name]-[hash]占位符来生成多个文件
        
```javascript
    module.exports ={
        entry:'./src/script/main.js',
        output:{
            path:'dist/js',
            filename:'bundle.js'
        }
    }
```    
- 如果该配置内容在命令行运行webpack后报错，需要设置绝对路径，则解决办法：引入path模块，指定绝对路径

```javascript
    const path = require('path')
    
    module.exports ={
        entry:'./src/script/main.js',
        output:{
            path:path.resolve(__dirname, 'dist/js'),
            filename:'bundle.js'
        }
    }
```
- 可以在package.json中配置script（允许加参数，如打包进度，打包模块，字体颜色，打包原因等），则打包只需在命令行输入npm run webpack

```javascript
    "scripts": {
        "webpack":"webpack --config webpack.config.js --progress --display-modules --colors --display-reasons"
      }
 ```     
- 当多个页面配置webpack.config.js时，output使用[name]-[hash]占位符来生成多个文件，为避免手动修改index.html中对script标签引用的修改，可以使用html-webpack-plugin插件，自动生成script引用（该处若webpack为全局安装，该插件安装时会报Cannot find module 'webpack/lib/node/NodeTemplatePlugin'的错误，解决办法是在项目文件中再安装局部webpack并配置package.json）


```javascript
    //webpack.config.js
    const path = require('path')
    const htmlWebpackPlugin = require('html-webpack-plugin')
    
    module.exports ={
        entry:{
            main:'./src/script/main.js',
            main2:'./src/script/main2.js',
            publicPath:'http://domain.com'  //占位符，供上线使用，引用为绝对路径
        },
        output:{
            path:path.resolve(__dirname, 'dist'),
            filename:'js/[name]-[hash].js'
        },
        plugins:[
            new htmlWebpackPlugin({
                filename:'index-[hash].html',    //指定生成的文件名
                template:'index.html' ,  //配置模板文件，在模板基础上生成引用
                inject:'head', //指定脚本放置位置
                title:'webpack test', //对模板传参，需要用<%=  %>在模板中指定
                date:new Date() //指定的传参值
            }),
            new htmlWebpackPlugin({         //多个插件供多页面使用
                filename:'detail.html'  
            })
        ]
    }
```
```html
    /***************指定的模板index.html*************/
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title><%= htmlWebpackPlugin.options.title %></title>
    </head>
    <body>
        <%= htmlWebpackPlugin.options.date %>
        <script type="text/javascript" src="bundle.js"></script>
    </body>
    </html>

    /***************生成的index.html*************/
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>webpack is good</title>
    <script type="text/javascript" src="js/main2-831dfc2cd163bb3fec4b.js"></script><script type="text/javascript" src="js/main-831dfc2cd163bb3fec4b.js"></script></head>
    <body>
        Mon Jun 26 2017 23:07:35 GMT+0800 (CST)
        <script type="text/javascript" src="bundle.js"></script>
    </body>
    </html>
```    
