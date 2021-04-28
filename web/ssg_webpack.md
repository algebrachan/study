### Webpack5 入门到精通

#### Webpack简介

##### 1.webpack是静态模块打包器

##### 2.webpack的核心概念

- Entry：打包入口文件
- Output：打包资源输出文件，如何命名
- Loader：处理非js文件
- Plugins：执行范围更广的任务
- Mode：
  - development
  - production

#### Webpack配置文件

- 全局安装: `npm i webpack webpack-cli -g`
- 注意事项：
  - webpack原生只能打包js文件和json文件，不能打包css/img文件
  - 生产环境和开发环境将ES6模块化编译成浏览器能识别的模块化~
  - 生产环境比开发环境多一个压缩js代码

```javascript
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'built.js',
    path: resolve(__dirname, 'build'),
    publicPath:"./"
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader','css-loader']
      },
      {
        test: /\.(jpg|png|gif)$/,
        loader: 'url-loader',
        options: {
          limit: 8 * 1024,
          esModule: false,
          name: '[hash:10].[ext]'
        }
      },
      {
        test: /\.html$/,
        loader: 'html-loader'
      },
        {
        //处理其他资源
        exclude:/\.(html|js|css|less|jpg|png|gif)/,
        loader:'file-loader',
        options:{
          name:'[hash:10].[ext]'
        }
      }

    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ],
  mode:'development',
  // mode: 'production',
}
```

- 安装loader

  - `npm i css-loader style-loader less-loader -D`
  - `npm i html-webpack-plugin -D`
  - `npm i url-loader file-loader -D`
  - `npm i html-loader -D`

- devServer: `npm i webpack-dev-server -D` 

  - `npx webpack-dev-server`

  ```javascript
  // 开发服务器
  devServer:{
      contentBase:resolve(__dirname,'build'),
      // 启动gzip
      compress:true,
      port:3000
    }
  ```

- 提取css单独文件

  - `npm i mini-css-extract-plugin -D`

  ```javascript
  const MiniCssExtractPlugin = require('mini-css-extract-plugin')
  {
  	// 匹配哪些文件
  	test: /\.css$/,
  	use: [
  		MiniCssExtractPlugin.loader,
  		'css-loader'
  	]
  },
  plugins: [
  	new MiniCssExtractPlugin({
  		filename:'css/main.css'
  	})
  ],
  ```

  - css样式兼容性问题 在 package.json中配置browerslist
  - 压缩css `npm i -D optimize-css-assets-webpack-plugin`

- js语法检查eslint

  ```json
  module:{
  	rules:[
          {
              /*
              	package.json 中 eslintConfig中设置
              	"eslintConfig":{
              		"extends":"airbnb-base"
              	}
              	airbnb --> eslint-config-airbnb-base eslint-plugin-import eslint
              */
  
              test:/\.js$/,
              exclude:/node_modules/,
              loader:'exlint-loader',
              options:{
                  
              }
          }
      ]
  }
  // js兼容性
  {
      test:/\.js$/,
  	exclude:/node_modules/,
  	loader:'babel-loader',
  	options:{
  		presets:[
              [
                  '@babel/preset-env',
                  {
                      // 按需加载
                      useBuiltIns:'usage',
                      // 指定core-js
                      corejs:{
                          version:3
                      }
                      // 指定兼容性做到哪个版本浏览器
                      targets:{
                      	chrome:'60',
                      	firefox:'60'
                  	}
                  }
              ]
          ]
  	}
  }
  ```

#### Webpack生产环境配置

- source-map

  ```json
  devtool:'inline-source-map'
  ```

- oneOf: 以下loader只执行一个

- 缓存

  - babel缓存 cacheDirectory:true
  - 文件资源缓存
    - hash: 每次webpack会生成唯一的hash值
    - chunkhash: 来自同一个chunk，hash值一样
    - contenthash: 不同文件hash值一定不一样

- tree shaking：去除无用代码

  - package.json中配置 "sideEffects":false 

#### webpack详细配置

- entry

  ```json
  /*
  	entry: 入口起点 单入口
  	1.string --> './src/index.js'
  	 打包形成一个chunk 输出一个bundle文件
  	 此时chunk的名称默认是 main
  	2.array --> ['./src/index.js','./src/add.js'] 多入口
  	 所有文件形成一个chunk 输出一个bundle
  	3.object
  	多入口
  	有几个 入口文件，生成几个 chunk 输出 几个bundle文件
  */
  entry:{
      index:['./src/index.js','./src/test.js']
      main:'./src/main.js'
  }
  ```

- output

  ```json
  output:{
      // 文件名称 （指定名称+目录）
      filename:'js/[name].js',
      // 输出文件目录 所有资源输出的公共目录
      path:resolve(__dirname,'build')
      // 所有资源引入公共路径的前缀 
      publicPath:'/',
      chunkFilename:'[name]_chunk.js', //非入口chunk的名称 
      library:'[name]', // 整个库向外暴露的变量名
      libraryTarget:'window', // 变量名暴露到 browser
      // libraryTarget:'global' // 变量名暴露到 node
      // libraryTarget:'commonjs',
  }
  ```

- module

  ```json
  module:{
      rules:[
          // loader配置
          {
  			test:/\.css$/,
              exclude: /node_modules/, // 排除一些文件
              include: resolve(__dirname,'src'), //指定某个文件夹下面 
              use:['style-loader','css-loader'],
              enforce:'pre',// 优先执行
         	},
          {
  			oneOf:[]// 只生效一个
          }
      ]
  }
  ```

- resolve

  ```json
  // 解析模块的规则
  resolve:{
      // 配置解析模块路径别名
      alias:{
          $css:resolve(__dirname,'src/css')
      },
      // 配置省略文件后缀
      extensions:['.js','.json','.jsx','.css'],
      // 告诉webpack 解析模块是去哪个目录找
      modules:[resolve(__dirname,'../../node_modules','node_modules')]
  }
  ```

- devServer

  ```json
  devServer: {
  	contentBase: resolve(__dirname, 'build'),
      // 监视 contentBase 目录下的所有文件，一旦变化就会reload
      watchContentBase:true,
      watchOptions:{
          ignored:/node_modules/
      }
  	// 启动gzip
      compress: true,
      port: 3000,
      host:'localhost',
      open: true,
      // 开启HMR功能
      hot: true,
      // 不要显示启动服务器的日志信息
      clientLogLevel:'none',
      // 除了一些基本信息 其他内容不显示
      quiet:true,
      // 错误不要全屏提示
      overlay:false,
      proxy:{
      	// 服务器代理 解决开发环境跨域问题
      	'/api':{
      		target:'http://localhost:3000',
      		pathRewrite:{
      			'^/api':'',// 路径重写
  			}
  		}
  	}
  }
  ```

- optimization

  ```json
  optimization:{
      splitChunks:{
          chunks:'all',
          minSize: 30 * 1024,
          maxSize:0,
          minChunks:1,
          maxAsyncRequests:5,
          maxInitialRequests:3,
          automaticNameDelimiter:'~',
          name:true
      }
  }
  ```





