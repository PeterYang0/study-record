#### 常见的plugin以及作用

- html-webpack-plugin:(打包html文件)自动生成一个index.html文件（可以指定模板）将打包的js文件，自动通过script标签插入到body中

- webpack-dev-server: 开启一个开发服务器，帮助我监视文件变动，做到按需更新页面

- webpack-merge: (webpack.config.js抽离合并) 在webpack.config.js里面内容太多的时候进行分割，分为prod（生产环境），dev（开发环境），base（基本），然后在webpack.json中的“bulid”“dev”脚本后面增加 "–config ./build/prod.config.js"指定对应的webpackconfig。

- uglifyjs-webpack-plugin 压缩和混淆代码，不支持es6压缩。

- terser-webpack-plugin 压缩和混淆代码，支持es6压缩。

- webpack-parallel-uglify-plugin 多进程执行代码压缩，提升构建速度。

- mini-css-extract-plugin 分离样式文件，css提取为独立文件，支持按需加载。

- serviceworker-webpack-plugin 为网页添加离线缓存功能。

- clean-webpack-plugin 打包前清空目录

- speed-measure-webpack-plugin 展示loader和plugin运行的速度。

- html-webpack-plugin 生成html文件，自动引用css和js文件。

- extract-text-webpack-plugin 将js文件中引用的样式单例抽离成css。

- DefinePlugin 定义全局变量，对开发和发布模式的构建允许不同的行为。

- HotModuleReplacementPlugin 热加载

- DllPlugin、DLLReferencePlugin 相互配合，前者第三方包的构建，值构建业务代码。

- optimize-css-assets-webpack-plugin 优化css，去掉重复的css。

- webpack-bundle-analyzer bundle文件分析公工具。

- compression-webpack-plugin 生产环境可采用gzip压缩js和css。

- happypack 通过多进程模型，来加速代码构建。