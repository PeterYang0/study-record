## webpack 基础配置学习笔记

## webpack 安装

- yarn add webpcak webpack-cli //安装 webpack4.X 还需要一个需要 webpack-cli
- webpack.config.js
- npx webpack 局部执行 webpack，解决了多版本 webpack 共存

## webpack.config.js

#### 文件输出配置

```
npx webpack --config webpack.config.js //可以更换默认配置文件
```

```
const path =require('path'); //引入path模块
path.resolve(__dirname,'dist'); //__dirname根目录，配置绝对路径 输出文件目录
```

```
mode: 'development',//设置环境'production'/'development'
devtool: 'source-map', // 源码映射直接显示源码错误位置
// build=> 'source-map'打包速度-- 生成app.js.map映射文件 'inline-source-map' 以dataurl的方式直接写在app.js
```

```
"scripts": {
    "build": "webpack" //优先执行工程下webpack版本，类似npx
  },
```

#### 模块配置

```
module:{
  rules:[
    {},{},{} //处理不同类型文件
  ]
}
```

#### 核心文件配置

1. 处理图片或字体

- 安装 file-loader
  ```
  npm install --save-dev file-loader
  ```
- 配置
  ```
  module: {
      rules: [
        {
          test: /\.(eot|ttf|svg)$/,
          use: [
            {
              loader: 'file-loader',
              options: {
                name: 'static/[name].[ext]',// 自定义文件name
              }
            }
          ]
        }
      ]
    }
  ```
- 安装 url-loader
  ```
  npm install url-loader --save-dev
  ```
- 配置(默认转换成 base64 形式打包到 js 中)
  ```
  rules: [
    {
      test: /\.(png|jpg|gif)$/i,
      use: [
        {
          loader: 'url-loader',
          options: {
            limit: 8192 //设置大小(b)，超出则打包成图片文件，没超过就转换成base64字符串加载到js中
          }
        }
      ]
    }
  ]
  ```

2. 处理 vue 文件

- 安装--将 vue-loader 和 vue-template-compiler 一起安装
  ```
  npm install -D vue-loader vue-template-compiler
  ```
- 配置
  ```
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: ['vue-loader']
      }
    ]
  },
  plugins: [
    // 请确保引入这个插件！
    new VueLoaderPlugin()
  ]
  ```

3. 处理 css 文件

- 安装
  ```
  yarn add style-loader css-loader -D
  ```
- 配置
  ```
  {
    test: /\.css$/,
    use: [
      {
      loader: 'css-loader',
      options: {
        modules: true, // 开启 css-module
      }
    ]
  ```

4. 处理 less 文件

- 安装
  ```
  yarn add less-loader less -D
  ```
- 配置
  ```
  {
    test: /\.less$/,
    use: ['style-loader', 'css-loader', 'less-loader'] // loader执行顺序是从下到上，从左到右
  }
  ```

5. 处理 sass 文件

- 安装
  ```
  yarn add sass-loader node-sass -D
  ```
- 配置
  ```
  {
    test: /\.scss$/,
    use: ['style-loader', 'css-loader', 'sass-loader'] // loader执行顺序是从下到上，从左到右
  },
  ```

6. 抽离 css

```
const ExtractTextPlugin = require("extract-text-webpack-plugin");
{//配置css
      test: /\.css$/,
      use: ExtractTextPlugin.extract({
  			fallback: "style-loader",
  			use: "css-loader"
      })
    },
//处理css文件
new ExtractTextPlugin("css/[name].css"),
```

7. 编译 es6
   babel-loader @babel/core 还要@babel/preset-env

```
{
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: "babel-loader",
        options: {
          presets: ["@babel/preset-env"]
        }
      }
    },

    import "@babel/polyfill"; // 打包变大，=》变量实现

    presets: [["@babel/preset-env", {
              "useBuiltIns": "usage"
            }]] // 只补充当前使用
```

8. 打包react   @babel/preset-react

```
presets: ["@babel/preset-env", "@babel/preset-react"]
```

#### plugins

1. HtmlWebpackPlugin

- yarn add html-webpack-plugin
- 配置

```
const HtmlWebpackPlugin = require('html-webpack-plugin');
const path = require('path');

module.exports = {
  entry: 'index.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: 'index_bundle.js'
  },
  plugins: [new HtmlWebpackPlugin()]
};
```

2. CleanWebpackPlugin

- yarn add clean-webpack-plugin
- 配置

```
new CleanWebpackPlugin(['dist'], { //第一个是填写要删除的路径，格式是个数组 //第二个参数是对象类型的配置
    root: path.resolve(__dirname, '..'), // root：绝对路径，就是要根据这个root去找要删除的文件夹
    dry: false // 启用删除文件 //为false是删除文件夹的，为true是不删除的，默认值是false
  }),
```

3. HtmlWebpackPlugin

```
const HtmlWebpackPlugin = require("html-webpack-plugin");
new HtmlWebpackPlugin({
      template: "./src/app.html",
      filename: "index.html",
      title: "webpack",
      inject: true, // true || 'head' || 'body' || false将所有资产注入给定template或templateContent。传递true或'body'所有javascript资源将被放置在body元素的底部。'head'将脚本放在head元素中
      hash: true,
      chunks: ["commons", name], // 允许您仅添加一些块（例如，仅添加单元测试块）
      // favicon: "./favicon.ico"
    }),
```

### webpack-dev-server

1. webpack --watch

2. webpack-dev-server

```
devServer: {
  contentBase: './dist',
  port: 8008,
  open: true,
  proxy: {
      "/weatherApi": {
        target: "https://www.apiopen.top",
        changeOrigin: true
      }
    },
  hot: true, // HMR热更新
  hotOnly: true,

  if (module.hot) {
    module.hot.accept('', function(){

    })
  }
}
```

### 代码压缩
```
  optimization: {
    minimize: true, // 压缩
    usedExport: true, // 只打包export Tree shaking
  },
```

### config合并

```
const merge=require('webpack-merge');
const commonConfig={}
const devConfig={}
module.exports= merge(commonConfig, devConfig);
```

### 配置引用路径
```
resolve:{
  	alias:{
  		style: path.resolve(__dirname, './src/static/style'),
  	}
  },
```