---
title: webpack3.0升级4.0爬坑记
date: 2018-03-28 14:41:20
tags: webpack
categories: 前端工程
---
# 老司机:taxi:, 等等我!
>### webpack官方2018年2月25日发布了4.0.0版本
<!--more-->
>>![webpack-4.0.0](/images/webpack4/release4.png)
>### 写这篇blog时, webpack已升级到4.6.0版本, 简直不要太快:rocket:, 下载量也是惊人,  webpack社区真是太强大了:heart_eyes:, 我大前端真是从入门到入门啊:joy:
>>![npm-webpack](/images/webpack4/npm.jpg)
# 开始愉快的爬坑之旅吧:turtle:
>克隆我的webapck仓库
```bash
$ git clone https://github.com/atbulbs/webpack-learning.git
```
>切换到webpack3目录
```bash
$ cd webpack3
```
>此时package.json文件里有很多依赖需要更新, 此时, 推荐一个神器 npm-check-updates, 它能自动把我们的依赖升级到最新版本
1. 全局安装
>```bash
$ npm i -g npm-check-updates
```
2. 检查依赖的最新版本, 可以看到每个依赖的当前版本对应的最新版本
>```bash
$ ncu
```
3. 更新依赖到最新版本
>```bash
$ ncu -u
```
4. 安装依赖
>```bash
$ npm i
```
5. 运行
>```bash
$ npm run dev
```
> 此时webpack会提醒我们新版本的需要安装独立的webpack-cli
![webpack-cli](/images/webpack4/webpack-cli.png)
>安装webpack-cli, 再跑起来试试
>```bash
$ npm i webpack-cli
$ npm run dev
```
>提醒我们按照官方推荐设置mode
![mode](/images/webpack4/mode.png)
>访问webpack官网
![webpack-mode](/images/webpack4/webpack-mode.jpg)
>按照官网设置mode, 我们给/build/webpack.config.base.js里的config对象添加mode属性
>```javascript
mode: process.env.NODE_ENV || 'production'
```
# talk is cheap, show you my code
>## 基本配置
```javascript
/**
 * webpack4/build/webpack.config.base.js
 */

// npm install -g npm-check-updates
// 检查依赖包的最新版本
// $ ncu
// 更新依赖包到最新版本
// $ ncu -u
// $ npm i
// $ npm i webpack-cli -D

const path = require('path')
const createVueLoaderOptions = require('./vue-loader.config')
// const isDev = process.env.NODE_ENV === 'development'

const config = {
  // 'development'会做开发时的优化
  // 'production'会做正式环境的优化,比如压缩
  // 优化会自动配置
  // mode 在4.0一定要加,3.xxx不能加
   mode: process.env.NODE_ENV || 'production',//只接收 'development' || 'production'
  // 编译目标运行的环境
  target: 'web',
  // 入口文件
  entry: path.join(__dirname, '../src/main.js'),
  // 出口文件
  output: {
    // hash是整个项目的hash
    // dev-server 不能使用chunkhash,不然会报错
    filename: 'bundle.[hash:8].js',
    path: path.join(__dirname, '../dist')
  },
  // 模块的loader规则
  // 声明 appropriate 合适的 loader
  // webpack 只支持原生js,配置loader后可以import非js的模块
  module: {
    rules: [
      // 启用eslint
      // {
      //   // 让eslint-loader为前置loader,在babel-loader之前检查
      //   enforce: 'pre',
      //   test: /.(jsx?|vue)$/,
      //   loader: 'eslint-loader',
      //   include: path.join(__dirname, '../src'),
      //   exclude: /node_modules/
      // },
      {
        test: /\.vue$/,
        use: [
          {
            loader: 'vue-loader',
            options: createVueLoaderOptions()
          },
          {
            loader: 'iview-loader',
            options: {
              prefix: false
            }
          }
        ]
      },
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        },
        include: [
          path.resolve(__dirname, '../src')
          // 限定只在 src 目录下的 js/jsx 文件需要经 babel-loader 处理
          // 通常我们需要 loader 处理的文件都是存放在 src 目录
          // 缩小loader应用的文件范围,提高构建速度
        ]
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader'
          },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      },
      {
        test: /\.styl$/,
        use: [
          // 用了vue-style-loader才支持css热更替
          'vue-style-loader',
          'css-loader',
          {
            loader: 'postcss-loader',
            options: {
              sourceMap: true
            }
          },
          'stylus-loader'
        ]
      },
      {
        test: /\.less$/,
        use: [
          'vue-style-loader',
          'css-loader',
          {
            loader: 'postcss-loader',
            options: {
              sourceMap: true
            }
          },
          'less-loader'
        ]
      },
      {
        test: /\.(jpg|png|jpeg|gif|svg)$/,
        use: [
          {
            // 依赖于file-loader(读取操作文件)
            loader: 'url-loader',
            options: {
              // 文件小于10000b时压缩成base64
              limit: 10000,
              // name: 文件原名
              // ext: 原文件扩展名
              // resources: 自定义文件夹名
              // path: 原路径
              // name: 'resources/[path][name].[hash:8].[ext]'
              name: 'static/imgs/[name].[hash:8].[ext]'
            }
          }
        ]
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          // name: 'resources/[path][name].[hash:8].[ext]'
          name: 'static/fonts/[name].[hash:8].[ext]'
        }
      }
    ]
  },
  resolve: {
    // 从前到后自动匹配扩展名
    extensions: ['.js', '.vue', '.json', '.jsx'],
    // 别名,方便引入时使用
    alias: {
      '@': path.join(__dirname, '../src'),
      // 'styles': path.join(__dirname, '../src/assets/styles'),
      'views': path.join(__dirname, '../src/views')
    }
  }
}

module.exports = config

```
>## 配置
```javascript
/**
 * webpack4/build/webpack.config.js
 */

const path = require('path')
const webpack = require('webpack')
// 合并webpack配置
const merge = require('webpack-merge')
// 提取css代码,生产环境使用外部样式,并可单独缓存
// 不支持webpack4.0
// const ExtractTextPlugin = require('extract-text-webpack-plugin')
// 渲染的html模板x
const HTMLPlugin = require('html-webpack-plugin')
// 压缩js代码,webpack4.0自带
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
const baseConfig = require('./webpack.config.base')
// https://github.com/webpack-contrib/mini-css-extract-plugin
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin')

const isDev = process.env.NODE_ENV === 'development'

const defaultPlugins = [
  // 注册全局环境变量,提供给webpack,vue和我们自己使用
  new webpack.DefinePlugin({
    'process.env': {
      // 注意双引号,否则webpack转义代码会解析成变量,调用时会报错
      // webpack判断process.env去选择不同版本的源码去打包
      // 比如开发版源码(有错误信息提示),生产版源码
      NODE_ENV: isDev ? '"development"' : '"production"'
    }
  }),
  new HTMLPlugin({
    template: path.join(__dirname, './template.html'),
    minify: { // 压缩 HTML 的配置
      minifyCSS: true, // 压缩 HTML 中出现的 CSS 代码
      minifyJS: true // 压缩 HTML 中出现的 JS 代码
    },
    chunksSortMode: 'none'
  })
]

const devServer = {
  // 可用localhost,127.0.0.1,以及内网IP访问
  // 但不能用0.0.0.0:8000打开
  host: '0.0.0.0',
  port: 8000,
  headers: {
    'Access-Control-Allow-Origins': '*'
  },
  // 代理
  proxy: {
    '/api': {
      target: 'http://10.130.212.170',
      changeOrigin: true,
      pathRewrite: {
        '^/api': '/'
      }
    }
  },
  overlay: {
    // 出现的错误显示到页面
    errors: true
  },
  // 处理url和页面的映射
  historyApiFallback: {
    index: '/dist/index.html'
  },
  // hotModuleReplacement
  // 修改代码后页面无刷新更新,不会丢失数据
  hot: true,
  // 启动时自动打开浏览器
  // 但是改webpack配置后也会打开新页面
  open: true
}

let config

if (isDev) {
  config = merge(baseConfig, {
    // 映射源码和编译后的代码,方便在浏览器调试代码
    // source-map是最完整的映射代码,但是效率低,文件大,编译和调试慢
    // eval-source-map会导致代码对不齐
    // 官方推荐cheap-module-eval-source-map
    // https://webpack.js.org/configuration/devtool/#development
    devtool: 'cheap-module-eval-source-map',
    devServer,
    plugins: defaultPlugins.concat([
      // 启动hotModuleReplacement功能
      new webpack.HotModuleReplacementPlugin()
      // 减少不需要的信息展示
      // webpack4.0不需要
      // new webpack.NoEmitOnErrorsPlugin()
    ])
  })
} else {
  config = merge(baseConfig, {
    entry: {
      app: path.join(__dirname, '../src/main.js')
      // 把第三方类库源码单独打包,利用长缓存,提高加载速度
      // webpack4.0不需要
      // vendor: [
      //   'vue', 'vue-router', 'vuex', 'axios', 'iview',
      //   'echarts', 'js-cookie', 'js-file-download'
      // ]
    },
    output: {
      // 使用chunkhash,每个文件会单独生成一个hash
      // 生产模式必须用chunkhash,不然vendor的哈希也会变
      // 就没有vendor的意义了
      filename: '[name].[chunkhash:8].js',
      path: path.join(__dirname, '../dist')
    },
    module: {
      rules: [
        {
          test: /\.styl$/,
          use: [
            MiniCssExtractPlugin.loader,
            {
              loader: 'css-loader',
              options: {
                minimize: true
              }
            },
            {
              loader: 'postcss-loader',
              options: {
                sourceMap: true
              }
            },
            'stylus-loader'
          ]
        },
        {
          test: /\.less$/,
          use: [
            MiniCssExtractPlugin.loader,
            {
              loader: 'css-loader',
              options: {
                minimize: true
              }
            },
            {
              loader: 'postcss-loader',
              options: {
                sourceMap: true
              }
            },
            'less-loader'
          ]
        },
        {
          test: /\.css$/,
          use: [
            MiniCssExtractPlugin.loader,
            {
              loader: 'css-loader',
              options: {
                minimize: true
              }
            }
          ]
        }
        // {
        //   test: /.css$/,
        //   use: ExtractTextPlugin.extract({
        //     fallback: 'style-loader',
        //     use: [
        //       {
        //         loader: 'css-loader',
        //         options: {
        //           minimize: true
        //         }
        //       }
        //     ]
        //   })
        // }
      ]
    },
    // webpack4.0新增字段
    // optimization 优化,SEO里的O
    optimization: {
      minimizer: [
        new UglifyJsPlugin({
          cache: true,
          parallel: true,
          sourceMap: true
        }),
        new OptimizeCSSAssetsPlugin({}) // use OptimizeCSSAssetsPlugin
      ]
    },
    plugins: defaultPlugins.concat([
      new MiniCssExtractPlugin({
        filename: '[name].[chunkhash:8].css',
        chunkFilename: '[id].css'
      }),
      // 通过css文件内容进行哈希生成的文件名
      // new ExtractTextPlugin('styles.[hash:8].css'),
      // new ExtractTextPlugin('styles.[contentHash:8].css'),
      // 单独打包
      // webpack4.0移除
      // new webpack.optimize.CommonsChunkPlugin({
      //   name: 'vendor'
      // }),
      // 优化
      // 把与webpack相关的代码单独打包到一个文件
      // 避免有新的模块加入时,webpack会给模块加一个id
      // 插入的顺序在中间时,会导致后面每个模块的id发生变化
      // 会导致打包出的内容的hash会变化,就不能使用长缓存
      // 注意vendor要放在runtime的前面
      // new webpack.optimize.CommonsChunkPlugin({
      //   name: 'runtime'
      // }),
      new webpack.NamedChunksPlugin()
      // new UglifyJsPlugin()
    ])
  })
}

module.exports = config

```